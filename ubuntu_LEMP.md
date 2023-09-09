# Установка LEMP на Ubuntu 22.04

```
apt update & apt -y upgrade
```
### Создаем пользователя
```
adduser hoster
```

```
usermod -aG sudo hoster
```
Подключаем под новым пользователем
```
mkdir .ssh
nano .ssh/authorized_keys // Вставляем ключ для авторизации по ключу
```

### Настраиваем ssh

```
nano /etc/ssh/sshd_config
```

```
Port 28 // меняем порт подключения
PubkeyAuthentication yes // разрешаем использовать публичный ключ для авторизации
```


```
nano /etc/ssh/sshd_config
```
```
PermitRootLogin no // отключаем авторизаию root
PasswordAuthentication no // отключаем авторизацию по паролю
```

```
systemctl restart sshd
```

### Установка Nginx 1.18

```
sudo apt install -y nginx certbot unzip mc
```

### Установка MySql 8

```
sudo apt install -y mysql-server
```
### Настройка Mysql
```
sudo mysql_secure_installation

sudo mysql -u root -p
```
```mysql
UNINSTALL COMPONENT "file://component_validate_password";

CREATE USER 'hoster'@'localhost' IDENTIFIED BY 'Pa$$w0rd';

GRANT ALL ON *.* TO 'hoster'@'localhost';

FLUSH PRIVILEGES;

```


### Установка PostgreSQL
```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget -qO- https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo tee /etc/apt/trusted.gpg.d/pgdg.asc &>/dev/null
apt update
apt install postgresql postgresql-client -y

sudo -u postgres psql

postgres=# CREATE USER [Username] WITH ENCRYPTED PASSWORD '[Password]';
CREATE DATABASE [DatabaseName];
GRANT ALL PRIVILEGES ON DATABASE [DatabaseName] TO [Username];
ALTER DATABASE [DatabaseName] OWNER TO [Username];
\connect [DatabaseName];
CREATE SCHEMA [SchemaName] AUTHORIZATION [Username];

/etc/postgresql/15/main/postgresql.conf

listen_addresses = '*'

/etc/postgresql/15/main/pg_hba.conf

# IPv4 local connections:
host    all             all             0.0.0.0/0            scram-sha-256


systemctl restart postgresql
```

### Установка PHP 8.2

```
sudo add-apt-repository ppa:ondrej/php && \
sudo apt update && \ 
sudo apt install -y  php8.2-fpm php8.2-cgi php8.2-common php8.2-pgsql php8.2-gmp \
php8.2-curl php8.2-redis php8.2-intl php8.2-mbstring php8.2-xmlrpc php8.2-gd php8.2-xml php8.2-cli php8.2-zip php8.2-mysqli php8.2-bz2
```
 

#### Для удобства работы рекомендую настроить nginx и php на работу от созданного пользователя

### Настройка Nginx

Идем на сайт https://www.digitalocean.com/community/tools/nginx и проходим все шаги


### Настройка PHP

nano /etc/php/8.2/fpm/pool.d/www.conf

```
user = hoster
group = hoster


listen.owner = hoster
listen.group = hoster

```

nano /etc/php/8.2/fpm/php.ini

```
memory_limit = 256M

post_max_size = 256M
upload_max_filesize = 256M
```
systemctl restart php8.2-fpm

sudo chown -R hoster:hoster /home/hoster/www


cd ~ && \
curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php && \
sudo php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer





### phpMyAdmin

phpMyAdmin устанавливаем когда уже развернули сайт, т.к. размещать его будем в директории сайта

```
wget https://files.phpmyadmin.net/phpMyAdmin/5.2.1/phpMyAdmin-5.2.1-all-languages.zip
unzip phpMyAdmin-5.2.1-all-languages.zip
sudo mv phpMyAdmin-5.2.1-all-languages /home/новый_юзер/www/папка_с_сайтом/public/phpmyadmin
rm phpMyAdmin-5.2.1-all-languages.zip
cd /home/новый_юзер/www/папка_с_сайтом/public/phpmyadmin
cp config.sample.inc.php config.inc.php
https://www.motorsportdiesel.com/tools/blowfish-salt/pma/

```

### Fish shell

sudo apt-add-repository ppa:fish-shell/release-3 && \
sudo apt update && \
sudo apt install fish -y

echo /usr/bin/fish | sudo tee -a /etc/shells
chsh -s /usr/bin/fish
