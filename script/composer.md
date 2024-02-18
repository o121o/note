# Composer

## 官網
[https://getcomposer.org/](https://getcomposer.org/)


## 安裝
```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
sudo mv composer.phar /usr/local/bin/composer
```

## 使用
### 初始專案/安裝專案套件
```
composer install
```
### 安裝新套件
```
composer require {套件名稱}
```
### 移除套件
```
composer remove {套件名稱}
```
### 更新全部套件
例如變更 PHP 版本後執行( **注意** 可能會有套件功能異動)
```
composer update
```
### 更新指定套件
**注意** 可能會有套件功能異動
```
composer update {套件名稱}
```
### Composer 自我更新
```
composer self-update
```
