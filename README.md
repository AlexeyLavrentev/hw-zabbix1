# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Лаврентьев Алексей`

## Задание 1
Установите Zabbix Server с веб-интерфейсом.

Процесс выполнения
Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.
Требования к результатам
Прикрепите в файл README.md скриншот авторизации в админке.
Приложите в файл README.md текст использованных команд в GitHub.
Выполнение:
<img width="1883" height="995" alt="image" src="https://github.com/user-attachments/assets/d68df822-2e2c-495f-ae8e-da2e5b226f05" />

### Использованные команды

# Обновление пакетов и установка PostgreSQL
sudo apt update
sudo apt install -y postgresql postgresql-contrib
sudo systemctl enable --now postgresql

### Подключение репозитория Zabbix 7.0 LTS (Debian 11)
cd /tmp
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-1+debian11_all.deb
sudo dpkg -i zabbix-release_6.0-1+debian11_all.deb
sudo apt update

### Установка Zabbix Server, Web и Agent
sudo apt install -y \
  zabbix-server-pgsql \
  zabbix-frontend-php \
  zabbix-apache-conf \
  zabbix-sql-scripts \
  zabbix-agent \
  php-pgsql

### Создание пользователя и базы данных в PostgreSQL
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix

### Импорт схемы базы Zabbix
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz \
  | sudo -u zabbix psql zabbix

### Запуск сервисов
sudo systemctl enable zabbix-server zabbix-agent apache2
sudo systemctl restart zabbix-server zabbix-agent apache2

### Редактирование файла README.md, коммит и git push
git add 
git commit -m "update README.md"
git push origin main

## Задание 2
Установите Zabbix Agent на два хоста.

Процесс выполнения
Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.
Требования к результатам
Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
Приложите в файл README.md текст использованных команд в GitHub

### Выполнение:
### Скриншот Configuration - Hosts
<img width="1906" height="624" alt="image" src="https://github.com/user-attachments/assets/92382e20-2692-485a-94e4-43dd385baadc" />
### Скриншот Monitoring - Latest data
<img width="1895" height="1003" alt="image" src="https://github.com/user-attachments/assets/10cc7b9a-f148-4c59-94c6-f19c8c314b52" />

### Использованные команды

### Настройка агента на сервере
sudo nano /etc/zabbix/zabbix_agentd.conf
sudo systemctl enable zabbix-agent
sudo systemctl restart zabbix-agent

### Установка и настройка агента на втором хосте
sudo apt install -y zabbix-agent
sudo nano /etc/zabbix/zabbix_agentd.conf
sudo systemctl enable zabbix-agent
sudo systemctl restart zabbix-agent

### Редактирование файла README.md, коммит и git push
git add 
git commit -m "update README.md"
git push origin main

### Задание 3 со звёздочкой*
Установите Zabbix Agent на Windows (компьютер) и подключите его к серверу Zabbix.

Требования к результатам
Приложите в файл README.md скриншот раздела Latest Data, где видно свободное место на диске C:

### Выполнение:
### Скриншот добавленного хоста wondows
<img width="1906" height="643" alt="image" src="https://github.com/user-attachments/assets/636603dd-8cf4-48be-97be-77d288164a52" />

### Скриншот свободного места на диске C:
<img width="1896" height="777" alt="image" src="https://github.com/user-attachments/assets/94595708-c0ed-4897-9e42-3d025a614104" />

