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
sudo ufw allow OpenSSH
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



## Lanzar a producción

```sh

```
