# arch
Hacer la instalación sin swap y usando btrfs. Si ya hay una partición EFI de otro OS, se puede usar una partición btrfs y con calamares elegir la opción de reemplazar partición (se perderá el contenido de la partición btrfs), el instalador usará automáticamente si borrar la partición EFI sin afectar al otro OS y el dual boot.
## En el Live-CD editar:
```
sudo reflector --country Chile --age 6 --sort rate --save /etc/pacman.d/mirrorlist; cat /etc/pacman.d/mirrorlist
```
## Paquetes
```
pacman -Sy grub-btrfs inotify-tools ntfs-3g xpadneo-dkms discover ripgrep duperemove pkgfile flatpak flatpak-kcm xdg-desktop-portal-kde xdg-desktop-portal-gtk partitionmanager syncthing trash-cli input-remapper
```
```
paru -S input-remapper-git
```
```
flatpak install org.keepassxc.KeePassXC com.brave.Browser md.obsidian.Obsidian org.gnome.World.PikaBackup net.davidotek.pupgui2 com.usebottles.bottles org.kde.haruna it.mijorus.gearlever
```
## Servicios
```
 systemctl --user enable --now syncthing.service
 sudo systemctl enable --now input-remapper
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
Cambia orden de resultados de paru para más comodidad, decomentar `BottomUp` en:
```
 sudo micro /etc/paru.conf
```
## CachyOS Hello App
Desde la aplicación instalar Snapper y Gaming Packages
## BTRFS snapshots
Usar Btrfs Assistant para configurar y crear snapshots
## Montar Particiones Extras
Crear carpetas y darles permisos:
```
sudo mkdir /mnt/Almacen
sudo chown $USER:$USER /mnt/Almacen
sudo mkdir /mnt/Extras
sudo chown $USER:$USER /mnt/Extra
```
Verificar particiones usando `lsblk -f`.
Agregar al fstab:
```
UUID=5024574F24573766					  /mnt/Almacen   ntfs   defaults,noatime,nofail,users 0 0
UUID=e1fb76ac-1a48-498c-a4a1-45f1fe765cb6 /mnt/Extra     btrfs   defaults,lazytime,commit=120,compress=zstd,nofail,noatime,users 0 0
```
Montar particiones:
```
sudo systemctl daemon-reload
sudo mount -a
```
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
- En Brave ir a `brave://flags/` habilitar Middle button autoscroll para hacer scroll más rápido.
