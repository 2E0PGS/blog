headers

# Using gparted to extend a LVM partition

## Boot options

Choose gparted CD/ISO

## Paritioning

deactivate

expand extended

expand LV

## Shutdown and remove gparted ISO/CD

## Boot back into Ubuntu

```
sudo lvextend -l+100%FREE /dev/minecraft-vg/root

sudo resize2fs /dev/mapper/minecraft--vg-root
```
