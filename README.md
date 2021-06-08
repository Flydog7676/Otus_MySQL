# Otus_MySQL
1. Установка MYSQL

  1.1) Обновление системы yum -y update
  1.2) Установка программ yum -y wget
  1.3) Выключение selinux
  
- setenforce 0
- sed -i "s/SELINUX=enforcing/SELINUX=permissive/" /etc/selinux/config
