
- [Section 10: Disk Management (Section 10)](#Section-4-disk-management-section-10)
  - [Why Manage Disks?](#why-manage-disks)
  - [Key Concepts](#key-concepts)
  - [Commands and Steps](#commands-and-steps)
  - [Example Scenario](#example-scenario)

# Section 10: Disk Management
Disk management involves adding/removing storage (volumes) for scalability, including attaching, formatting, and mounting to prevent space shortages (e.g., from logs or apps).

## Why Manage Disks?
- Handle storage growth (e.g., logs filling up space).
- Initial storage (e.g., 8GB) fills up with logs/apps. Add volumes instead of deleting data.
- Add volumes without downtime.
- Process: Attach volume → Format → Mount → Use.

## Key Concepts
- **Block Storage**: Raw storage (e.g., EBS in AWS); not directly usable.
- **Partitions**: Divide storage (e.g., via `fdisk`).
- **File Systems**: Formats like ext4 or XFS for usability.
- **Mounting**: Attach to a directory (e.g., `/mnt`).

## Commands and Steps
1. **List Storage**:
   - `lsblk`: Lists attached disks/partitions (e.g., xvda for 8GB).
   - `fdisk -l`: Detailed partition info.

**Interview Questions**:
- How do you list all disk partitions on a Linux system?
  - Use `lsblk` or `fdisk -l`.

2. **Add Volume (AWS Example)**:
   - Create 10GB EBS volume in the same AZ as instance. (e.g., `us-east-1c`).
   - Attach to instance (device: /dev/xvdf).
   - Verify: `lsblk` shows new disk.

3. **Format and Mount**:
   - Create mount point: `mkdir -p /mnt/demo_volume`.
   - Format: `mkfs -t ext4 /dev/xvdf` (or xfs; ext4/xfs common).
   - Mount: `mount /dev/xvdf /mnt/demo_volume`.
   - Verify: `df -h` shows new increased space (e.g., root now has more GB available); `ls /mnt/demo_volume` (create test files).
   - Use: Create files in mounted location; apps can write to it.

**Interview Questions**:
- What is the process to add and mount a new disk in Linux?
  - Attach the disk, format with `mkfs -t ext4 <device>`, create mount point, and mount with `mount <device> <mount_point>`.

4. **Unmount**: `umount /mnt/demo_volume` (to detach).

## Example Scenario
- Original: 8GB disk, 7GB root, 1GB boot.
- Add 10GB: Format to ext4, mount to /mnt/demo_volume. Now 14GB+ free space.
- Mount to app directories (e.g., /var/logs) for log storage.

---