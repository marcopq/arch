# arch

## En EndeavourOS Live-CD editar:
btrfsSubvolumes:
```
/etc/calamares/modules/mount.conf
```
```
    - mountPoint: /var/tmp
      subvolume: /@tmp
    - mountPoint: /var/lib/libvirt
      subvolume: /@libvirt
    - mountPoint: /.snapshots
      subvolume: /@snapshots
```
