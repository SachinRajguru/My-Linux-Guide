
# Linux - Folder Structures

## Overview
- **Focus**: Default folder structure in the Linux root filesystem (/). Essential for troubleshooting, admin tasks, and understanding organization.
- **Key Concept**: Folders segregate system binaries, configs, user data, etc., for security and efficiency.
- **Resources**: GitHub repo (my-linux-guide) for detailed notes, categorized in readme.md.

## Linux Environment Setup Recap
- **Options**: Docker (via GitHub's setup.md command), WSL, or EC2 instance.
- **Terminal Prompt**:
  - Docker: `root@ubuntu:/#` (user@hostname:current_dir#).
  - EC2: `ubuntu@host:~$` (tilde ~ = /home/ubuntu).
- **Users**:
  - **Root**: Admin user (full access; default in Docker).
  - **Ubuntu**: Non-admin default in EC2 (security measure).
- **Current Directory**: Starts at / (root) in Docker or ~/home/ubuntu in EC2.
- **Commands**: `ls -ltr` to list folders/files; `cd /` to go to root.

## The Root File System (/)
- Base directory (akin to filesystem root or Google Drive root).
- All subfolders/files originate here.
- Purpose: Organize system components, binaries, configs, data, etc.
- **Note**: In containers, some dirs (e.g., /boot) are minimal; structures follow the Filesystem Hierarchy Standard (FHS).

## Key Folders and Purposes
(Categorized for easy revision; see summary table below.)

### System Binaries and Libraries
- **/sbin**: System binaries for admin tasks (e.g., `useradd`, `groupadd`, `userdel`). Restricted to admins.
- **/bin**: User binaries for non-admin commands (e.g., `date`, `diff`, `sed`). Accessible to all users.
- **/usr**: Parent directory for /usr/bin, /usr/sbin, /usr/lib (full paths). /sbin, /bin, /lib are symlinks (shortcuts) to these.
- **/lib**: Libraries for kernel system calls (not user-accessible). Example: Check with `ls /lib`.

### System and Boot Directories
- **/boot**: Files for booting/restarting (e.g., kernel images). More populated on VMs (EC2) vs. containers.
- **/etc**: System configs (like Windows' C:\Windows\System32). Examples:
  - `passwd`: User accounts/passwords.
  - `hosts`: Local DNS caching.
  - `os-release`: OS details (e.g., `cat /etc/os-release`).
  - RC folders: Startup scripts/executables.

### Application and User-Specific Directories
- **/opt**: Third-party software (e.g., custom Java). Shared location; avoid user homes for privacy.
- **/srv**: Server files (e.g., web server configs like Apache).
- **/home**: User home directories (e.g., /home/ubuntu, /home/sachin). Private files; auto-created with `adduser`.
- **/root**: Home for root user (exception: not under /home).

### Mount and Data Directories
- **/mnt**: Mounting additional volumes/disks (e.g., adding storage).
- **/media**: Removable media (e.g., USB); less relevant for DevOps.
- **/data**: Shared organizational data (e.g., billing info). Often a custom mount in containers.

### Temporary/Volatile Directories
- **/tmp**: Temporary files (auto-cleaned periodically).
- **/var**: Logs, cache, third-party libs (e.g., /var/log for system/app logs; check with `cat /var/log/syslog`).
- **/run**: Runtime data for processes.
- **/proc, /dev, /sys**: Virtual filesystems for processes, devices, system info (used in scripts; e.g., `cat /proc/cpuinfo`).
- **/lost+found**: Recovered files after filesystem checks (e.g., via `fsck`).

### Summary Table of Key Folders
| Category | Directory | Purpose | Example Command |
|----------|-----------|---------|-----------------|
| Binaries | /bin, /sbin, /usr | Executables (user/admin) | `which ls` |
| Configs | /etc | System settings | `cat /etc/passwd` |
| Users | /home, /root | Personal data | `ls /home` |
| Apps | /opt, /srv | Third-party/server files | `ls /opt` |
| Mounts | /mnt, /media, /data | External storage | `df -h` |
| Temp/Volatile | /tmp, /var, /run, /proc | Dynamic data | `ls /var/log` |

## How Commands Work: PATH Variable
- Commands (e.g., `ls`) execute without full paths because they're in PATH.
- PATH includes: /usr/local/sbin, /usr/local/bin, /usr/sbin, /usr/bin, /sbin, /bin.
- Check with `echo $PATH`; `which ls` shows full path (/usr/bin/ls).
- If not in PATH, use full path or add to PATH (covered later).

## Navigation Tips
- **Explore**: Use `tree /` (install with `apt install tree`) for a visual directory tree or `du -sh /var` for sizes.
- **Practice**: In Docker/EC2, run `cd / && ls -la` to inspect root.

## Additional Notes
- **Symlinks**: Created with `ln` (like Windows shortcuts) for convenience.
- **Security**: Admin binaries (/sbin) restricted; user binaries (/bin) open to prevent corruption.
- **Future Relevance**: Used in upcoming sections (e.g., user management, permissions, monitoring).
- **Practice**: Use Docker/EC2; refer to GitHub for categorized summaries.

## Recap
- Covered Linux filesystem from / (root).
- Key folders: /sbin (admin binaries), /bin (user binaries), /etc (configs), /home (users), /opt (third-party), /var (logs), /tmp (temp).
- Emphasized organization/security; PATH for command execution.
- Next: user management.

### Further Reading
- [Filesystem Hierarchy Standard](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html) – Official spec.
- Your GitHub repo (my-linux-guide) for notes.

---