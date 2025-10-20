Kathara -- это бесплатное ПО с открытым исходным кодом, поддерживаемое на всех основных операционных системах (Linux, macOs, Windows).

## Установка на macOS

Для работы Kathara необходимо, чтобы был установлен и запущен Docker, поэтому для начала обратимся к инструкции по его установке.
Необходимо:
1. Скачать установщик в формате `.dmg` по следующей ссылке https://docs.docker.com/desktop/release-notes/
2. Дважды щелкнуть на файл Docker.dmg, чтобы открыть установщик, затем перетащить значок Docker в папку «Программы». По умолчанию Docker Desktop устанавливается в папку /Applications/Docker.app.
3. Дважды щелкнуть Docker.app в папке «Программы», чтобы запустить Docker. Установка завершена успешно.

Есть два варианта, как установить Kathara: загрузив релизный пакет с GitHub или используя Homebrew.
В первом случае необходимо:
1. Скачать установочный файл в выбранный вами каталог из раздела Release в официальном репозитории Kathara.
2. Запустить мастер установки и следовать инструкциям.
3. Не забыть запустить Docker перед использованием Kathara.

При установке через Homebrew шагов меньше. В терминале необходимо ввести следующие команды:
```shell
brew tap KatharaFramework/kathara
brew install --cask kathara
```

## Установка на Linux

Kathara может быть установлена на всех основных дистрибутивах Linux. Повторим, что по умолчанию Kathara использует Docker в качестве основной среды виртуализации, однако также может работать на Kubernetes с использованием компонента Megalos.

Для установки Docker можно воспользоваться официальной инструкцией по установке, доступной на сайте Docker.

Рекомендуется установить эмулятор терминала xterm, который Kathara использует по умолчанию. Команда установки зависит от используемого дистрибутива:
```
sudo apt install xterm      # для систем на базе Debian/Ubuntu
sudo yum install xterm      # для систем на базе Red Hat/Fedora
sudo pacman -S xterm        # для Arch Linux
```

### Debian-based

Kathara предоставляет готовые бинарные пакеты для архитектур amd64 и arm64.

#### Ubuntu

Для установки необходимо придерживаться следующей инструкции:
1. Добавить открытый ключ Kathara в систему: `sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 21805A48E6CBBA6B991ABE76646193862B759810`.
2. Подключить репозиторий Kathara: `sudo add-apt-repository ppa:katharaframework/kathara`.
3. Обновить список пакетов: `sudo apt update`.
4. Установить Kathara: `sudo apt install kathara`.

#### Debian 9-12

Kathara поддерживает все основные версии Debian (9, 10, 11 и 12) для архитектур amd64 и arm64. Инструкция по установке во многом одинакова, различия касаются только способа добавления ключа репозитория и версии (кодового имени) Ubuntu, на которой основаны пакеты Kathara.

Инструкция по установке:

1. Добавить открытый ключ Kathara в систему:
- **Для Debian 9 и 10**:
```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 21805A48E6CBBA6B991ABE76646193862B759810
```
- **Для Debian 11 и 12**:
```
wget -qO - "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x21805a48e6cbba6b991abe76646193862b759810" | sudo gpg --dearmor -o /usr/share/keyrings/ppa-kathara-archive-keyring.gpg
```
2. Добавить репозиторий Kathara. Можно вручную создать файл /etc/apt/sources.list.d/kathara.list и добавив в него строки, в зависимости от версии Debian:

| **Версия Debian** | **Кодовое имя Ubuntu** | **Содержимое файла**                                                                                                                                                                                                                                                                                 |
| ----------------- | ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **9 / 10**        | bionic                 | ```<br>deb http://ppa.launchpad.net/katharaframework/kathara/ubuntu bionic maindeb-src http://ppa.launchpad.net/katharaframework/kathara/ubuntu bionic main<br>```                                                                                                                                   |
| **11**            | focal                  | ```<br>deb [ signed-by=/usr/share/keyrings/ppa-kathara-archive-keyring.gpg ] http://ppa.launchpad.net/katharaframework/kathara/ubuntu focal maindeb-src [ signed-by=/usr/share/keyrings/ppa-kathara-archive-keyring.gpg ] http://ppa.launchpad.net/katharaframework/kathara/ubuntu focal main<br>``` |
| **12**            | jammy                  | ```<br>deb [ signed-by=/usr/share/keyrings/ppa-kathara-archive-keyring.gpg ] http://ppa.launchpad.net/katharaframework/kathara/ubuntu jammy maindeb-src [ signed-by=/usr/share/keyrings/ppa-katharaframework/kathara/ubuntu jammy main<br>```                                                        |
Альтернативно, можно выполнить соответствующие команды:
- **Debian 9 / 10**
```
echo "deb http://ppa.launchpad.net/katharaframework/kathara/ubuntu bionic main" | sudo tee /etc/apt/sources.list.d/kathara.list
echo "deb-src http://ppa.launchpad.net/katharaframework/kathara/ubuntu bionic main" | sudo tee -a /etc/apt/sources.list.d/kathara.list
```
- **Debian 11 / 12** (Для Debian 12 заменить focal на jammy)
```
echo "deb [ signed-by=/usr/share/keyrings/ppa-kathara-archive-keyring.gpg ] http://ppa.launchpad.net/katharaframework/kathara/ubuntu focal main" | sudo tee /etc/apt/sources.list.d/kathara.list
echo "deb-src [ signed-by=/usr/share/keyrings/ppa-kathara-archive-keyring.gpg ] http://ppa.launchpad.net/katharaframework/kathara/ubuntu focal main" | sudo tee -a /etc/apt/sources.list.d/kathara.list
```
3. Обновить список пакетов: `sudo apt update`.
4. Установить Kathara: `sudo apt install kathara`.

### Kali Linux

Для установки на Kali Linux необходимо придерживаться инструкций для Debian 12.

### Red Hat-based

1. Загрузите пакет Kathara в формате `.rpm` из раздела Release официального  репозитория на GitHub.
2. Перейдите в каталог, где находится загруженный файл, и выполните команду: `sudo rpm -U <KATHARA_RPM_FILE>.rpm`.
3. Перед запуском Kathara необходимо отключить `firewalld`, чтобы обеспечить корректную работу эмуляции сети: `sudo systemctl stop firewalld`.

### Arch-based

Установка возможна двумя способами: из готового бинарного пакета или из исходников через AUR (Arch User Repository).
1. Установка из бинарного файла
	1. Скачайте файл Kathara в формате `.pkg.tar.zst` из раздела Release.
	2. Перейдите в каталог с файлом и выполните команду: `sudo pacman -U <KATHARA_PKG_FILE>.pkg.tar.zst`.
2. Установка из AUR (исходные файлы)
	1. Клонируйте репозиторий Kathara из AUR: `git clone https://aur.archlinux.org/kathara.git`.
	2. Перейдите в созданный каталог: `cd kathara`.
	3. Соберите и установите пакет: `makepkg -si`. Эта команда автоматически выполнит сборку пакета из исходников и установит его в систему.

## Установка на Windows

Kathara зависит от Docker. Если он у вас уже установлен, то можно переходить сразу к установке эмулятора.

Скачать Docker Desktop можно с официального сайта https://docs.docker.com/desktop/release-notes/ (Docker Desktop Installer.exe). При установке требуется указать 
`Use WSL 2 instead of Hyper-V`, так как Kathara по умолчанию работает с использованием Linux контейнеров. После перезапуска компьютера проверить корректность установки можно из powershell:

```powershell
docker --version
docker run hello-world
```

Необходимо установить Docker Desktop версии >= 2.5.0.0.

Перед использованием Kathará требуется запустить Docker. Для установки эмулятора необходимо: 
1. Скачать установочный файл в выбранную вами директорию со страницы Releases https://github.com/KatharaFramework/Kathara/releases.
2. Запустить мастер установки и следовать инструкциям.

Kathara не используется из классической командной строки.

## Megalos

Megalos представляет собой расширение фреймворка Kathara, которое добавляет поддержку Kubernetes в качестве бэкенд-системы виртуализации. Данная функциональность была официально представлена в версии Kathara 3.0.0.

Megalos отличается от стандартного Kathara. В нем ограничено именование: длина имен collision domain ограничена 4 символами. Сетевые интерфейсы именуются как netX вместо ethX. Каждое устройство имеет служебный интерфейс eth0 для коммуникации с Kubernetes, поэтому этот интерфейс запрещено изменять. Интерфейс eth0 должен быть отключен в демонах маршрутизации. Требуется блокировка подсети Kubernetes Pods в таблицах маршрутизации.

Установка Megalos CNI в кластере Kubernetes требует:
1. Настроить подключения к Kubernetes API Server через kathara settings.
2. Указать URL и API Key для удаленного подключения.


## Запуск примера топологии

На сайте https://github.com/KatharaFramework/Docker-Images представлены Docker файлы и образы, позволяющие быстро развернуть топологию в эмуляторе. Образы собираются как с помощью `docker build`, так и с помощью `docker buildx` для поддержки нескольких архитектур. В настоящее время образы основаны на Debian 11 и компилируются для архитектур amd64 и arm64.

Однако для проверки работы эмулятора создадим простейшую топологию, состоящую из двух хостов и маршрутизатора.

Соединения между хостами прописываются в файле lab.conf:

```
pc1[0]="A"

router[0]="A"
router[1]="B"

pc2[0]="B"
```

Опишем также оба хоста в файлах pc1.startup, pc2.startup:

```
ip link set eth0 up
ip addr add 10.0.0.1/24 dev eth0
ip route add default via 10.0.0.254
```

```
ip link set eth0 up
ip addr add 10.0.1.1/24 dev eth0
ip route add default via 10.0.1.254
```

Запустим топологию `kathara lstart` и посмотрим список устройств `kathara list`:
![[image (2).png]]

Зайдем в устройство pc1 и пропингуем pc2 `kathara connect pc1`:
![[image (1).png]]
Пинг проходит успешно.

