
# Study Guide on User Management, File Management, and Vi Shortcuts

## Introduction
This study guide documents three key sections of the Linux course: **User Management** (Section 3), **File Management** (Section 4), and **Vi Shortcuts** (Section 5). These topics build on foundational Linux concepts and emphasize practical commands, security, and efficient system usage in a Linux environment.

The guide includes clear sections, explanations, key commands, examples, and interview-style questions for reinforcement. It assumes familiarity with basic Linux setup (e.g., using WSL, Docker, or cloud instances) as discussed in prior sections.

---

## Section 3: User Management

1. [Section 3: User Management](#section-3-user-management)
   - [Why User Management Matters](#why-user-management-matters)
   - [Understanding Users and the Root User](#understanding-users-and-the-root-user)
   - [Creating Users](#creating-users)
   - [Managing Passwords](#managing-passwords)
   - [Deleting Users](#deleting-users)
   - [Working with Groups](#working-with-groups)
   - [Switching Users and Demonstrating Permissions](#switching-users-and-demonstrating-permissions)
   - [Remote Access via SSH](#remote-access-via-ssh)
   - [Advanced User Management Commands](#advanced-user-management-commands)
   - [Common Pitfalls and Best Practices](#common-pitfalls-and-best-practices)
   - [Summary and Exercises](#summary-and-exercises-section-3)

### Why User Management Matters
In personal computing, a single user account often suffices because accountability is straightforward—you know your own actions. However, in enterprise environments like production Linux servers, where applications run 24/7 and multiple stakeholders (developers, DevOps engineers, QA testers) access the system, user management becomes critical. Without it, sharing a single root account leads to chaos: no way to track who deleted a critical file (e.g., /sbin), resulting in security breaches, data loss, and unaccountable actions.

User management enables:
- **Accountability**: Each action is tied to a user, facilitating audits and troubleshooting.
- **Security**: Granular permissions prevent unauthorized access (e.g., developers can't modify system binaries).
- **Scalability**: Efficiently manage hundreds or thousands of users via groups.

This section explores creating users, passwords, groups, and remote access, drawing from real-world server scenarios.

### Understanding Users and the Root User
- **Users**: Represent individuals or processes with unique identities. Each user has a UID (User ID), home directory, and permissions.
- **Root User**: The superuser (UID 0) with unrestricted access. Avoid using root for daily tasks to minimize risks—use it only for administration.
- **Key Files**:
  - `/etc/passwd`: Stores user details (username, UID, GID, home directory, shell).
    - Format: `username:x:UID:GID:full_name:home_dir:shell`
    - Example: `johndoe:x:1001:1001:John Doe:/home/johndoe:/bin/bash`
  - `/etc/shadow`: Contains encrypted passwords (one-way hashed; cannot decrypt).
  - `/etc/group`: Lists groups and their members.

### Creating Users
Creating users assigns unique identities. Use `useradd` for scripts or `adduser` for interactive setups.

#### Basic User Creation with `useradd`
- Command: `useradd <username>`
- Example: `useradd johndoe`
- What it does: Creates a user without a home directory or prompts for details.
- Verification: `cat /etc/passwd | grep johndoe`
- Note: Run as root or with `sudo`.

#### Interactive User Creation with `adduser`
- Command: `adduser <username>`
- Example:
  ```
  adduser johndoe
  # Prompts: Full name? John Doe
  # Room number? (optional, press Enter)
  # Work phone? (optional)
  # Home phone? (optional)
  # Other? (optional)
  # Is the information correct? y
  # Enter password: [type password]
  # Retype password: [confirm]
  ```
- What it does: Creates user with home directory (/home/johndoe), sets up default files (e.g., .bashrc), and prompts for details.
- Verification: `ls /home` (check for johndoe directory); `cat /etc/passwd`.

**Tip**: Use `adduser` for manual setups; `useradd` for automation (e.g., in shell scripts).

### Managing Passwords
Passwords secure user accounts. They are hashed and stored in `/etc/shadow`.

- Command: `passwd <username>`
- Example:
  ```
  passwd johndoe
  # Enter new UNIX password: [type password]
  # Retype new UNIX password: [confirm]
  ```
- Verification: `cat /etc/shadow | grep johndoe` (shows encrypted hash).
- Key Insight: Passwords use SHA-256 hashing; decryption is impossible. If forgotten, reset via `passwd` (as admin).

**Security Note**: Enforce strong passwords and regular changes (see Advanced Commands).

### Deleting Users
Remove users when they leave or are no longer needed.

- Command: `userdel <username>`
- Example: `userdel johndoe`
- Verification: `cat /etc/passwd | grep johndoe` (should be empty).
- Options: `userdel -r <username>` (removes home directory and mail spool).

**Caution**: Deleting a user doesn't remove their files elsewhere; clean up manually.

### Working with Groups
Groups simplify permission management for multiple users.

- **Why Groups?**: Instead of setting permissions per user, assign to groups (e.g., all developers in "dev" group).
- Creating Groups: `groupadd <groupname>` (e.g., `groupadd devops`).
- Adding Users to Groups: `usermod -aG <groupname> <username>` (e.g., `usermod -aG devops johndoe`).
- Verification: `cat /etc/group | grep devops` (shows group ID and members).

**Example Workflow**:
1. Create group: `groupadd dev`
2. Add users: `usermod -aG dev developer1; usermod -aG dev developer2`
3. Modify group permissions (covered in future episodes).

### Switching Users and Demonstrating Permissions
- Switch User: `su - <username>` (e.g., `su - johndoe`).
- Check Current User: `whoami` or `id`.
- Demo: As johndoe, try `rm -rf /sbin` → Permission denied (root only).

This illustrates user isolation for security.

### Remote Access via SSH
SSH enables secure remote login to Linux servers.

- **Setup**: Servers run `sshd`; clients use SSH tools (e.g., Git Bash, Terminal).
- Connect: `ssh <username>@<IP>` (e.g., `ssh johndoe@192.168.1.100`).
- Password Auth: Enabled by default; disable in `/etc/ssh/sshd_config` (`PasswordAuthentication no`) for key-based auth.
- Restart SSH: `sudo systemctl restart ssh`.

**Tip**: Use key pairs for production (covered in future episodes).

### Advanced User Management Commands
- Force Password Change: `chage -m 90 <username>` (every 90 days).
- Lock/Unlock Account: `usermod -L <username>` (lock); `usermod -U <username>` (unlock).
- View User Info: `id <username>` (shows UID, GID, groups).

### Common Pitfalls and Best Practices
- **Pitfalls**: Forgetting to set passwords; using root for everything; not verifying creations.
- **Best Practices**: Use unique usernames; enforce password policies; document user roles; test in non-production environments.

### Summary and Exercises
User management ensures secure, accountable Linux systems. Key commands: `adduser`, `passwd`, `groupadd`, `usermod`, `ssh`.

**Exercises**:
1. Create a user "testuser" with `adduser`, set a password, and verify in `/etc/passwd`.
2. Add "testuser" to a new group "testers" and confirm.
3. Switch to "testuser" and attempt to list `/root` (should fail).
4. Delete "testuser" and clean up.

---