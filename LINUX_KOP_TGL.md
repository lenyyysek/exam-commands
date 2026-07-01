ААА ГИТ КОЛААА(ПРОСТО ИНТЕРФЕЙС ДЛЯ ГИТА)
sudo apt install git git-cola -y

ЧАСТЬ 1. УНИВЕРСАЛЬНОЕ ДЛЯ ЛЮБОГО ВАРИАНТА (25-28 баллов)
Это делается ВСЕГДА, независимо от билета. Время: 30 минут.
1.1. Обновляем список пакетов и устанавливаем последние версии. Это стандартная процедура перед любой установкой.
sudo apt update && sudo apt upgrade -y

1.2. Установка базового ПО (копируй целиком)
sudo apt install -y curl wget git nano vim gedit htop build-essential net-tools openssh-server xrdp cups printer-driver-cups-pdf timeshift  auditd rsyslog ufw fail2ban genisoimage libreoffice

Редакторы
	nano, vim, gedit — текстовые редакторы (как Блокнот), чтобы редактировать файлы
Инструменты разработки
	build-essential — компиляторы (GCC/G++/Make), чтобы компилировать код
	git — система контроля версий (как GitHub)
	curl, wget — скачивание файлов из интернета
Сеть и удаленный доступ
	openssh-server — SSH-сервер для удаленного подключения
	xrdp — удаленный рабочий стол (RDP) для Windows
	net-tools — сетевые утилиты (ifconfig, netstat) 
Принтеры
	cups — система печати
	printer-driver-cups-pdf — виртуальный PDF-принтер
Резервное копирование
	timeshift — точки восстановления системы (как в Windows)
	genisoimage — создание ISO-образов
Безопасность и логи
	auditd — журнал мониторинга (слежка за системой)
	rsyslog — система логирования
	ufw — брандмауэр (файрвол)
	fail2ban — защита от взлома
Офис
	libreoffice — офисный пакет (Word, Excel, PowerPoint)
1.3. Запуск служб 
# Проверка редакторов
nano --version
vim --version
gedit --version
# Проверка компиляторов (GNU GCC) - ЭТО ВАЖНО!
gcc --version
g++ --version
make --version
# Проверка офисного пакета
libreoffice –version
Включаем службы, чтобы они автоматически запускались при загрузке и работали сейчас
sudo systemctl enable ssh
sudo systemctl start ssh

sudo systemctl enable xrdp
sudo systemctl start xrdp

sudo systemctl enable cups
sudo systemctl start cups

sudo systemctl enable auditd
sudo systemctl start auditd

sudo systemctl enable fail2ban
sudo systemctl start fail2ban
1.4. Настройка ядра 
sudo sysctl vm.swappiness=10
1.5. Настройка сети (ОБЯЗАТЕЛЬНО показать!)
bash
# Проверка соединения
ping -c 4 google.com
curl -I https://google.com
1.6. Виртуальный принтер (проверка)
bash
# Печать тестовой страницы в PDF
echo "Test print" | lp -d PDF
# Файл появится в /home/ваше_имя/PDF/
ls ~/PDF/
1.7. Резервное копирование
bash
tar -czvf /home/$USER/backup_$(date +%Y%m%d).tar.gz /home/$USER/
1.8. Установочный образ системы (одна команда)
# СОЗДАНИЕ ОБРАЗА С ФЛАГОМ ДЛЯ БОЛЬШИХ ФАЙЛОВ
sudo genisoimage -allow-limited-size -o /home/$USER/system_image_$(date +%Y%m%d).iso /
# ПРОВЕРКА
ls -lh /home/$USER/*.iso
Создаем ISO-образ всей системы.
Флаг -allow-limited-size нужен, потому что в системе есть файлы больше 4 ГБ (стандартный ISO 9660 не поддерживает такие большие файлы). Без этого флага будет ошибка.
/ в конце - это корневая папка, которую мы упаковываем в ISO.
1.9. Точка восстановления
bash
sudo timeshift --create --comments "Exam restore point"
sudo timeshift –list
Чтобы откаться нужно написать sudo timeshift –restore и дальше выбрать номер точки, до которой хотим откатиться
1.10. Пользователи и группы (сделать БЫСТРО)
# Группы
sudo groupadd developers
sudo groupadd admins

# Пользователи
sudo useradd -m -s /bin/bash student
sudo passwd student
# Добавление в группы
sudo usermod -aG developers student
sudo usermod -aG admins student
# Проверка
groups student
1.11. Права доступа (одна команда)
# Создать папку и дать права
sudo mkdir -p /srv/project
sudo chown -R root:developers /srv/project
sudo chmod -R 770 /srv/project
Первая цифра — права для владельца файла или каталога.
Вторая цифра — права для группы, к которой принадлежит файл или каталог.
Третья цифра — права для всех остальных пользователей.
	Чтение (r) — 4
	Запись (w) — 2
	Выполнение (x) — 1
	Нет прав — 0
1.12. Настройка аутентификации
# Разрешить группе admins делать sudo без пароля
echo "%admins ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/admins
1.13. Журнал мониторинга
bash
# Правила аудита
sudo auditctl -w /etc/passwd -p wa -k identity
sudo auditctl -w /srv/project -p rwxa -k project_access
# Просмотр логов
sudo ausearch -k identity
sudo journalctl -n 10
-w – Watch(следить) за папкой /etc/passwd; -p wa  это Permissions (права доступа): w – запись, a = attribute (изменение атрибутов)
1.14. Брандмауэр
sudo ufw allow 22/tcp
sudo ufw allow 3389/tcp
sudo ufw enable
sudo ufw status





ЧАСТЬ 2. В ЗАВИСИМОСТИ ОТ УСЛОВИЙ ВАРИАНТА (8-10 баллов)
ВАРИАНТ №1 (Android разработка + 3D + безопасность)
КЛЮЧЕВЫЕ СЛОВА: Android, мобильные приложения, эмулятор 3D графики, эмулятор ОС, безопасность от хакеров
Какие команды копировать:
bash
# Android Studio (есть ярлык, графическое приложение)
sudo snap install android-studio --classic
# VirtualGL - эмулятор 3D графики (нет ярлыка, консольный)
wget https://github.com/VirtualGL/virtualgl/releases/download/3.1.1/virtualgl_3.1.1_amd64.deb
sudo dpkg -i virtualgl_3.1.1_amd64.deb
# Mesa - утилиты для 3D (нет ярлыка, консольный)
sudo apt install mesa-utils -y
# VirtualBox - эмулятор ОС (есть ярлык, графическое приложение)
sudo apt install virtualbox -y
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y




ВАРИАНТ №2 (Веб-сервисы + API + Node.js)
КЛЮЧЕВЫЕ СЛОВА: высоконагруженные веб-сервисы, Python, JavaScript, Django, Node.js, REST API, тестирование API, защита соединений
Какие команды копировать:
bash
# VS Code - среда разработки (есть ярлык, графическое приложение)
sudo snap install code --classic
# Node.js - среда выполнения JavaScript (нет ярлыка, консольный)
sudo apt install nodejs npm -y
# Python (нет ярлыка, консольный)
sudo apt install python3 python3-pip -y
# PostgreSQL - база данных (нет ярлыка, консольный)
sudo apt install postgresql -y
# Postman - тестирование API (есть ярлык, графическое приложение)
sudo snap install postman
# Angular CLI - фреймворк (нет ярлыка, консольный)
sudo npm install -g @angular/cli
# Create React App - фреймворк (нет ярлыка, консольный)
sudo npm install -g create-react-app




ВАРИАНТ №3 (Flutter / Dart)
КЛЮЧЕВЫЕ СЛОВА: кроссплатформенные мобильные приложения, iOS и Android, Dart, Flutter SDK, эмулятор мобильных устройств, визуальное проектирование
Какие команды копировать:
bash
# Flutter SDK (нет ярлыка, консольный)
sudo snap install flutter --classic
# VS Code - среда разработки (есть ярлык, графическое приложение)
sudo snap install code --classic
# Flutter расширение для VS Code (нет ярлыка, расширение)
code --install-extension dart-code.flutter
# Dart расширение для VS Code (нет ярлыка, расширение)
code --install-extension dart-code.dart-code
# GIMP - для 2D графики (есть ярлык, графическое приложение)
sudo apt install gimp -y
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y
# Проверка установки Flutter (нет ярлыка, консольный)
flutter doctor




ВАРИАНТ №4 (3D игры + Unity + C#)
КЛЮЧЕВЫЕ СЛОВА: 3D игры, игровой движок, C#, графический API, 3D модели, анимации, текстуры, управление ассетами
Какие команды копировать:
bash
# Unity Hub (есть ярлык, графическое приложение)
sudo apt install curl
sudo install -d /etc/apt/keyrings
curl -fsSL https://hub.unity3d.com/linux/keys/public | sudo gpg --dearmor -o /etc/apt/keyrings/unityhub.gpg
echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/unityhub.gpg] https://hub.unity3d.com/linux/repos/deb stable main" | sudo tee /etc/apt/sources.list.d/unityhub.list
sudo apt update
sudo apt install unityhub
# Blender - 3D моделирование (есть ярлык, графическое приложение)
sudo apt install blender -y
# Git LFS - управление ассетами (нет ярлыка, консольный)
sudo apt install git-lfs -y
git lfs install
# Vulkan - графический API (нет ярлыка, консольный)
sudo apt install vulkan-tools libvulkan-dev mesa-vulkan-drivers -y
# GIMP - 2D графика (есть ярлык, графическое приложение)
sudo apt install gimp -y
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y




ВАРИАНТ №5 (DevOps + CI/CD + Docker)
КЛЮЧЕВЫЕ СЛОВА: DevOps, CI/CD, сборка, развертывание, контейнеризация, мониторинг серверов
Какие команды копировать:
bash
# VS Code - редактор (есть ярлык, графическое приложение)
sudo snap install code --classic
# Docker - контейнеризация (нет ярлыка, консольный)
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker $USER
# Jenkins - CI/CD (через App Center, есть веб-интерфейс)
# Установить через оранжевое приложение App Center в Ubuntu
# После установки зайти на http://localhost:8080
# Пароль: sudo cat /var/snap/jenkins/5050/secrets/initialAdminPassword
# Grafana + Prometheus - мониторинг (нет ярлыка, веб-интерфейс)
sudo snap install grafana prometheus
# Ansible - управление конфигурациями (нет ярлыка, консольный)
sudo apt install ansible -y




ВАРИАНТ №6 (Кросс-платформенные десктопные приложения)
КЛЮЧЕВЫЕ СЛОВА: кросс-платформенные настольные приложения, Windows, macOS, Linux, JavaScript/TypeScript, Electron, установочные пакеты, инсталляторы
Какие команды копировать:
bash
# VS Code - среда разработки (есть ярлык, графическое приложение)
sudo snap install code --classic
# Node.js (нет ярлыка, консольный)
sudo apt install nodejs npm -y
# Electron - фреймворк (нет ярлыка, консольный)
sudo npm install -g electron electron-builder
sudo chown root:root /usr/lib/node_modules/electron/dist/chrome-sandbox
sudo chmod 4755 /usr/lib/node_modules/electron/dist/chrome-sandbox
# NSIS - создание установщиков (нет ярлыка, консольный)
sudo apt install nsis -y
# Webpack / Vite - сборка (нет ярлыка, консольный)
sudo npm install -g webpack webpack-cli vite
# KeePassXC - хранение паролей (есть ярлык, графическое приложение)
sudo apt install keepassxc -y



ВАРИАНТ №7 (Data Science + ML)
КЛЮЧЕВЫЕ СЛОВА: анализ данных, машинное обучение, GPU, Python, R, Jupyter, нейронные сети, эксперименты, управление версиями данных
Какие команды копировать:
bash
# Python (нет ярлыка, консольный)
sudo apt install python3 python3-pip python3-venv -y
# Jupyter Notebook (нет ярлыка, веб-интерфейс)
sudo apt install jupyter-notebook jupyter -y
# R - язык статистики (нет ярлыка, консольный)
sudo apt install r-base r-base-dev -y
# MLflow - управление экспериментами (нет ярлыка, веб-интерфейс)
sudo apt install pipx -y
pipx install mlflow
pipx ensurepath
mlflow ui
# DVC - управление версиями данных (нет ярлыка, консольный)
sudo apt install pipx -y
pipx install dvc
dvc --version
# PyTorch, TensorFlow, Scikit-learn (в виртуальном окружении)
python3 -m venv ml_env
source ml_env/bin/activate
pip install torch torchvision torchaudio
pip install tensorflow
pip install scikit-learn



ВАРИАНТ №8 (SPA + React/Angular/Vue + Node.js + NoSQL)
КЛЮЧЕВЫЕ СЛОВА: SPA-приложения, JavaScript/TypeScript, React, Angular, Vue, Node.js, бандлер, NoSQL, защита от веб-уязвимостей
Какие команды копировать:
bash
# VS Code (есть ярлык, графическое приложение)
sudo snap install code --classic
# Node.js (нет ярлыка, консольный)
sudo apt install nodejs npm -y
# Angular CLI (нет ярлыка, консольный)
sudo npm install -g @angular/cli
# Create React App (нет ярлыка, консольный)
sudo npm install -g create-react-app
# Vite - бандлер (нет ярлыка, консольный)
sudo npm install -g vite
# MongoDB - NoSQL база (нет ярлыка, консольный)
sudo apt update
sudo apt install -y gnupg curl
curl -fsSL https://mongodb.com | sudo gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg --dearmor
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://mongodb.org $(lsb_release -cs)/mongodb-org/8.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.0.list
sudo apt update
sudo apt install -y mongodb-org
sudo systemctl start mongod
sudo systemctl enable mongod
# ESLint, Prettier, Snyk - безопасность (нет ярлыка, консольные)
sudo npm install -g eslint prettier snyk



ВАРИАНТ №9 (.NET + Docker)
КЛЮЧЕВЫЕ СЛОВА: .NET, C#, F#, VB.NET, ASP.NET Core, Entity Framework, SQL Server, контейнеризация
Какие команды копировать:
bash
# VS Code (есть ярлык, графическое приложение)
sudo snap install code --classic
# .NET SDK (нет ярлыка, консольный)
wget https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt update
sudo apt install dotnet-sdk-8.0 -y
# Docker - контейнеризация (нет ярлыка, консольный)
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker $USER
# PostgreSQL - база данных (нет ярлыка, консольный)
sudo apt install postgresql -y
sudo -u postgres psql -c "ALTER USER postgres PASSWORD 'postgres';"



ВАРИАНТ №10 (IoT + MQTT)
КЛЮЧЕВЫЕ СЛОВА: Интернет Вещей, IoT, C/C++/Python, микроконтроллеры, Raspberry Pi, ESP32, MQTT, CoAP, симулятор IoT
Какие команды копировать:
bash
# VS Code (есть ярлык, графическое приложение)
sudo snap install code --classic
# Python (нет ярлыка, консольный)
sudo apt install python3 python3-pip python3-venv -y
# Mosquitto - MQTT брокер (нет ярлыка, консольный)
sudo apt install mosquitto -y
# Node-RED - визуализация IoT (нет ярлыка, веб-интерфейс)
sudo npm install -g node-red
# PlatformIO - для микроконтроллеров (нет ярлыка, консольный)
sudo apt install pipx -y
pipx ensurepath
pipx install platformio



ВАРИАНТ №11 (Микроконтроллеры)
КЛЮЧЕВЫЕ СЛОВА: микроконтроллеры, C/C++, JTAG/SWD, ARM, эмулятор, симулятор, программатор, отладчик
Какие команды копировать:
bash
# Arduino IDE (есть ярлык, графическое приложение)
sudo snap install arduino
# VS Code (есть ярлык, графическое приложение)
sudo snap install code --classic
# Python (нет ярлыка, консольный)
sudo apt install python3 python3-pip python3-venv -y
# QEMU - эмулятор микроконтроллера (нет ярлыка, консольный)
sudo apt install qemu-system-arm qemu-user-static -y
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y



ВАРИАНТ №12 (VR/AR)
КЛЮЧЕВЫЕ СЛОВА: VR, AR, виртуальная реальность, дополненная реальность, игровой движок, C#, C++, 3D-ассеты, Oculus, HTC Vive, HoloLens
Какие команды копировать:
bash
# Unity Hub (есть ярлык, графическое приложение)
sudo apt install curl
sudo install -d /etc/apt/keyrings
curl -fsSL https://hub.unity3d.com/linux/keys/public | sudo gpg --dearmor -o /etc/apt/keyrings/unityhub.gpg
echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/unityhub.gpg] https://hub.unity3d.com/linux/repos/deb stable main" | sudo tee /etc/apt/sources.list.d/unityhub.list
sudo apt update
sudo apt install unityhub
# Blender - 3D модели (есть ярлык, графическое приложение)
sudo apt install blender -y
# Vulkan - графический API (нет ярлыка, консольный)
sudo apt install vulkan-tools libvulkan-dev mesa-vulkan-drivers -y
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y



ВАРИАНТ №13 (DevSecOps)
КЛЮЧЕВЫЕ СЛОВА: DevSecOps, безопасность, CI/CD, SAST, DAST, SCA, сканирование зависимостей, уязвимости, Kubernetes
Какие команды копировать:
bash
# OWASP ZAP - динамический анализ (есть ярлык, графическое приложение)
sudo snap install zaproxy
# OWASP Dependency Check - сканирование зависимостей (нет ярлыка, консольный)
wget https://github.com/dependency-check/DependencyCheck/releases/download/v12.0.0/dependency-check-12.0.0-release.zip
unzip dependency-check-12.0.0-release.zip
cd dependency-check
chmod +x bin/dependency-check.sh
./bin/dependency-check.sh --updateonly
# Kubectl - управление Kubernetes (нет ярлыка, консольный)
sudo snap install kubectl --classic
# Docker (нет ярлыка, консольный)
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker



ВАРИАНТ №14 (Автоматизированное тестирование)
КЛЮЧЕВЫЕ СЛОВА: автоматизированное тестирование, Selenium, Cypress, Appium, Python, Java, JavaScript, нагрузочное тестирование, WebDriver
Какие команды копировать:
bash
# VS Code (есть ярлык, графическое приложение)
sudo snap install code --classic
# Python (нет ярлыка, консольный)
sudo apt install python3 python3-pip python3-venv -y
# Selenium (нет ярлыка, библиотека)
sudo apt install python3-selenium -y
sudo apt install python3-webdriver-manager -y
sudo snap install chromium
# Cypress (нет ярлыка, консольный)
sudo npm install -g cypress
# JMeter - нагрузочное тестирование (есть ярлык, графическое приложение)
sudo apt install jmeter -y
# Allure - отчетность (есть ярлык, графическое приложение)
sudo apt install allure -y




ВАРИАНТ №15 (Администрирование)
КЛЮЧЕВЫЕ СЛОВА: серверная инфраструктура, управление конфигурациями, Ansible, Puppet, Chef, мониторинг, Zabbix, Nagios, Prometheus, Grafana, виртуализация, KVM, резервное копирование
Какие команды копировать:
bash
# Ansible (нет ярлыка, консольный)
sudo apt install ansible -y
# Grafana + Prometheus - мониторинг (нет ярлыка, веб-интерфейс)
sudo snap install grafana prometheus
# Timeshift - резервное копирование (есть ярлык, графическое приложение)
sudo apt install timeshift -y




ВАРИАНТ №16 (Медицина)
КЛЮЧЕВЫЕ СЛОВА: медицинские учреждения, C++, Python, HIPAA, GDPR, криптографическая защита, DICOM, статистический анализ
Какие команды копировать:
bash
# VS Code (есть ярлык, графическое приложение)
sudo snap install code --classic
# Python (нет ярлыка, консольный)
sudo apt install python3 python3-pip python3-venv -y
# PostgreSQL - СУБД (нет ярлыка, консольный)
sudo apt install postgresql -y
sudo -u postgres psql -c "ALTER USER postgres PASSWORD 'postgres';"
# OpenSSL - криптография (нет ярлыка, консольный)
sudo apt install openssl -y
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y



ВАРИАНТ №17 (Автомобильные системы)
КЛЮЧЕВЫЕ СЛОВА: AUTOSAR, автомобильные системы, CAN-шина, SOME/IP, DDS, ISO 26262, Linux QNX
Какие команды копировать:
bash
# Eclipse (есть ярлык, графическое приложение)
sudo snap install eclipse --classic
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y



ВАРИАНТ №18 (Банковское ПО)
КЛЮЧЕВЫЕ СЛОВА: Java EE, финансовые транзакции, Oracle, PCI DSS, криптографическая защита, двухфакторная аутентификация, Red Hat Enterprise Linux
Какие команды копировать:
bash
# IntelliJ IDEA (есть ярлык, графическое приложение)
sudo snap install intellij-idea-community --classic
# Java (нет ярлыка, консольный)
sudo apt install openjdk-17-jdk -y
# Maven (нет ярлыка, консольный)
sudo apt install maven -y
# PostgreSQL - альтернатива Oracle (нет ярлыка, консольный)
sudo apt install postgresql -y
sudo -u postgres psql -c "ALTER USER postgres PASSWORD 'postgres';"
# OpenSSL - криптография (нет ярлыка, консольный)
sudo apt install openssl -y
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y



ВАРИАНТ №19 (Онлайн-игры)
КЛЮЧЕВЫЕ СЛОВА: многопользовательские онлайн-игры, C++, Python, серверная логика, мониторинг, балансировка нагрузки, нагрузочное тестирование, DDoS
Какие команды копировать:
bash
# VS Code (есть ярлык, графическое приложение)
sudo snap install code --classic
# Python (нет ярлыка, консольный)
sudo apt install python3 python3-pip python3-venv -y
# JMeter - нагрузочное тестирование (есть ярлык, графическое приложение)
sudo apt install jmeter -y
# Grafana - мониторинг (нет ярлыка, веб-интерфейс)
sudo snap install grafana prometheus
# HAProxy - балансировка нагрузки (нет ярлыка, консольный)
sudo apt install haproxy -y
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y



ВАРИАНТ №20 (Компьютерное зрение)
КЛЮЧЕВЫЕ СЛОВА: компьютерное зрение, видеонаблюдение, Python, C++, IP-камеры, видеопотоки, нейросети, детекция объектов, CUDA, cuDNN, NVIDIA GPU
Какие команды копировать:
bash
# VS Code (есть ярлык, графическое приложение)
sudo snap install code --classic
# Python (нет ярлыка, консольный)
sudo apt install python3 python3-pip python3-venv -y
# OpenCV (нет ярлыка, библиотека)
sudo apt install python3-opencv -y
# GIMP - обработка изображений (есть ярлык, графическое приложение)
sudo apt install gimp -y
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y



ВАРИАНТ №21 (Голосовое управление)
КЛЮЧЕВЫЕ СЛОВА: голосовое управление, распознавание речи, Python, аудио, голосовые ассистенты, акустический анализ
Какие команды копировать:
bash
# VS Code (есть ярлык, графическое приложение)
sudo snap install code --classic
# Python (нет ярлыка, консольный)
sudo apt install python3 python3-pip python3-venv -y
# SpeechRecognition (нет ярлыка, библиотека)
pipx install SpeechRecognition
# Vosk и Whisper (нет ярлыка, библиотеки)
pipx install vosk openai-whisper
# Системные аудио-зависимости (нет ярлыка, консольные)
sudo apt install portaudio19-dev python3-pyaudio sox libsox-fmt-all -y
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y



ВАРИАНТ №22 (Умный дом)
КЛЮЧЕВЫЕ СЛОВА: умный дом, IoT, Python, C++, Zigbee, Z-Wave, эмуляция домашней сети, тестирование безопасности
Какие команды копировать:
bash
# VS Code (есть ярлык, графическое приложение)
sudo snap install code --classic
# Python (нет ярлыка, консольный)
sudo apt install python3 python3-pip python3-venv -y
# Mosquitto - MQTT брокер (нет ярлыка, консольный)
sudo apt install mosquitto -y
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y



ВАРИАНТ №23 (AR для промышленности)
КЛЮЧЕВЫЕ СЛОВА: AR-решения, промышленность, Unity, AR-плагины, SDK для AR-гарнитур, 3D-маркировка
Какие команды копировать:
bash
# Unity Hub (есть ярлык, графическое приложение)
sudo apt install curl
sudo install -d /etc/apt/keyrings
curl -fsSL https://hub.unity3d.com/linux/keys/public | sudo gpg --dearmor -o /etc/apt/keyrings/unityhub.gpg
echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/unityhub.gpg] https://hub.unity3d.com/linux/repos/deb stable main" | sudo tee /etc/apt/sources.list.d/unityhub.list
sudo apt update
sudo apt install unityhub
# Blender - 3D модели (есть ярлык, графическое приложение)
sudo apt install blender -y
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y



ВАРИАНТ №24 (Чат-боты + NLP)
КЛЮЧЕВЫЕ СЛОВА: чат-боты, виртуальные помощники, Python, NLP, диалоговые системы, текстовые корпуса, понимание естественного языка, Ubuntu Server
Какие команды копировать:
bash
# VS Code (есть ярлык, графическое приложение)
sudo snap install code --classic
# Python (нет ярлыка, консольный)
sudo apt install python3 python3-pip python3-venv -y
# Rasa - диалоговые системы (нет ярлыка, консольный)
sudo add-apt-repository ppa:deadsnakes/ppa -y && sudo apt update
sudo apt install python3.10 python3.10-venv -y
python3.10 -m venv rasa_env && source rasa_env/bin/activate
pip install rasa
# spaCy - NLP (нет ярлыка, библиотека)
pip3 install spacy
python3 -m spacy download ru_core_news_sm
# NLTK - обработка текстов (нет ярлыка, библиотека)
pip3 install nltk
python3 -c "import nltk; nltk.download('all')"
# Transformers - нейросети для текста (нет ярлыка, библиотека)
pip3 install transformers sentence-transformers
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y



ВАРИАНТ №25 (SCADA + Энергетика)
КЛЮЧЕВЫЕ СЛОВА: SCADA, энергетика, C++, промышленные протоколы, IEC 61850, DNP3, визуализация энергосетей, телеметрия, QNX
Какие команды копировать:
bash
# VS Code (есть ярлык, графическое приложение)
sudo snap install code --classic
# Python (нет ярлыка, консольный)
sudo apt install python3 python3-pip python3-venv -y
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y



ВАРИАНТ №26 (Биометрия)
КЛЮЧЕВЫЕ СЛОВА: биометрические системы, Python, компьютерное зрение, камеры высокого разрешения, нейросети, распознавание, CUDA
Какие команды копировать:
bash
# VS Code (есть ярлык, графическое приложение)
sudo snap install code --classic
# Python (нет ярлыка, консольный)
sudo apt install python3 python3-pip python3-venv -y
# OpenCV (нет ярлыка, библиотека)
sudo apt install python3-opencv -y
# dlib и face_recognition (нет ярлыка, библиотеки)
sudo apt install pipx -y
pipx ensurepath
source ~/.bashrc
pipx install dlib
pipx install face_recognition
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y



ВАРИАНТ №27 (WMS + Склады)
КЛЮЧЕВЫЕ СЛОВА: WMS, склад, Java, RFID, штрих-коды, большие объемы транзакций, симуляция склада, оптимизация маршрутов
Какие команды копировать:
bash
# IntelliJ IDEA (есть ярлык, графическое приложение)
sudo snap install intellij-idea-community --classic
# Java (нет ярлыка, консольный)
sudo apt install openjdk-17-jdk -y
# Maven (нет ярлыка, консольный)
sudo apt install maven -y
# PostgreSQL (нет ярлыка, консольный)
sudo apt install postgresql -y
sudo -u postgres psql -c "ALTER USER postgres PASSWORD 'postgres';"
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y



ВАРИАНТ №28 (Транспорт + Геоданные)
КЛЮЧЕВЫЕ СЛОВА: транспортные системы, Python, временные ряды, визуализация транспортных потоков, геоданные, прогнозирование загруженности
Какие команды копировать:
bash
# VS Code (есть ярлык, графическое приложение)
sudo snap install code --classic
# Python (нет ярлыка, консольный)
sudo apt install python3 python3-pip python3-venv -y
# Folium - визуализация карт (нет ярлыка, библиотека)
pip3 install folium
# GeoPandas - геоданные (нет ярлыка, библиотека)
pip3 install geopandas shapely plotly
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y


ВАРИАНТ №29 (SCM + Логистика)
КЛЮЧЕВЫЕ СЛОВА: SCM, цепочки поставок, Java, ERP, моделирование логистики, анализ рисков, прогнозирование спроса, оптимизация запасов
Какие команды копировать:
bash
# IntelliJ IDEA (есть ярлык, графическое приложение)
sudo snap install intellij-idea-community --classic
# Java (нет ярлыка, консольный)
sudo apt install openjdk-17-jdk -y
# Maven (нет ярлыка, консольный)
sudo apt install maven -y
# PostgreSQL (нет ярлыка, консольный)
sudo apt install postgresql -y
sudo -u postgres psql -c "ALTER USER postgres PASSWORD 'postgres';"
# ClamAV - антивирус (есть ярлык, графический интерфейс ClamTK)
sudo apt install clamav clamtk -y
