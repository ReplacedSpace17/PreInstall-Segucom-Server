

# Arch 

## Actualizar el Sistema
Para actualizar sistema ejecutar:
```sh
sudo pacman -Syu
```

## Instalar y configurar el Cliente Dynu en tu Servidor
Primeramente es necesario instalar el cliente de dynu:
```sh
sudo pacman -S ddclient
```

Para configurar nuestro cliente es necesario editar el archivo de configuración: 
```sh
sudo nano /etc/ddclient/ddclient.conf
```
Copia y pega la siguiente configuración, reemplaza tu usuario, contraseña y dominio en caso de ser necesario.
```sh
daemon=300
syslog=yes
mail-failure=root
pid=/var/run/ddclient.pid
ssl=yes
use=web, web=checkip.dynu.com/, web-skip='IP Address'
protocol=dyndns2
server=api.dynu.com
login=miranda24
password='@322915Memotec'
segubackend.com
```
Habilitar y Ejecutar ddclient:
```sh
sudo systemctl enable ddclient
sudo systemctl start ddclient
```
Verifica que está corriendo:

```sh
sudo systemctl status ddclient
```
Puedes ingresar a https://www.dynu.com/en-US/ControlPanel/DDNS y verificar que aparezca la ip pública de tu máquina.
DYNU - 

User: 
miranda24 
Pass:
@322915Memotec

Puedes usar el siguiente comando para validar tu ip publica:

```sh
curl -4 ifconfig.me
```

## Cambiar los servers name de hostgator
Ingresa al portal del hostgator https://cliente.hostgator.mx/dominios/
Debes cambiar los servers name del dominio que deseas configurar por los de dynu.

```sh
ns1.dynu.com
ns2.dynu.com
```
Esperar hasta 72 hrs para validar que la configuración del dns haya cambiado en hostgator

Solo configurar el portforwarding en el modem y acceder como
http://segubackend.com:3000/test - Cambiar el puerto según la app



NO ES NECESRAIO__________________________________

## Instalar NGINX
Primeramente debemos instalas NGINX
```sh
sudo pacman -S nginx
```
Configuracion de NGINX:
Aumentar el tamaño del hash de tipos (types_hash_max_size)
```sh
sudo nano /etc/nginx/nginx.conf
```

Copiar y pegar la config
```sh
worker_processes  auto;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;
    error_log   /var/log/nginx/error.log;

    sendfile        on;
    tcp_nopush     on;
    tcp_nodelay    on;

    keepalive_timeout  65;

    gzip  on;

    # Aumentar el tamaño máximo del hash de tipos para resolver la advertencia
    types_hash_max_size 2048;

    include /etc/nginx/conf.d/*.conf;

    server {
        listen       80;
        server_name  localhost;

        location / {
            root   /usr/share/nginx/html;
            index  index.html index.htm;
        }

        error_page  404              /404.html;
        location = /404.html {
            root   /usr/share/nginx/html;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   /usr/share/nginx/html;
        }
    }
}


```


Crea un nuevo directorio para la configuración del sitio:
```sh
sudo mkdir /etc/nginx/conf.d

```

Crea un archivo de configuración para tu dominio en /etc/nginx/sites-available/segubackend.com:
```sh
sudo nano /etc/nginx/conf.d/segubackend.conf
```

Pega la configuracion
```sh
server {
    listen 80;
    server_name segubackend.com www.segubackend.com;

    location /backend-segucom/ {
        proxy_pass http://localhost:3000/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /comunication-segucom/ {
        proxy_pass http://localhost:3001/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

```

Habilitar la Configuración:

```sh
sudo nginx -t
sudo systemctl reload nginx
```

Verificar que NGINX esta corriendo:
```sh
sudo systemctl status nginx
```

## Configurar Portforwarding
Configurar todo el trafico que entra por el puerto 80 a la direccion ip locla de mi server en el puerto 80 y por tcp




## Configurar los SSL
Instalar Certbot

```sh
sudo pacman -S certbot certbot-nginx
```
Obtener el Certificado SSL

```sh
sudo certbot --nginx -d segubackend.com -d www.segubackend.com
```
Renovar el Certificado SSL Automáticamente
```sh
sudo crontab -e
```

Añade la siguiente línea
```sh
0 0 * * * /usr/bin/certbot renew --quiet
```



```sh
sudo pacman -S nginx
```
```sh
sudo pacman -S nginx
```
```sh
sudo pacman -S nginx
```
