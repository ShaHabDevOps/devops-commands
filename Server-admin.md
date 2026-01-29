# SERVER-ADMIN

## Disks / LVM / Kernel / sysctl

| Command                        | Purpose             |
| ------------------------------ | ------------------- |
| `lsblk`                        | Block devices       |
| `blkid`                        | Filesystem UUIDs    |
| `mount`                        | Mounted filesystems |
| `mount /dev/sdb1 /mnt`         | Mount disk          |
| `umount /mnt`                  | Unmount             |
| `df -Th`                       | FS type usage       |
| `fdisk -l`                     | Disk partitions     |
| `pvcreate /dev/sdb`            | Create LVM PV       |
| `vgcreate vg1 /dev/sdb`        | Create VG           |
| `lvcreate -L 10G vg1 -n lv1`   | Create LV           |
| `mkfs.ext4 /dev/vg1/lv1`       | Format LV           |
| `sysctl -a`                    | Kernel params       |
| `sysctl net.ipv4.ip_forward=1` | Enable forwarding   |
| `uname -a`                     | Kernel info         |
| `uptime -p`                    | Pretty uptime       |
