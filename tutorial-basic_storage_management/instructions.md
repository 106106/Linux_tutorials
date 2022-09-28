# Basic Storage Management




















Storage Deployment Summary

Because there are many steps necessary to add storage capacity to a Linux system, it's useful to see a summary of the process:

    Physically install the storage device.
    Confirm that Linux sees the storage device by using tools such as hwinfo, lsblk, and lsscsi.
    Partition the drive with fdisk or parted.
    Add a filesystem such as ext4 or XFS by using the mkfs command.
    Manually test mount the storage capacity to a mount point by using the mount command.
    Ensure the storage space is usable by copying actual data to the location using cp.
    Configure automatic mounting so that the capacity is available when the system boots.


