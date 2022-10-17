# LVM Storage Management

## Initial Tutorial Setup

```
lsblk /dev/sd{b,c,d}
sudo fdisk /dev/sdc
g
n
1
2048
+12.5G
n
2
26216448
52428766
w
```

![Creating some partitions on /dev/sdc for later use](./screenshots/prep_dev_sdc.png)

```
lsblk /dev/sd{b,c,d}
sudo parted /dev/sdd
mklabel gpt
y
mkpart "sdd1" 0GB 12.5GB
mkpart "sdd2" 12.5GB 25GB
print
q
```

![Creating some partitions on /dev/sdd for later use](./screenshots/prep_dev_sdd.png)

## Physical Volumes

### Create/Initialize Physical Volume(s)

#### Creating and Display Info for a Physical Volume on a unpartitioned disk

```
lsblk /dev/sd{b,c,d}
sudo pvcreate /dev/sdb
sudo pvs /dev/sdb
sudo pvdisplay /dev/sdb
sudo pvscan
```

![Creating a physical volume on /dev/sdb](./screenshots/create_pv_sdb.png)

#### Creating a Physical Volume on a Partition

```
lsblk /dev/sd{b,c,d}
sudo pvcreate /dev/sdc1
sudo pvcreate /dev/sdc2 /dev/sdd1 /dev/sdd2
sudo pvs /dev/sdc1 /dev/sdc2 /dev/sdd1 /dev/sdd2
sudo pvscan
```

![Creating physical volumes on some partitions](./screenshots/create_pv_sdcd.png)

### Remove Physical Volume(s)

```
sudo pvscan
sudo pvremove /dev/sdc2 /dev/sdd2
sudo pvscan
```

![Removed the second partition as physical volumes](./screenshots/remove_pv_cd2.png)

## Volume Groups

### Create a Volume group from physical volumes

```
sudo vgcreate vg_1 /dev/sdb /dev/sdc1 /dev/sdd1
sudo vgs vg_1
sudo vgscan
sudo vgdisplay vg_1
```

![Creating a volume group and checking it](./screenshots/create_vg_vg1.png)

### Create a Volume Group from non-initilized device

```
sudo vgcreate vg_2 /dev/sdc2
sudo vgcreate vg_3 /dev/sdd2
sudo vgs vg_2 vg_3
sudo vgscan
```

![Creating volume groups 2 and 3](./screenshots/create_vg_vg12.png)


### Removing a Volume group

```
sudo vgscan
sudo vgs
sudo vgremove vg_3
sudo vgscan
sudo vgs
```

![Removing a physical volume](./screenshots/remove_vg_vg3.png)

### Merge two volume groups

```
sudo vgs
sudo vgscan
sudo vgmerge vg_1 vg_2
sudo vgs
sudo vgscan
```

![Merge to volume groups](./screenshots/merge_vg12.png)

### Extend a Volume Group

```
sudo vgs vg_1
sudo vgextend vg_1 /dev/sdd2
sudo vgs vg_1
```

![Extending a Volume group](./screenshots/extend_vg1.png)

### Rename a Volume Group

```
sudo vgs
sudo vgscan
sudo vgrename vg_1 data-vg
sudo vgs
sudo vgscan
```

![Renaming a Volume Group](./screenshots/rename_vg1.png)

## Logical Volumes

### Create a Logical Volume

```
sudo lvs
sudo lvscan
sudo lvcreate --size 15G -n data-lv data-vg
sudo lvcreate --size 15G -n backup-lv data-vg
sudo lvs
sudo lvscan
```

![Create two Logical Volumes in data-vg](./screenshots/create_lvs.png)

```
sudo lvdisplay /dev/data-vg/data-lv
```

![Display more info about data-lv](./screenshots/display_lv_data.png)

### Extend a Logical Volume

#### Extend Logical Volume by Size

```
lsblk /dev/sd{b,c,d}
sudo lvs
vgs
sudo lvextend --size +10G /dev/date-vg/data-lv
sudo lvs
sudo vgs
```

![Extending data-lv by 10G](./screenshots/extend_lv_size.png)

```
lsblk /dev/sd{b,c,d}
```

![Display how the lvs use the block devices](./screenshots/lsblk_lvextend1.png)

#### Extend Logical Volume by a Physical Volume

```
lsblk /dev/sd{b,c,d}
sudo lvextend /dev/data-vg/data-lv /dev/sdd2
lsblk /dev/sd{b,c,d}
sudo lvs
```

![Extend a logical volume by a physical volume](./screenshots/extend_lv_pv.png)

#### Extend Logical Volume by percentage of free left in Volume Group

```
sudo vgs
sudo lvextend --extents +100%FREE /dev/data-vg/backup-lv
sudo vgs
lsblk /dev/sd{b,c,d}
```

![Extend by using percentage of free space in vg](./screenshots/extend_lv_vg.png)

## Quick Test of Logical Volume

```
sudo mkfs.ext4 /dev/data-vg/data-lv
sudo mkdir /data
sudo mount /dev/data-vg/data-lv /data
lsblk /dev/sd{b,c,d}
mount | tail -n 1
```

![Make a fs and mounting data-lv](./screenshots/test_lv_data.png)

# References

1. man pages


