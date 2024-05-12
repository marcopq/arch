# arch
Hacer la instalación sin swap y usando btrfs.
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
## Zram
### Desactivar zswap
```
sudo bash -c "echo 0 > /sys/module/zswap/parameters/enabled"
```
Agregar `zswap.enabled=0` a los parámetros del kernel:
```
sudo  nano /etc/default/grub
```
```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
Reiniciar el sistema para que se apliquen los cambios.
### Activar zram
```
sudo  pacman -S zram-generator
```
```
sudo nano /etc/systemd/zram-generator.conf
```
```
[zram0]
zram-size = ram / 2
compression-algorithm = zstd
swap-priority = 100
fs-type = swap
```
```
sudo systemctl daemon-reload
```
```
sudo systemctl start systemd-zram-setup@zram0.service
```
Para comprobar si esta activa la zram, hay 2 formas:
```
systemctl status systemd-zram-setup@zram0.service
```
```
zramctl
```
### Optimizar zram
```
sudo nano /etc/sysctl.d/99-vm-zram-parameters.conf
```
```
vm.swappiness = 180
vm.watermark_boost_factor = 0
vm.watermark_scale_factor = 125
vm.page-cluster = 0
```
## Post Instalación
- 
