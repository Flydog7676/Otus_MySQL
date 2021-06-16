# Otus_MySQL
1. Установка MYSQL на MASTER (mysql1) и SLAVE (mysql2)
- Обновление системы ```yum -y update```
- Установка доп программ ```yum -y  install wget```
- Выключение selinux
```  
setenforce 0
sed -i "s/SELINUX=enforcing/SELINUX=permissive/" /etc/selinux/config
```
- Установка MYSQL
```
wget https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
rpm -Uvh mysql80-community-release-el7-3.noarch.rpm
yum install -y mysql-server
systemctl start mysqld
```
- смотрим временный пароль MYSQL
``` grep password /var/log/mysqld.log ```
- скрипт начальной настройки MySQL - меняем пароль на Otus23@!, чистим пользователей и базы ```mysql_secure_installation ```
2. Настраиваем на MASTER сервере
- В файле конфигурации сервера MySql ```/etc/my.cnf``` для того что бы сервер слушал по внутреннему адресу порт 3306 вставляем строку :
 ```bind-address=192.168.50.10```
- Заходим в консоль Mysql c правами root  ```mysql -u root -p```
- Создаем нового пользователя repl с правами только на рекпликацию и разрешение на подключение с адреса 192.168.50.11
```
CREATE USER repl@192.168.50.11 IDENTIFIED WITH caching_sha2_password BY 'Otus23@!';
GRANT REPLICATION SLAVE ON *.* TO repl@192.168.50.11;
SELECT User, Host FROM mysql.user;
```
- Проверяем serverID ```SELECT @@server_id;```
- Проверяем текущее рабочее значение binlog и текущую позицию записи:
```
mysql> SHOW MASTER STATUS;
+---------------+----------+--------------+------------------+-------------------+
| File          | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+---------------+----------+--------------+------------------+-------------------+
| binlog.000004 |      156 |              |                  |                   |
+---------------+----------+--------------+------------------+-------------------+
```
- Перезапускаем сервер MySQL для применения изменения в конфигурации ```service mysqld restart```
3. Настраиваем на SLAVE сервере
- В файле конфигурации сервера MySql ```/etc/my.cnf``` вставляем строчку ```server_id = 2```
- Перезапускаем сервер MySQL для применения изменения в конфигурации ```service mysqld restart```
- Заходим в консоль Mysql c правами root  ```mysql -u root -p```
- Настраиваем репликацию с MASTER
```
CHANGE MASTER TO MASTER_HOST='192.168.50.10', MASTER_USER='repl', MASTER_PASSWORD='oTUSlave#2020', MASTER_LOG_FILE='binlog.000004', MASTER_LOG_POS=156, GET_MASTER_PUBLIC_KEY = 1;
```
- Проверяем статус SLAVE сервера ```SHOW SLAVE STATUS\G```
- Выходим из консоли MySQL ```exit```
- Сделаем логический бэкап ```mysqldump --u root -p -all-databases --no-create-info > dump-data.sql```
- Сделаем физический бэкап бинлогов в текущий каталог ```mysqlbinlog --read-from-remote-server --host=10.128.0.48 --stop-never binlog.000001
