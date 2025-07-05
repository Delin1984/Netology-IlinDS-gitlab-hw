# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Ильин Денис`

### Задание 1 

Установите Zabbix Server с веб-интерфейсом.

#### Процесс выполнения
1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
3. Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
4. Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.

#### Требования к результатам 
1. Прикрепите в файл README.md скриншот авторизации в админке.
2. Приложите в файл README.md текст использованных команд в GitHub.

### <ins>Решение 1</ins> 
#### Команды для установки системы мониторинга zabbix на debian: 
- переходим в режим root: sudo su
- apt install postgresql
- wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_6.0+debian12_all.deb
- dpkg -i zabbix-release_latest_6.0+debian12_all.deb
- apt update
- apt install zabbix-server-pgsql zabbix-frontend-php php8.2-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent
- sudo -u postgres createuser --pwprompt zabbix
- sudo -u postgres createdb -O zabbix zabbix
- zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
- заходим в файл /etc/zabbix/zabbix_server.conf и редактируем поле DBPassword=zabbix
- systemctl restart zabbix-server zabbix-agent apache2
- systemctl enable zabbix-server zabbix-agent apache2
![админка zabbix](https://github.com/Delin1984/Netology-IlinDS-gitlab-hw/blob/main/img/adminka_zabbix.jpg)
