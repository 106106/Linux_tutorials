# Managing Classical Storage

## Process Overview 

1. Physically install the storage device.
2. Confirm that Linux sees the storage device by using tools such as hwinfo, lsblk, and lsscsi.
3. Partition the drive with fdisk or parted.
4. Add a filesystem such as ext4 or XFS by using the mkfs command.
5. Manually test mount the storage capacity to a mount point by using the mount command.
6. Ensure the storage space is usable by copying actual data to the location using cp.
7. Configure automatic mounting so that the capacity is available when the system boots.

## lsblk

```
lsblk
```

![listing the block devices availible for storage](./screenshots/lsblk_command1.png)

## fdisk command

### Listing the partition tables

```
sudo fdisk -l /dev/sda
sudo fdisk -l /dev/sdb
sudo fdisk -l /dev/sdc
sudo fdisk -l /dev/sdd
```

![listing partitions with fdisk](./screenshots/fdisk_list.png)

### MBR Partitioning

#### Creating a first partition

```
sudo fdisk /dev/sdb
o
n
p
1
2048
+10G
w
```

![Created a 10G MBR/DOS partition on sdb](./screenshot/fdisk_mbr_sdb1.png)

![Listing sdb1](./screenshots/fdisk_list_sdb1.png)

![lsblk sdb1](./screenshots/lsblk_command2.png)

#### Creating some more partitions

```
sudo fdisk /dev/sdb
n
p
2
<RETURN>
+5G
n
3
<RETURN>
+5G
w
```

![Creating more partitons](./screenshots/fdisk_mbr_sdb2_and_3.png)

![Listing /dev/sdb](./screenshots/lsblk_command3.png)

#### Creating a extended partition

```
sudo fdisk /dev/sdb
n
e
4
<RETURN>
<RETURN>
w
```

![Created an extended partition](./screenshots/fdisk_mbr_sdb4.png)

![Listing /dev/sdb](./screenshots/lsblk_command4.png)

#### Creating some logical partition

```
sudo fdisk /dev/sdb
n
<RETURN>
+1G
n
<RETURN>
+1G
n
<RETURN>
+2G
w
```

![Created some logical partitions](./screenshots/fdisk_mbr_logical.png)

![Listing /dev/sdb](./screenshots/lsblk_command5.png)

### GPT Partitioning

```
sudo fdisk /dev/sdc
g
n
1
2048
+12G
n
2
<RETURN>
w
```

![Created GPT partitions](./screenshots/fdisk_gpt_sdc.png)

![Listing /dev/sdc](./screenshots/lsblk_command6.png)

## parted command

### Creating msdos/MBR Partitions 

```
sudo parted /dev/sdd
mklabel msdos
y
mkpart primary 0GB 10GB
print
mkpart primary 10GB 25GB
print
q
```

![Making msdos partitions with parted](./screenshots/parted_mbr_sdd.png)

![Listinf /dev/sdd](./screenshots/lsblk_command7.png)

### Creating GPT partitions

```
sudo parted /dev/sdd
mklabel gpt
y
mkpart "sdd1" 0GB 15GB
print
mkpart "new_data" 20GB 25GB
print
q
```

![Making gpt patitions](./screenshots/parted_gpt_sdd.png)

![Listing /dev/sdd](./screenshots/lsblk_command8.png)

## mkfs command

### Make ext4

```
sudo mkfs -t ext4 /dev/sdb1
```

![Making an ext4 filesystem](./screenshots/mkfs_ext4_sdb1.png)

```
sudo mkfs.ext4 /dev/sdb2
```

![Making an ext4 filesystem](./screenshots/mkfs_ext4_sdb2.png)

### Make xfs

```
sudo mkfs -t xfs /dev/sdb3
sudo mkfs.xfs /dev/sdb5
```

![Making xfs filesystems](./screenshots/mkfs_xfs_sdb3&5.png)

## Mounting and Unmounting

### Temporarily mounting a filesystem

#### Mount

```
mkdir data_ext4_sdb1
mkdir data_ext4_sdb2
mkdir data_xfs_sdb3
mkdir data_xfs_sdb5
sudo mount /dev/sdb1 data_ext4_sdb1/
sudo mount /dev/sdb2 data_ext4_sdb2/
sudo mount /dev/sdb3 data_ext4_sdb3/
sudo mount /dev/sdb5 data_ext4_sdb5/
lsblk /dev/sdb
```

![Mounting some file systems](./screenshots/mounting_sdb1235.png)

```
sudo mount | tail -n 5
echo "test data" > test.txt
ls -l data_ext4_sdb1/
ls -ld data_ext4_sdb1/
sudo cp test.txt data_ext4_sdb1/
sudo cat data_ext4_sdb1/test/txt
```

![Checking the partitions/filesystems were mounted](./screenshots/temporary_mount_check.png)

Now if we reboot, these partitions/filesystems will no longer be mounted since we haven't added them to /etc/fstab.

#### umount

```
sudo umount /dev/sdb1
sudo umount data_ext4_sdb2
sudo umount dev/sdb3
sudo umount data_xfs_sdb5
ls -l data_ext4_sdb1/
mount | tail -n 5
lsblk /dev/sdb
```

![unmounting some partitions](./screenshots/umount_sdb1235.png)

# References

1. man pages
2. comptia linux+ study guide
