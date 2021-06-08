# Otus_MySQL
1. Установка MYSQL
- Обновление системы ```yum -y update```
- Установка доп программ ```yum -y wget```
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
