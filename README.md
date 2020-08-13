# Cài đặt WSL 1 trên Windows 10

## Update windows phiên bản mới nhất

## Kích hoạt Windows Subsystem Linux

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

## Mở windows store lên cài đặt Ubuntu 18.04

## Khởi động sau khi cài đặt

## Login vào WSL với user mặc định là root

```powershell
ubuntu1804 config --default-user root
```

## Cài đặt các gói cần thiết để chạy một webserver trên Ubuntu

```shell
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get dist-upgrade -y
sudo apt-get autoremove -y
sudo apt -y install software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo add-apt-repository ppa:ondrej/nginx
sudo apt-key adv --fetch-keys 'https://mariadb.org/mariadb_release_signing_key.asc'
sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://ftp.yz.yamagata-u.ac.jp/pub/dbms/mariadb/repo/10.5/ubuntu bionic main'
sudo apt-get update -y
apt install nginx redis mariadb-server php7.4 php7.4-fpm php7.4-mysql php7.4-mbstring -y
```

## Cho phép mysql đăng nhập root với password null

```shell
mysql -u root
```

```sql
SELECT `user`, `plugin`, `host`, `password` FROM `mysql.user` WHERE `user` = 'root';
ALTER USER 'root'@'localhost' IDENTIFIED BY '';
FLUSH PRIVILEGES;
```

## Start service

```shell
service nginx start
service php7.4-fpm start
service mariadb start
service redis-server start
```
