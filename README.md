# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Алексей Сидоров`
---

### Задание 1

`Установите Zabbix Server с веб-интерфейсом.`

Процесс выполнения
1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
3. Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Nginx.
4. Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.




`Tекст использованных команд:`

```
sudo -s
wget https://repo.zabbix.com/zabbix/7.4/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.4+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_7.4+ubuntu24.04_all.deb
apt update
apt install postgresql
apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-nginx-conf zabbix-sql-scripts
su - postgres -c 'psql --command "CREATE USER zabbix WITH PASSWORD 
'\'qwertyui\'';"'
su - postgres -c 'psql --command "CREATE DATABASE zabbix OWNER zabbix;"'
zcat /usr/share/zabbix/sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
sed -i 's/# DBPassword=/DBPassword=qwertyui/g' /etc/zabbix/zabbix_server.conf
sudo systemctl restart zabbix-server nginx.service
sudo systemctl enable zabbix-server
```


`Cкриншоты авторизации в админке:`


![img1](https://github.com/PhartomX/netology_zabbix_1/blob/main/img/img1.png)

![img2](https://github.com/PhartomX/netology_zabbix_1/blob/main/img/img2.png)


---

### Задание 2

`Установите Zabbix Agent на два хоста.`

Процесс выполнения
1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
3. Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
4. Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
5. Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.

`Установка агента:`
```
sudo -s 
wget https://repo.zabbix.com/zabbix/7.4/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.4+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_7.4+ubuntu24.04_all.deb
apt update 
apt install zabbix-agent
systemctl restart zabbix-agent
systemctl enable zabbix-agent 
```

`Cкриншот раздела Monitoring > Hosts:`

![img3](https://github.com/PhartomX/netology_zabbix_1/blob/main/img/img3.png)

`Cкриншоты логов zabbix agent:`

![img4](https://github.com/PhartomX/netology_zabbix_1/blob/main/img/img4.png)

![img5](https://github.com/PhartomX/netology_zabbix_1/blob/main/img/img5.png)


`Cкриншот раздела Monitoring > Latest data:`

![img6](https://github.com/PhartomX/netology_zabbix_1/blob/main/img/img6.png)

---
