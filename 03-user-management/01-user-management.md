
# User Management in Linux

## Introduction to User Management in Linux
Linux is a multi-user operating system, allowing multiple users to operate simultaneously while ensuring security, controlled access, and system integrity. User management involves creating, modifying, and deleting user accounts, managing passwords, and organizing users into groups. This helps prevent unauthorized access and maintains accountability (e.g., tracking who performed actions like file deletions).

### Key Files Involved
- `/etc/passwd`: Stores user account details (e.g., username, UID, GID, home directory, shell).
- `/etc/shadow`: Stores hashed (not encrypted) user passwords and password policies. Passwords are one-way hashed (e.g., with SHA-256), making them irreversible—**correction**: The original content says "encrypted," but it's hashing for security.
- `/etc/group`: Stores group information (e.g., group names, GIDs, members).
- `/etc/gshadow`: Stores secure group details, including hashed group passwords (rarely used).

**Note**: Always back up these files before editing. Use `sudo` for root-level changes.

---

## Creating Users in Linux
Creating users assigns unique identities for access control. Use `useradd` for basic creation or `adduser` for interactive setups.

### `useradd` Command (Available on Most Linux Distributions)
This is a low-level command for quick user creation. It doesn't create a home directory by default.

```bash
useradd username
```
- Creates a user without a home directory or password prompt.
- Example: `useradd john` → Adds user "john" with default settings.

To create a user with a home directory:
```bash
useradd -m username
```
- `-m`: Creates `/home/username`.

To specify a default shell (e.g., Bash):
```bash
useradd -s /bin/bash username
```
- `-s`: Sets the login shell.

**Tip**: Check creation with `cat /etc/passwd | grep username`. For scripts, `useradd` is preferred (non-interactive).

### `adduser` Command (Interactive, Common on Debian-Based Systems like Ubuntu)
This is a higher-level, user-friendly command that prompts for details and creates a home directory.

```bash
adduser username
```
- Prompts for password, full name, room number, etc.
- Automatically creates `/home/username` and sets up basic files.

**Difference from `useradd`**:
- `useradd`: Non-interactive, minimal setup—ideal for automation/scripts.
- `adduser`: Interactive, comprehensive—better for manual creation.
- **Interview Question**: Use `useradd` in shell scripts to avoid prompts.

**Example Workflow**:
1. `adduser alice`
2. Enter password and details.
3. Verify: `ls /home` (should show "alice").

---

## Managing User Passwords
Passwords secure user accounts. They are stored hashed in `/etc/shadow`.

### Setting or Changing a User’s Password
```bash
passwd username
```
- Prompts for a new password (must be strong).
- As root, you can change any user's password.
- Example: `passwd bob` → Sets password for "bob".

**Security Note**: Passwords are hashed (one-way). If forgotten, reset it—you can't "decrypt" or restore the old one.

### Enforcing Password Policies
Use `chage` or `passwd` for policies to enforce security (e.g., expiration to prevent stale passwords).

- **Set password expiration (e.g., every 90 days)**:
  ```bash
  chage -M 90 username
  ```
  - Forces password change after 90 days.
  - View policy: `chage -l username`.

- **Lock a user account** (prevents login):
  ```bash
  passwd -l username
  ```
  - Adds "!" to the password hash in `/etc/shadow`.

- **Unlock a user account**:
  ```bash
  passwd -u username
  ```
  - Removes the lock.

**Tip**: For organizational compliance, integrate with tools like LDAP or SSO for centralized password management.

---

## Modifying Users
Modify existing users with `usermod` to update details without recreating accounts.

- **Change the username**:
  ```bash
  usermod -l new_username old_username
  ```
  - Example: `usermod -l jane john` → Renames "john" to "jane".

- **Change the home directory** (and move files):
  ```bash
  usermod -d /new/home/directory -m username
  ```
  - `-d`: New home path; `-m`: Moves existing files.
  - Example: `usermod -d /home/newdir -m alice`.

- **Change the default shell**:
  ```bash
  usermod -s /bin/zsh username
  ```
  - Example: `usermod -s /bin/zsh bob` → Sets Zsh as default.

**Note**: After changes, the user may need to log out/in for effects to apply.

---

## Deleting Users
Remove users when they leave or are no longer needed. Be cautious—deletion is permanent.

- **Remove a user but keep their home directory**:
  ```bash
  userdel username
  ```
  - Example: `userdel john` → Deletes user but leaves `/home/john`.

- **Remove a user and their home directory**:
  ```bash
  userdel -r username
  ```
  - `-r`: Recursively deletes home directory and mail spool.
  - Example: `userdel -r alice` → Full cleanup.

**Tip**: Check for running processes: `ps aux | grep username` before deleting. Use `pkill -u username` to stop them.

---

## Working with Groups
Groups organize users for shared permissions (e.g., grant access to a directory for all developers).

### Creating Groups
```bash
groupadd groupname
```
- Example: `groupadd developers` → Creates "developers" group.
- Check: `cat /etc/group | grep developers`.

### Adding Users to Groups
```bash
usermod -aG groupname username
```
- `-aG`: Appends to group (preserves existing groups).
- Example: `usermod -aG developers bob` → Adds "bob" to "developers".
- Verify: `groups bob` → Shows memberships.

### Viewing Group Memberships
```bash
groups username
```
- Example: `groups alice` → Lists groups (e.g., "alice : alice developers").

### Changing Primary Group
The primary group is the default for new files.
```bash
usermod -g new_primary_group username
```
- Example: `usermod -g developers alice` → Sets "developers" as primary.

**Tip**: Groups simplify permissions—change access for the group instead of individual users.

---

## Sudo Access and Privilege Escalation
Sudo allows users to run commands as root without full root access, enhancing security.

### Adding a User to the Sudo Group
- **On Debian-based systems (e.g., Ubuntu)**:
  ```bash
  usermod -aG sudo username
  ```
  - Grants full sudo access.
- **On RHEL-based systems (e.g., CentOS)**:
  ```bash
  usermod -aG wheel username
  ```
  - "wheel" is the sudo group equivalent.

**Example**: `usermod -aG sudo bob` → "bob" can now use `sudo`.

### Granting Specific Commands with Sudo
For limited access, edit the sudoers file (use `visudo` to avoid syntax errors).
```bash
visudo
```
- Add a line like:
  ```bash
  username ALL=(ALL) NOPASSWD: /path/to/command
  ```
  - Example: `alice ALL=(ALL) NOPASSWD: /usr/bin/apt update` → Allows "alice" to run `apt update` without a password.
- **Syntax**: `user host=(run_as) command`.

**Security Tip**: Avoid `NOPASSWD` unless necessary. Test with `sudo -l` to verify permissions.

---

## Additional Notes and Best Practices
- **Accountability**: Use unique usernames (e.g., first.last) to track actions in logs.
- **Switching Users**: `su - username` (full login) or `sudo -u username command` (run single command).
- **Demo Permissions**: As a non-root user, try `rm -rf /sbin` → Denied (proves restrictions).
- **Common Issues**: If password auth is disabled (e.g., on clouds), use SSH keys.
- **Interview Questions**:
  - Difference between `useradd` and `adduser`? Scripts vs. interactive.
  - Can you restore a password? No (hashed).
  - Why groups? Bulk permission management.
- **Practice**: In a Docker container, create users/groups, set passwords, and test sudo.

## Resources
- Official Docs: `man useradd`, `man passwd`, `man sudo`.
- GitHub Repo: Refer to `README.md` for full examples and scripts.
- Tools: Use `getent passwd username` for user info; `id username` for UID/GID.

If you encounter errors or need expansions, check logs in `/var/log/auth.log`. Happy managing!

---