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
apt install zip unzip nginx redis mariadb-server php7.4 php7.4-fpm php7.4-mysql php7.4-mbstring php7.4-xml php7.4-bcmath php7.4-zip composer -y
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php composer-setup.php --install-dir=/usr/bin --filename=composer
git config --global user.name "Nguyen Tran Chung"
git config --global user.email nguyentranchung52th@gmail.com
```

## Start service

```shell
service nginx start
service php7.4-fpm start
service mariadb start
service redis-server start
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

## VScode truy cập vào WSL

Cài extension: [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)

Trong command line gõ: ```code .```

## Thêm site laravel/php

Thêm 1 site mới bằng cách tạo file có tên: ```xxx.conf``` trong thư mục */etc/nginx/sites-enabled*

File mẫu xem [tại đây](https://github.com/nguyentranchung/wsl1/blob/master/site.conf)

## Chỉnh sửa file hosts

Mở file hosts: ```C:\Windows\System32\drivers\etc\hosts```

Thêm dòng:
127.0.0.1 chungnguyen.test

Lưu lại với quyền admin, nếu không được thì copy file hosts ra desktop sửa, sau đó dán (paste) lại thư mục ```C:\Windows\System32\drivers\etc```

## Lưu ý

Đối website laravel cần comment out dòng 155 ở file: vendor/laravel/framework/src/Illuminate/Filesystem/Filesystem.php
```php
chmod($tempPath, 0777 - umask());
```
Lý do là vì wsl toàn bộ files/folders có quyền 777 và không được sửa nên các thao tác chmod trong PHP sẽ xảy ra lỗi.
