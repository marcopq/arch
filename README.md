# arch

## En EndeavourOS Live-CD editar:
btrfsSubvolumes:
```
/etc/calamares/modules/mount.conf
```
```
    - mountPoint: /var/cache/pacman/pkg
      subvolume: /@pkg
    - mountPoint: /.snapshots
      subvolume: /@snapshots
```
## Paquetes
```
pacman -Sy 
```
## Servicios
```
 sudo systemctl enable 
```
## BTRFS con snapshots y rollback en arch linux

https://wiki.archlinux.org/title/snapper
https://www.dwarmstrong.org/btrfs-snapshots-rollbacks/
https://github.com/Antynea/grub-btrfs/blob/master/initramfs/readme.md
## Snapper
```
pacman -S snapper snap-pac grub-btrfs inotify-tools 
```
### Crear una nueva configuración `root`:
```
sudo snapper -c root create-config /
```
This generates:
- Configuration file at `/etc/snapper/configs/root`.
- Add `root` to `SNAPPER_CONFIGS` in `/etc/conf.d/snapper`.
- Subvolume `.snapshots` where future snapshots for this configuration will be stored.
### Setup .snapshots
```
sudo btrfs subvolume list /
```
```
sudo btrfs subvolume delete .snapshots
```
```
sudo mkdir /.snapshots && sudo mount -a
```
```
sudo chmod 750 /.snapshots && sudo chown :wheel /.snapshots
```
### Automatic timeline snapshots
```
nano /etc/snapper/configs/root
```
El límite de las snap programadas por hora que se conservarán:
```
TIMELINE_MIN_AGE="1800"
TIMELINE_LIMIT_HOURLY="5"
TIMELINE_LIMIT_DAILY="7"
TIMELINE_LIMIT_WEEKLY="0"
TIMELINE_LIMIT_MONTHLY="0"
TIMELINE_LIMIT_YEARLY="0"
```
### Servicios
Con el demon `cron` activado ocurren las snapshots programadas, por defecto arch usa systemd timers. Otra manera es activar el sevicio de systemd. No deben estar los 2 activados al mismo tiempo:
```
sudo systemctl enable --now snapper-timeline.timer
```
Servicio de limpieza:
```
sudo systemctl enable --now snapper-cleanup.timer
```
### Updatedb
Si esta instalado `locate`
```
nano /etc/updatedb.conf
```
PRUNENAMES = ".snapshots"
### Auto-update GRUB
```
sudo systemctl enable --now grub-btrfsd
```
Para bootear snaps en mode rw sin que se guarden los cambios:
```
nano /etc/default/grub-btrfs/config
```
```
GRUB_BTRFS_SNAPSHOT_KERNEL_PARAMETERS="rd.live.overlay.overlayfs=1"
```
```
sudo /etc/grub.d/41_snapshots-btrfs
```
En ese mismo archivo, verificar que encuentre el grub.cfg:
```
GRUB_BTRFS_GRUB_DIRNAME="/boot/grub"
```
### Rollback
```
yay -S btrfs-assistant
```
Bootear el snap que se quiera usar y con btrfs-assistant hacer el rollback.
### Administrar snapshots
Crear snapshot manual. Estas snapshots deben ser borradas manualmente:
```
sudo snapper -c root create -d "**Base system install**"
```
Lista de snapshots:
```
snapper -c root list
```
Comparar con este otro comando para verificar huérfanos, las listas deben tener el mismo número:
```
sudo btrfs subvolume list -o @/.snapshots
```
Borrar snapshots. `--sync` libera el espacio inmediatamente y `N` es el número de la snap. Se pueden especificar varias (60 65) y por rango (60-65). Snaps pre pro deben eliminarse juntas:
```
snapper -c root delete --sync N
```
Directorios excluidos por defecto
https://documentation.suse.com/sles/12-SP4/html/SLES-all/cha-snapper.html#snapper-dir-excludes