# Otus_MySQL
1. Установка MYSQL
- Обновление системы yum -y update
- Установка программ yum -y wget

1.1) Выключение selinux
- setenforce 0
- sed -i "s/SELINUX=enforcing/SELINUX=permissive/" /etc/selinux/config
