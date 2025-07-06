# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Ильин Денис`

### Задание 1

Установите Zabbix Server с веб-интерфейсом.

#### Процесс выполнения

1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
3. Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с
   поддержкой PostgreSQL и Apache.
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

![админка zabbix](https://github.com/Delin1984/Netology-IlinDS-gitlab-hw/blob/main/img/admin.jpg)

### Задание 2

Установите Zabbix Agent на два хоста.

#### Процесс выполнения

1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
3. Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
4. Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
5. Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.

#### Требования к результатам

1. Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
2. Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
3. Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от
   агентов данные.
4. Приложите в файл README.md текст использованных команд в GitHub

---

### Задание 3 со звёздочкой*

Установите Zabbix Agent на Windows (компьютер) и подключите его к серверу Zabbix.

#### Требования к результатам

1. Приложите в файл README.md скриншот раздела Latest Data, где видно свободное место на диске C:

### <ins>Решение 2 и 3</ins>

Поставил zabbix server и zabbix agent одновременно на debian, отдельно zabbix agent на отдельную linux машину не ставил,
но зато поставил на windows.

#### Debian

На debin поставил zabbix agent вместе с сервером. Если ставить отдельно(команды те же, только вместо сервера ставим
агент),
но не надо ставить базу данных и web сервер. Если zabbix agent стоит на отдельной от zabbix server машине, то идем
/etc/zabbix/zabbix_agentd.conf, ищем переменную "server" и делаем ее значение = ip адрес сервера zabbix.

#### Windows

Для того чтобы поставить агент на windows идем на сайт zabbix.com, скачиваем нужный архив. Далее в файле конфигурации
прописываем адрес zabbix сервера(в моем случае C:\zabbix\conf\zabbix_agentd)
Далее открываем терминал от имени администратора, устанавливаем и запускаем zabbix_agent:

- C:\zabbix\bin\zabbix_agentd.exe –-config C:\zabbix\conf\zabbix_agentd.conf -–install
- C:\zabbix\bin\zabbix_agentd.exe --start

После необходимо добавить 2 пправила в firewaall windows, разрешающие прохожденеие входящего и исходящего трафика на
порту 10050:

- New-NetFirewallRule -DisplayName 'Zabbix agent_inb' -Profile 'Any' -Direction Inbound -Action Allow -Protocol TCP
  -LocalPort 10050
- New-NetFirewallRule -DisplayName 'Zabbix agent_out' -Profile 'Any' -Direction Outbound -Action Allow -Protocol TCP
  -LocalPort 10050

В админке zabbix добавляем 2 хоста, один для debian, другой для windows. выбираем нужные шаблоны, указываем ip адреса
хостов.

![agents](https://github.com/Delin1984/Netology-IlinDS-gitlab-hw/blob/main/img/agents.jpg)
![debian](https://github.com/Delin1984/Netology-IlinDS-gitlab-hw/blob/main/img/debian.jpg)
![debianconnectserver](https://github.com/Delin1984/Netology-IlinDS-gitlab-hw/blob/main/img/debianconnectserver.jpg)
![Delin](https://github.com/Delin1984/Netology-IlinDS-gitlab-hw/blob/main/img/Delin.jpg)
![freespace](https://github.com/Delin1984/Netology-IlinDS-gitlab-hw/blob/main/img/freespace.jpg)