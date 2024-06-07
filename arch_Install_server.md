# Configuración del Servidor en Arch Linux

## Actualizar el Sistema
Para actualizar sistema ejecutar:

```sh
sudo pacman -Syu
```

```sh

```
__________________________________________

##Instalar y configurar ssh

```sh
sudo pacman -S openssh
sudo systemctl start sshd
sudo systemctl enable sshd
sudo pacman -S ufw
sudo ufw allow 22/tcp
sudo ufw status verbose
sudo ufw disable
sudo ufw enable

```

__________________________________________

## Instalar Node.js y npm:

```sh
sudo pacman -S nodejs npm
node -v
npm -v

```
__________________________________________

## Instalar MariaDB (Cliente y Servidor)

```sh
sudo pacman -S mariadb
sudo mariadb-install-db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo mysql_secure_installation
```
Para conectarte al cliente:
```sh
sudo mysql -u root -p
```
__________________________________________

## Conectarse por ssh:
Reemplazar ip y user por sus correspondientes.
```sh
ssh user@ip 
```






__________________________________________

## Inicializar la database:

Pimero debes crear la base de datos:

```sh
-- Crear la base de datos
CREATE DATABASE segucomm_db;
-- Crear el usuario con una contraseña segura
CREATE USER 'server_user'@'localhost' IDENTIFIED BY '6L@11Sk32gA#';
-- Asignar todos los privilegios al usuario en la base de datos
GRANT ALL PRIVILEGES ON segucomm_db.* TO 'server_user'@'localhost';
-- Aplicar los cambios
FLUSH PRIVILEGES;
-- Salir de MariaDB
exit
```

__________________________________________

## Clonar repo del backend:

Pimero debes crear la base de datos y el usuario:

```sh
git clone https://github.com/ReplacedSpace17/BackendSegucom
cd BackendSegucom
npm install
```

Despues ejecutar el script que carga datos a la db donde BackendSegucom/seed/seedDB.sql es la ubicacion del script, puedes consultarla con pwd
```sh
sudo mysql -u root -p segucomm_db < ubicacion

```

Probar que el backend funciona, ejecutar en la carpeta de BackendSegucom:
```sh
node index.js
```
Abrir un navegador y validar que está activo /test

__________________________________________



## Configurar un service

```sh
sudo nano /etc/systemd/system/backendsegucom.service

```
Agregar este contenido:


```sh

[Unit]
Description=Backend Segucom Node.js Application
After=network.target

[Service]
ExecStart=/usr/bin/node /home/rs17/Documentos/Despliegue/BackendSegucom/index.js
WorkingDirectory=/home/rs17/Documentos/Despliegue/BackendSegucom
Restart=always
User=rs17
Group=rs17
Environment=PATH=/usr/bin:/usr/local/bin
Environment=NODE_ENV=production
# Si estás usando pm2, el comando sería algo como:
# ExecStart=/usr/bin/pm2 start /home/rs17/Documentos/Despliegue/BackendSegucom/index.js --name backendsegucom

[Install]
WantedBy=multi-user.target


```

Recarga la configuración:

```sh
sudo systemctl daemon-reload
```

Activar el service
```sh
sudo systemctl enable backendsegucom
sudo systemctl start backendsegucom
sudo systemctl status backendsegucom
```


__________________________________________



## Configura el portforwarding

Obtener la ip publica

```sh
curl ifconfig.me

```
Ir a la puerta de enlace y agregar las reglas


En el server permitir la entrada
```sh
sudo ufw allow 443
sudo ufw reload

```

reemplazar la url del backend config en mapas por la ip publica








