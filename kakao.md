### this is the test a nd 
## this is test
# this is test
```

server {
    listen 80 default_server;
    listen [::]:80 default_server;
    root /var/www/html;
    
    # Add index.php to the list if you are using PHP

    index index.html index.htm index.nginx-debian.html;
    server_name hellonode;

    location ^~ /assets/ {
            gzip_static on;
            expires 12h;
            add_header Cache-Control public;
    }

    location / {
            proxy_http_version 1.1;
            proxy_cache_bypass $http_upgrade;

            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;

            proxy_pass http://localhost:1998;
    }
}

///laravel project

server {
    listen 80;
    server_name _ we www.we;
    root /var/www/html/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```
