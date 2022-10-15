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


