Подготовка к установке
1.1. Обновляем список пакетов и устанавливаем последние версии. Это стандартная процедура перед любой установкой.

sudo apt update && sudo apt upgrade -y

1.2. Установка базового ПО

sudo apt install -y curl wget git nano vim gedit htop build-essential net-tools openssh-server xrdp cups printer-driver-cups-pdf timeshift  auditd rsyslog ufw fail2ban genisoimage libreoffice

1.3. Запуск служб

# Проверка редакторов, проверка компиляторов (GNU GCC), проверка офисного пакета

nano --version

vim --version

gedit --version

gcc --version

g++ --version

make --version

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

1.5. Настройка сети (показать)

ping -c 4 google.com

curl -I https://google.com

1.6. Виртуальный принтер (проверка)

Печать тестовой страницы в PDF, Файл появится в /home/ваше_имя/PDF/

echo "Test print" | lp -d PDF

ls ~/PDF/

1.7. Резервное копирование

tar -czvf /home/$USER/backup_$(date +%Y%m%d).tar.gz /home/$USER/

1.8. Установочный образ системы (одна команда) С ФЛАГОМ ДЛЯ БОЛЬШИХ ФАЙЛОВ и проверка

sudo genisoimage -allow-limited-size -o /home/$USER/system_image_$(date +%Y%m%d).iso /

ls -lh /home/$USER/*.iso

1.9. Точка восстановления

sudo timeshift --create --comments "Exam restore point"

sudo timeshift –list

Чтобы откаться нужно написать sudo timeshift –restore и дальше выбрать номер точки, до которой хотим откатиться

1.10. Пользователи и группы (сделать БЫСТРО)

sudo groupadd developers

sudo groupadd admins

sudo useradd -m -s /bin/bash student

sudo passwd student

sudo usermod -aG developers student

sudo usermod -aG admins student

groups student

1.11. Права доступа (одна команда)

sudo mkdir -p /srv/project

sudo chown -R root:developers /srv/project

sudo chmod -R 770 /srv/project

1.12. Настройка аутентификации

# Разрешить группе admins делать sudo без пароля

echo "%admins ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/admins

1.13. Журнал мониторинга

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


2 Часть

2.1. Если нужна РАЗРАБОТКА ПОД ANDROID

sudo snap install android-studio --classic

Или через официальный сайт.

2.2. Если нужна ВЕБ-РАЗРАБОТКА

# Node.js и npm и Проверка

sudo apt install nodejs npm -y

node -v

npm -v

# Установка фреймворков (если требуется)

sudo npm install -g @angular/cli

sudo npm install -g create-react-app

2.3. Если нужна РАЗРАБОТКА НА PYTHON и установка библиотек

sudo apt install python3 python3-pip python3-venv -y

python3 --version

pip3 --version

pip3 install numpy pandas matplotlib

2.4. Если нужна РАЗРАБОТКА НА JAVA

sudo apt install openjdk-17-jdk maven -y

java -version

mvn -version

2.5. Если нужна РАБОТА С БАЗАМИ ДАННЫХ

sudo apt install postgresql postgresql-contrib -y

sudo -u postgres psql -c "SELECT version();"

sudo -u postgres psql -c "ALTER USER postgres PASSWORD 'postgres';"

2.6. Если нужна ГРАФИКА

# Для 2D графики

sudo apt install gimp -y

# Для 3D графики

sudo apt install blender -y

# Для векторной графики

sudo apt install inkscape -y

2.7. Если нужен Docker или КОНТЕЙНЕРЫ

sudo apt install docker.io -y

sudo systemctl enable docker

sudo systemctl start docker

# Добавление пользователя в группу docker

sudo usermod -aG docker $USER

docker --version

2.8. Если нужен ОФИСНЫЙ ПАКЕТ

sudo apt install libreoffice -y

2.9. Если нужен АРХИВАТОР

sudo apt install peazip -y

2.10. Если нужен АНТИВИРУС

sudo apt install clamav clamtk -y

sudo freshclam  # обновление баз

2.11. Если нужны ИНСТРУМЕНТЫ ТЕСТИРОВАНИЯ API

sudo snap install postman

или

sudo apt install curl jq -y

2.12. Если нужен VS CODE (универсальная IDE)

sudo snap install code --classic

# или

wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg

sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/

sudo sh -c 'echo "deb [arch=amd64,arm64,armhf] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'

sudo apt update

sudo apt install code -y

2.13. Настройка совместимости (ОБЯЗАТЕЛЬНО! 2.5 балла)

# Отключение масштабирования анимаций и композиции

gsettings set org.gnome.desktop.interface scaling-factor 1

gsettings set org.gnome.desktop.interface enable-animations false

gsettings set org.gnome.mutter check-alive-timeout 0

# Для Qt-приложений

export QT_AUTO_SCREEN_SCALE_FACTOR=0