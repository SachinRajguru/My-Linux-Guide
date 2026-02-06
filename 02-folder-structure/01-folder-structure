
# Understanding the Folder Structure

The Linux filesystem follows the **Filesystem Hierarchy Standard (FHS)**, which defines the purpose of directories for consistency across distros. In containers (like Docker), some directories are minimal or absent, but the structure remains similar. Below is a categorized overview of key directories, with tables for clarity.

### Introduction to FHS
- Directories are organized hierarchically, starting from `/` (root).
- In containers, focus on `/usr`, `/var`, `/etc`, and user dirs—system dirs like `/boot` are often irrelevant.
- Use commands like `ls /` to explore or `man hier` for FHS details.

### Symbolic Links (Less Significant)
These are transitional links in modern Linux for compatibility.

| Directory | Description |
|-----------|-------------|
| `/sbin -> /usr/sbin` | System binaries for administrative commands (linked to `/usr/sbin`). |
| `/bin -> /usr/bin` | Essential user binaries (linked to `/usr/bin`). |
| `/lib -> /usr/lib` | Shared libraries and kernel modules (linked to `/usr/lib`). |

### Important System Directories
Core dirs for system operation.

| Directory | Description |
|-----------|-------------|
| `/boot` | Stores files needed for booting the system (e.g., kernel images; not relevant in containers). |
| `/usr` | Contains most user-installed applications and libraries (e.g., `/usr/bin` for binaries). |
| `/var` | Stores logs, caches, and temporary files that change frequently (e.g., `/var/log` for system logs). |
| `/etc` | Stores system configuration files (e.g., `/etc/passwd` for users). |

### User & Application-Specific Directories
Dirs for users and apps.

| Directory | Description |
|-----------|-------------|
| `/home` | Default location for user home directories (e.g., `/home/user` for personal files). |
| `/opt` | Used for installing optional third-party software (e.g., proprietary apps). |
| `/srv` | Holds data for services like web servers (rarely used in containers). |
| `/root` | Home directory for the root user (equivalent to `/home/root`). |

### Temporary & Volatile Directories
Dirs for dynamic or virtual data.

| Directory | Description |
|-----------|-------------|
| `/tmp` | Temporary files (cleared on reboot; e.g., session data). |
| `/run` | Holds runtime data for processes (e.g., PID files; survives reboots in some cases). |
| `/proc` | Virtual filesystem for process and system information (e.g., `/proc/cpuinfo` for hardware details). |
| `/sys` | Virtual filesystem for hardware and kernel information (e.g., `/sys/devices` for devices). |
| `/dev` | Contains device files (e.g., `/dev/null` for discarding output, `/dev/sda` for disks). |
| `/lost+found` | Directory for recovered files after filesystem checks (e.g., via `fsck`). |

### Mount Points
Dirs for attaching external filesystems.

| Directory | Description |
|-----------|-------------|
| `/mnt` | Temporary mount point for external filesystems (e.g., manual NFS mounts). |
| `/media` | Mount point for removable media (e.g., USB drives auto-mount here). |
| `/data` | Likely your **mounted volume** from the host (e.g., `C:/ubuntu-data` in Docker for persistence). |

### Navigation Tips
- **Explore Directories**: Use `ls -la /` to list root contents or `tree /` (install with `apt install tree`) for a visual tree.
- **Check Usage**: Run `df -h` to see mounted filesystems or `du -sh /var` for directory sizes.
- **In Containers**: Your `/data` mount persists data—store configs or logs there to survive restarts.

### Further Reading
- [Filesystem Hierarchy Standard](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html) – Official spec.
- [Linux Directory Structure](https://tldp.org/LDP/Linux-Filesystem-Hierarchy/html/) – Detailed guide.

---