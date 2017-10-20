Installing a new site:

1) In the nginx folder create a new .conf file (eg. sitename.conf) and add a deve domain and point it to the code folder:

```
server {

    listen 80;
    listen [::]:80;

    server_name DOMAIN.dev;
    root /var/www/SITE-FOLDER/web;
    index index.php index.html index.htm;

    location / {
         try_files $uri $uri/ /index.php$is_args$args;
    }

    location ~ \.php$ {
        try_files $uri /index.php =404;
        fastcgi_pass php-upstream;
        fastcgi_index index.php;
        fastcgi_buffers 16 16k;
        fastcgi_buffer_size 32k;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }

    location /.well-known/acme-challenge/ {
        root /var/www/letsencrypt/;
        log_not_found off;
    }
}

```

2) Update your hosts file with the domain from step 1 pointing to 127.0.0.1

3) Reload your nginx container, easiest way is from kitematic or run the following form the laradock root folder: docker-compose up -d nginx mysql redis beanstalkd

4) Add a .env file to the project

5) Run composer install and npm update