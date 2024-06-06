#Configuración para ubuntu server




#Actualizar server
sudo apt update
sudo apt upgrade
__________________________________________________
#Instalar ssh
sudo apt update
sudo apt install openssh-server
sudo systemctl start ssh
sudo systemctl enable ssh
sudo ufw allow OpenSSH
sudo ufw enable

ssh user@ip
__________________________________________________
#Instalar node
sudo apt install nodejs npm
npm -v
node-v
__________________________________________________
#Instalar cliente mysql
sudo apt install mariadb-server
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo mysql_secure_installation
sudo mysql -u root -p

__________________________________________________
#Clonar el repo
git clone https://github.com/ReplacedSpace17/BackendSegucom
cd BackendSegucom
npm install
node index.js
__________________________________________________
#PM2 en el backend
Utiliza un gestor de procesos como PM2 para mantener tu aplicación en funcionamiento y manejar reinicios automáticos en caso de fallos.
sudo npm install -g pm2
O
 sudo npm install -g yarn
sudo yarn global add pm2
pm2 start index.js --name "BackendSegucom"
pm2 startup
pm2 save

__________________________________________________
#Configurar un Servidor Web (Proxy Reverso)
Utiliza un servidor web como Nginx para manejar las solicitudes HTTP y actuar como un proxy inverso para tu aplicación Node.js.
sudo apt install nginx
sudo nano /etc/nginx/sites-available/default

server {
    listen 80;
    server_name tu_dominio_o_ip;

    location / {
        proxy_pass http://localhost:3000;  # Cambia el puerto si tu aplicación corre en otro puerto
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}


Probar la Configuración de Nginx
Prueba la configuración de Nginx para asegurarte de que no haya errores:

sudo nginx -t
sudo systemctl restart nginx

__________________________________________________

Configurar HTTPS
Utiliza Certbot para obtener y renovar automáticamente certificados SSL gratuitos de Let's Encrypt.

Instalar Certbot
Instala Certbot y el plugin de Nginx:
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d tu_dominio
sudo certbot renew --dry-run

pm2 status
pm2 logs
pm2 monit






