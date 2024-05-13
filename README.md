# arch
Hacer la instalación sin swap y usando btrfs. Si ya hay una partición EFI de otro OS, se puede usar una partición btrfs y con calamares elegir la opción de reemplazar partición (se perderá el contenido de la partición btrfs), el instalador usará automáticamente si borrar la partición EFI sin afectar al otro OS y el dual boot.
## En el Live-CD editar:
```
sudo reflector --country Chile --age 6 --sort rate --save /etc/pacman.d/mirrorlist; cat /etc/pacman.d/mirrorlist
```
## Paquetes
```
pacman -Sy grub-btrfs inotify-tools ntfs-3g xpadneo-dkms discover shairport-sync htop ripgrep duperemove pkgfile flatpak flatpak-kcm xdg-desktop-portal-kde xdg-desktop-portal-gtk partitionmanager syncthing
```
```
paru -S uxplay
```
```
flatpak install org.keepassxc.KeePassXC com.brave.Browser md.obsidian.Obsidian org.gnome.World.PikaBackup net.davidotek.pupgui2 com.usebottles.bottles org.kde.haruna org.kde.haruna
```
## Servicios
```
 sudo systemctl enable --now syncthing.service
```
## GRUB
### Detectar otro OS y agregar entrada en el grub
Editar `/etc/default/grub` y descomentar:
```
GRUB_DISABLE_OS_PROBER=false
```
Generar nueva configuración grub:
```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
## Arreglos
Mejora rendimiendo con Gamescope:
```
 setcap 'CAP_SYS_NICE=eip' $(which gamescope)
```
## CachyOS Hello App
Desde la aplicación instalar Snapper y Gaming Packages
## BTRFS snapshots
Usar Btrfs Assistant para configurar y crear snapshots
## Shairport-sync
Abrir puertos y guardar cambios
```
sudo firewall-cmd --add-port=3689/tcp
sudo firewall-cmd --add-port=5353/udp
sudo firewall-cmd --add-port=5000/tcp
sudo firewall-cmd --add-port=6000-6009/udp
```
Ejecución
```
shairport-sync -o pa -a NOMBRE
```
## Instalar fuentes de Windows
Desde una instalación de Windows copiar fuentes desde `Windows/Fonts` hacia:
```
~/.local/share/fonts
```
Ver si el sistema las reconoce con:
```
fc-list ':' file
```
## Post Instalación
- 
