# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Ильин Денис`


## <ins>Задание 1</ins> 
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