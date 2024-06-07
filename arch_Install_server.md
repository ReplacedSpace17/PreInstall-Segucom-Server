# Configuración del Servidor en Arch Linux

## Actualizar el Sistema
```sh
sudo pacman -Syu
```

________________________
##Instalar y configurar ssh
```sh
sudo pacman -S openssh
sudo systemctl start sshd
sudo systemctl enable sshd

# (Opcional) Configurar firewall para permitir SSH
sudo pacman -S ufw
sudo ufw allow OpenSSH
sudo ufw enable
