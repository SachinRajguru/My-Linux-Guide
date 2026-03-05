
# Disk and Storage Management in Linux

## Introduction to Disk and Storage Management
Managing disks and storage efficiently is crucial for system performance and stability. Linux provides various commands to monitor, partition, format, mount, and manage disk storage. Key concepts include block devices, filesystems (e.g., ext4, XFS), and logical volumes for flexibility.

**Key Concepts**:
- **Block Devices**: Represent disks/partitions (e.g., `/dev/sda`).
- **Filesystems**: Organize data on partitions (e.g., ext4 for general use, XFS for large files).
- **LVM**: Allows resizing and snapshots without downtime.
- **Swap**: Virtual memory for RAM overflow.

## Index of Commands Covered

### Viewing Disk Information
- `lsblk` – Display block devices
- `fdisk -l` – List disk partitions
- `blkid` – Show UUIDs of devices
- `df -h` – Check disk space usage
- `du -sh /path` – Show size of a directory

### Partition Management
- `fdisk /dev/sdX` – Create and manage partitions (MBR)
- `parted /dev/sdX` – Alternative to `fdisk` for GPT disks
- `mkfs.ext4 /dev/sdX1` – Format a partition as ext4
- `mkfs.xfs /dev/sdX1` – Format a partition as XFS

### Mounting and Unmounting
- `mount /dev/sdX1 /mnt` – Mount a partition
- `umount /mnt` – Unmount a partition
- `mount -o remount,rw /mnt` – Remount a partition as read-write

### Logical Volume Management (LVM)
- `pvcreate /dev/sdX` – Create a physical volume
- `vgcreate vg_name /dev/sdX` – Create a volume group
- `lvcreate -L 10G -n lv_name vg_name` – Create a logical volume
- `mkfs.ext4 /dev/vg_name/lv_name` – Format an LVM partition
- `mount /dev/vg_name/lv_name /mnt` – Mount an LVM partition
- `lvextend -L +5G /dev/vg_name/lv_name` – Resize a logical volume (extend)
- `resize2fs /dev/vg_name/lv_name` – Resize the filesystem after extending

### Swap Management
- `mkswap /dev/sdX` – Create a swap partition
- `swapon /dev/sdX` – Enable swap space
- `swapoff /dev/sdX` – Disable swap space

## Viewing Disk Information
### Using `lsblk`
List all block devices:
```bash
lsblk
```
- **Sample Output**:
  ```
  NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
  sda      8:0    0  100G  0 disk
  ├─sda1   8:1    0   96G  0 part /
  └─sda2   8:2    0    4G  0 part [SWAP]
  sdb      8:16   0   20G  0 disk
  ```

### Using `fdisk`
View partition details:
```bash
fdisk -l
```
- **Sample Output** (abbreviated):
  ```
  Disk /dev/sda: 100 GiB, 107374182400 bytes, 209715200 sectors
  Units: sectors of 1 * 512 = 512 bytes
  Sector size (logical/physical): 512 bytes / 512 bytes
  I/O size (minimum/optimal): 512 bytes / 512 bytes
  Disklabel type: dos
  Device     Boot Start       End   Sectors Size Id Type
  /dev/sda1  *     2048  209715199 209713152 100G 83 Linux
  ```

### Using `df`
Check available disk space:
```bash
df -h
```
- **Sample Output**:
  ```
  Filesystem      Size  Used Avail Use% Mounted on
  /dev/sda1        96G   20G   72G  22% /
  ```

### Using `du`
Find the size of a directory:
```bash
du -sh /var/log
```
- **Sample Output**: `1.2G	/var/log`

## Partition Management
### Creating a Partition with `fdisk`
```bash
fdisk /dev/sdX
```
- Follow prompts: `n` (new), `p` (primary), size, `w` (write).
- **Caution**: Partitioning can destroy data—backup first.

### Formatting a Partition
Format as ext4:
```bash
mkfs.ext4 /dev/sdX1
```
Format as XFS:
```bash
mkfs.xfs /dev/sdX1
```
- **Tip**: Use `tune2fs` or `xfs_info` for filesystem details post-format.

## Mounting and Unmounting
### Mount a Partition
```bash
mount /dev/sdX1 /mnt
```
### Unmount a Partition
```bash
umount /mnt
```
### Remount a Partition
```bash
mount -o remount,rw /mnt
```
- **Tip**: Use `/etc/fstab` for permanent mounts.

## LVM Management
LVM provides flexible storage management.
### Create a Physical Volume
```bash
pvcreate /dev/sdX
```
### Create a Volume Group
```bash
vgcreate vg_name /dev/sdX
```
### Create a Logical Volume
```bash
lvcreate -L 10G -n lv_name vg_name
```
### Format and Mount the Logical Volume
```bash
mkfs.ext4 /dev/vg_name/lv_name
mount /dev/vg_name/lv_name /mnt
```
### Resize a Logical Volume
Extend size:
```bash
lvextend -L +5G /dev/vg_name/lv_name
resize2fs /dev/vg_name/lv_name  # For ext4; use xfs_growfs for XFS
```
- **Tip**: LVM allows snapshots with `lvcreate -s`.

## Swap Management
### Create a Swap Partition
```bash
mkswap /dev/sdX
```
### Enable Swap
```bash
swapon /dev/sdX
```
### Disable Swap
```bash
swapoff /dev/sdX
```
- **Tip**: Check swap with `free -h`; add to `/etc/fstab` for persistence.

## Additional Notes - When to Use fdisk, mount, or Both
### Check Available Disks
Before creating or mounting anything, always check what block devices exist:
```bash
lsblk
```
### Example output:
| NAME | MAJ:MIN | RM | SIZE | RO | TYPE | MOUNTPOINT |
|------|---------|----|------|----|------|------------|
| sda  | 8:0    | 0  | 100G | 0  | disk |            |
| ├─sda1| 8:1   | 0  | 96G  | 0  | part | /          |
| └─sda2| 8:2   | 0  | 4G   | 0  | part | [SWAP]     |
| sdb  | 8:16  | 0  | 20G  | 0  | disk |            |

- `sda` → existing disk (already partitioned)
- `sdb` → new disk, no partitions yet

### When to use `fdisk`
Use `fdisk` when:
- The disk is brand new and has no partitions
- You want to create `/dev/sdb1`, `/dev/sdb2`, etc.

Inside `fdisk`:
1. Press `n` → create a new partition
2. Press `w` → write changes

Then confirm:
```bash
lsblk
```

### When to Use `mount`
Use `mount` when:
- The partition already exists and is formatted
- You just want to make it accessible
```bash
sudo mkdir /mnt/mydisk
sudo mount /dev/sdb1 /mnt/mydisk
```
- Now your disk is available at `/mnt/mydisk`.

### When to Use fdisk + mount (Full Setup)
Use `fdisk + mkfs + mount` when:
- The disk is completely new
- You need to partition → format → mount it
```bash
# 1. Check available disks
lsblk
# 2. Create partition
sudo fdisk /dev/sdb
# 3. Format the partition
sudo mkfs.ext4 /dev/sdb1
# 4. Mount it
sudo mkdir /data
sudo mount /dev/sdb1 /data
```

### Quick Reference
| Use Case                     | Command(s)                  |
|-------------------------------|-----------------------------|
| View disks and partitions     | `lsblk`                     |
| Partition a new disk          | `fdisk`                     |
| Mount an existing partition   | `mount`                     |
| Full setup (new disk)         | `fdisk + mkfs + mount`      |

## Troubleshooting and Best Practices
- **Disk Not Mounting?** Check `dmesg | tail` for errors; ensure filesystem is supported.
- **Low Space?** Use `du -sh /*` to find large dirs; clean with `rm` or move to external storage.
- **LVM Issues?** Use `pvdisplay`, `vgdisplay`, `lvdisplay` for status.
- Backup data before partitioning or resizing.
- Use GPT (`parted`) for disks >2TB; MBR is limited.
- Test in a virtual environment (e.g., Docker or VM) first.
- Monitor with `df -h` regularly; automate checks with cron.

## Conclusion
Disk and storage management is key to Linux administration. Tools like `fdisk`, `mount`, and LVM enable efficient setup and maintenance. For more, see [Ubuntu Storage Guide](https://help.ubuntu.com/community/InstallingANewHardDrive) or [LVM Docs](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_and_managing_logical_volumes/index).

---
