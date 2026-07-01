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
2.1. Если нужна РАЗРАБОТКА ПОД ANDROID (Вариант №1)
sudo snap install android-studio --classic
Или через официальный сайт.
2.2. Если нужна ВЕБ-РАЗРАБОТКА (Варианты №2, №6, №8)
# Node.js и npm
sudo apt install nodejs npm -y
# Проверка
node -v
npm -v
# Установка фреймворков (если требуется)
sudo npm install -g @angular/cli
sudo npm install -g create-react-app
2.3. Если нужна РАЗРАБОТКА НА PYTHON (Варианты №7, №10, №11, №21, №24)
sudo apt install python3 python3-pip python3-venv -y

# Проверка
python3 --version
pip3 --version
# Установка библиотек (если требуется)
pip3 install numpy pandas matplotlib
2.4. Если нужна РАЗРАБОТКА НА JAVA (Варианты №18, №27, №29)
sudo apt install openjdk-17-jdk maven -y
# Проверка
java -version
mvn -version
2.5. Если нужна РАБОТА С БАЗАМИ ДАННЫХ (Варианты №2, №8, №9, №16, №18)
bash
sudo apt install postgresql postgresql-contrib -y
# Проверка
sudo -u postgres psql -c "SELECT version();"
# Установка пароля
sudo -u postgres psql -c "ALTER USER postgres PASSWORD 'postgres';"
2.6. Если нужна ГРАФИКА (Варианты №3, №4, №12, №20)
bash
# Для 2D графики
sudo apt install gimp -y

# Для 3D графики
sudo apt install blender -y

# Для векторной графики
sudo apt install inkscape -y
2.7. Если нужен Docker или КОНТЕЙНЕРЫ (Варианты №5, №9, №13)
bash
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker

# Добавление пользователя в группу docker
sudo usermod -aG docker $USER
# Проверка
docker --version
2.8. Если нужен ОФИСНЫЙ ПАКЕТ
bash
sudo apt install libreoffice -y
2.9. Если нужен АРХИВАТОР
bash
sudo apt install peazip -y
2.10. Если нужен АНТИВИРУС
bash
sudo apt install clamav clamtk -y
sudo freshclam  # обновление баз
2.11. Если нужны ИНСТРУМЕНТЫ ТЕСТИРОВАНИЯ API (Вариант №2)
bash
sudo snap install postman
# или
sudo apt install curl jq -y
2.12. Если нужен VS CODE (универсальная IDE)
bash
sudo snap install code --classic
# или
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
sudo apt update
sudo apt install code -y
2.13. Настройка совместимости (ОБЯЗАТЕЛЬНО! 2.5 балла)
bash
# Отключение масштабирования
gsettings set org.gnome.desktop.interface scaling-factor 1

# Отключение анимаций
gsettings set org.gnome.desktop.interface enable-animations false

# Отключение композиции
gsettings set org.gnome.mutter check-alive-timeout 0

# Для Qt-приложений
export QT_AUTO_SCREEN_SCALE_FACTOR=0
