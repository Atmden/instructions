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
sudo apt install -y nginx
```

### Установка MySql 8

```
sudo apt install -y mysql-server
```

```
sudo mysql_secure_installation
```
```
sudo mysql -u root -p
```

### Установка PHP 8.2

```
sudo add-apt-repository ppa:ondrej/php && \
sudo apt update && \ 
sudo apt install -y php8.2-fpm php8.2-common php8.2-pgsql php8.2-gmp \
php8.2-curl php8.2-redis php8.2-intl php8.2-mbstring php8.2-xmlrpc php8.2-gd php8.2-xml php8.2-cli php8.2-zip
```

#### Для удобства работы рекомендую настроить nginx и php на работу от созданного пользователя

### Настройка Nginx

rm /etc/nginx/sites-enabled/default
rm /etc/nginx/sites-available/default
nano /etc/nginx/sites-available/site.com

nano /etc/nginx/nginx.conf
```nginx configuration
user hoster;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 2068;
        multi_accept on;
}

http {

        ##
        # Basic Settings
        ##

        sendfile on;
        tcp_nopush on;
        types_hash_max_size 2048;
        # server_tokens off;

        # server_names_hash_bucket_size 64;
        # server_name_in_redirect off;

        include /etc/nginx/mime.types;
        default_type application/octet-stream;

        ##
        # SSL Settings
        ##

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLE
        ssl_prefer_server_ciphers on;

        ##
        # Logging Settings
        ##

        access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;

        ##
        # Gzip Settings
        ##

        gzip on;

        # gzip_vary on;
        # gzip_proxied any;
        # gzip_comp_level 6;
        # gzip_buffers 16 8k;
        # gzip_http_version 1.1;
        # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

        ##
        # Virtual Host Configs
        ##

        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
}


#mail {
#       # See sample authentication script at:
#       # http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
#
#       # auth_http localhost/auth.php;
#       # pop3_capabilities "TOP" "USER";
#       # imap_capabilities "IMAP4rev1" "UIDPLUS";
#
#       server {
#               listen     localhost:110;
#               protocol   pop3;
#               proxy      on;
#       }
#
#       server {
#               listen     localhost:143;
#               protocol   imap;
#               proxy      on;
#       }
#}
root@tinki:/home/hoster#

```
