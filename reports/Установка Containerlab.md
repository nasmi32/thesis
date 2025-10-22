## Linux

Containerlab распространяется как Linux-пакет deb/rpm/apk для архитектур amd64 и arm64 и может быть установлен на любой дистрибутив Debian- или RHEL-подобного типа за несколько секунд.
Для успешной работы Containerlab необходимо заранее:
- Установить Docker или Linux сервер;
- Иметь права sudo для запуска Containerlab;
- Загрузить образы контейнеров (Containerlab попытается загрузить образы во время выполнения, если они не существуют локально).
Самый простой способ начать работу с Containerlab - использовать скрипт быстрой установки quick-setup.sh. Он написан на bash и автоматически определяет тип ОС (Debian, Ubuntu, RHEL, CentOS, Fedora, Rocky), после чего выполняет установку нужных пакетов и зависимостей.

В начале скрипт проверяет:
- что пользователь имеет права sudo (требуются для установки системных пакетов);
- что выполняется на поддерживаемой Linux-системе;
- наличие интернет-соединения для загрузки зависимостей.

Далее при необходимости устанавливается Docker, Containerlab и инструмент GitHub Cli (gh).

Чтобы установить все компоненты сразу, выполните следующую команду на любой из поддерживаемых ОС:

```
curl -sL https://containerlab.dev/setup | sudo -E bash -s "all"
```

По умолчанию это также настроит sshd в системе, чтобы неизвестные ключи не блокировали попытки подключения по SSH. Это поведение можно отключить, установив переменную окружения SETUP_SSHD в значение `false` перед запуском приведённой выше команды. Переменная окружения устанавливается и экспортируется следующей командой:

```
export SETUP_SSHD="false"
```

Чтобы завершить установку и включить выполнение команд `docker` без sudo, выполните `newgrp docker` или выйдите из системы и войдите снова.

Containerlab также настраивается для работы без sudo, и пользователь, выполняющий скрипт быстрой установки, автоматически получает доступ к привилегированным командам.

Чтобы установить отдельный компонент, укажите имя функции в качестве аргумента скрипта. Например, чтобы установить только docker:

```
curl -sL https://containerlab.dev/setup | sudo -E bash -s "install-docker"
```

Выйдите из текущей сессии пользователя и войдите снова, чтобы увидеть новое двухстрочное приглашение в действии:

```
[*]─[clab]─[~]
└──>
```

Если вам не надо устанавливать дополнительные инструменты, а необходим лишь один Containerlab можно воспользоваться скриптом установки.  Например, чтобы скачать и установить последнюю версию:

```
bash -c "$(curl -sL https://get.containerlab.dev)"
```

Также можно установить официальные выпуски containerlab через публичный репозиторий APT/YUM.
**APT**
```
echo "deb [trusted=yes] https://netdevops.fury.site/apt/ /" | \
sudo tee -a /etc/apt/sources.list.d/netdevops.list

sudo apt update && sudo apt install containerlab
```
**YUM**
```
sudo yum-config-manager --add-repo=https://netdevops.fury.site/yum/ && \ [](https://containerlab.dev/install/#__codelineno-9-2)echo "gpgcheck=0" | sudo tee -a /etc/yum.repos.d/netdevops.fury.site_yum_.repo [](https://containerlab.dev/install/#__codelineno-9-3)[](https://containerlab.dev/install/#__codelineno-9-4)sudo yum install containerlab
```

Пакетный установщик поместит бинарный файл containerlab в каталог `/usr/bin`, а также создаст символическую ссылку `/usr/bin/clab -> /usr/bin/containerlab`. Эта ссылка позволяет пользователям экономить на наборе команд при использовании containerlab: clab <command>. Containerlab также настраивается для работы без sudo, и текущий пользователь (даже если менеджер пакетов был вызван через sudo) автоматически получает доступ к привилегированным командам Containerlab.

## Установка с помощью контейнера

Containerlab также доступен в виде образа контейнера. Последнюю версию можно загрузить командой:

```
docker pull ghcr.io/srl-labs/clab
```

Чтобы выбрать любую из выпущенных версий начиная с **0.19.0**, используйте номер версии в качестве тега, например:

```
docker pull ghcr.io/srl-labs/clab:0.19.0
```

Загруженный контрейнер запускается командой:

```
docker run --rm -it --privileged \
    --network host \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /var/run/netns:/var/run/netns \
    -v /etc/hosts:/etc/hosts \
    -v /var/lib/docker/containers:/var/lib/docker/containers \
    --pid="host" \
    -v $(pwd):$(pwd) \
    -w $(pwd) \
    ghcr.io/srl-labs/clab bash
```

Рассмотрим основные требуемые привилегии:

- `--privileged`: полный доступ к хосту для создания и управления сетевым пространством;
- `--network host`: доступ к сетевому стеку хоста;
- `--pid="host"`: доступ к пространству процессов хоста.

В запущенном контейнере можно использовать те же команды **deploy/destroy/inspect** для управления лабораториями.
Можно развернуть лабораторию альтернативным способом без запуска оболочки, например:

```
docker run --rm -it --privileged \
# <параметры опущены>
-w $(pwd) \
ghcr.io/srl-labs/clab deploy -t somelab.clab.yml
```

## Ручная установка

Если дистрибутив Linux не поддерживает установку deb/rpm пакетов, containerlab можно установить из архива:

```
LATEST=$(curl -s https://github.com/srl-labs/containerlab/releases/latest | \
       sed -e 's/.*tag\/v\(.*\)\".*/\1/')
curl -L -o /tmp/clab.tar.gz "https://github.com/srl-labs/containerlab/releases/download/v${LATEST}/containerlab_${LATEST}_Linux_amd64.tar.gz"
mkdir -p /etc/containerlab
tar -zxvf /tmp/clab.tar.gz -C /etc/containerlab
mv /etc/containerlab/containerlab /usr/bin && chmod a+x /usr/bin/containerlab
```

## Обновление  

Чтобы обновить containerlab до последней доступной версии, выполните:

```
sudo -E containerlab version upgrade
```

Эта команда скачает скрипт установки и обновит инструмент до последней версии.
## С помощью исходников

Чтобы собрать containerlab из исходников есть два варианта:

- **go build**;
- **goreleaser**.
В первом случае, требуется склонировать репозиторий, перейти в корневой каталог и выполнить команду `go build`:

```
git clone https://github.com/srl-labs/containerlab
cd containerlab
go build
```
  
Во втором случае, для сборки запускается: 

```
goreleaser --snapshot --skip-publish --rm-dist
```
## Удаление
Отдельно рассмотрим удаление инструмента.
Чтобы удалить containerlab, установленный через скрипт или пакеты:

- **Для Debian-систем:**

```
apt remove containerlab
```

- **Для RPM-систем:**
```
yum remove containerlab
# or
dnf remove containerlab
```

## Windows

Для того, чтобы развернуть Containerlab на Windows, требуется использовать WSL ( [Windows Subsystem Linux](https://learn.microsoft.com/en-us/windows/wsl/)). WSL позволяет пользователям запускать легковесные Linux VM внутри Windows, это можно использовать для развертывания Containerlab.

Если WSL не установлен, исправить это можно одной командой:

```powershell
wsl --install
```

Далее расcмотрим установку Containerlab. В Powersher от имени администратора включаем компоненты Windows, необходимые для работы подсистемы Linux: активируем базовую функциональность WSL и платформу виртуальных машин для использования более производительной версии WSL 2:

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

После выполнения этих команд и перезагрузки компьютера, нужно с помощью команды `wsl --set-default-version 2` задать WSL 2 как версию по умолчанию.

Загрузим файл `.wsl` со [страницы релизов](https://github.com/srl-labs/wsl-containerlab/releases/tag/0.69.3-1.0). Просто дважды щелкнем по файлу, и дистрибутив будет установлен. Альтернативно его можно установить с помощью` wsl -install -from-file C:\path\to\clab.wsl`. Containerlab должен появиться как программа в меню "Пуск".
Запустим Containerlab:
```powershell
wsl -d Containerlab
```
При первом запуске Containerlab WSL выполняется начальный этап настройки, который помогает настроить дистрибутив.  Настраивается оболочка и командная строка.  После этого SSH-ключи будут скопированы или сгенерированы (если ключей не существует). Это необходимо для обеспечения доступа к SSH без пароля, что улучшает интеграцию с DevPod.

## macOS

Запуск containerlab на macOS возможен как на ARM (M1/M2/M3 и т.д.), так и на Intel-чипах. Долгое время существовали ограничения для ARM-чипов, но с появлением ARM64-совместимых сетевых ОС, таких как Nokia SR Linux и Arista cEOS, а также благодаря эмуляции Rosetta для x86_64-систем, теперь можно запускать containerlab и на Mac с ARM чипом.

Для работы Containerlab на macOS используется Linux-виртуальная среда, поскольку macOS не имеет нативной поддержки Linux-сетевых пространств. В качестве такой среды рекомендуется использовать OrbStack — лёгкий гипервизор, совместимый с Docker и Linux-ядром.

### Порядок установки

1. **Установить OrbStack**. Скачать и установить OrbStack с официального сайта [https://orbstack.dev](https://orbstack.dev). OrbStack создаёт лёгкую виртуальную машину Linux, в которой работает Docker и все сетевые инструменты, необходимые Containerlab.
2. **Запустить Linux-среду внутри OrbStack**. После установки открыть терминал OrbStack и войти в Linux-оболочку (по умолчанию Ubuntu-подобная система).
3. **Установить Docker и Containerlab**. Внутри OrbStack-Linux выполнить стандартную установку для Linux: `curl -sL https://containerlab.dev/setup | sudo -E bash -s "all"`. Эта команда установит Docker, Docker Compose, GitHub CLI и сам Containerlab.

![[Screenshot 2025-10-22 at 18.12.49.png]]

4. **Проверить установку**. После завершения выйти и войти снова, чтобы активировать группы пользователей, и проверить:
```
clab version
docker ps
```

![[Screenshot 2025-10-22 at 18.15.15.png]]

5. **Проверить архитектуру сетевых образов**. На ARM-Mac рекомендуется использовать контейнерные образы, собранные под архитектуру arm64. Проверить это можно командой:
```
docker image inspect <image> -f '{{.Architecture}}'
```
6. **Развернуть лабораторию**. Теперь можно развернуть тестовую лабораторию, например: `sudo clab deploy -t examples/srl01.clab.yml`.
  
  ![[Screenshot 2025-10-22 at 18.25.40.png]]

Зайдем на первый хост, посмотрим существующие интерфейсы и поднимем интерфейс ethernet-1/1, добавим ipv4 адрес 10.0.0.1/31.

![[Screenshot 2025-10-22 at 18.34.40.png]]

Проверка добавления интерфейса.

![[Screenshot 2025-10-22 at 18.34.53.png]]

Аналогично делаем на втором узле. Присваиваем ipv4 адрес 10.0.0.2/31.

![[Screenshot 2025-10-22 at 18.37.52.png]]

![[Screenshot 2025-10-22 at 18.38.14.png]]

Пропингуем первый хост со второго: ping 10.0.0.1

С помощью команды `containerlab graph -t srl02.clab.yml` посмотрим на графическое изображение сети.
![[Screenshot 2025-10-22 at 19.08.04.png]]