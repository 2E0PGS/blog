Using `vmware-mount` which seemed to come with vmware player.

List partitions

`sudo vmware-mount -p mydisk.vmdk`

```
Nr Start Size      Type Id System
1  2024  947890176 BIOS 7  HPFS/NTFS
```

Create mount folder

`sudo mkdir /mnt/myfiles`

Mount partition in that folder

`sudo vmware-mount mydisk.vmdk 1 /mnt/myfiles`
