
# File Permissions in Linux

## Introduction to File Permissions in Linux
File permissions in Linux control access to files and directories, ensuring security and preventing unauthorized actions like accidental deletions or modifications. They complement **user management** by restricting what users, groups, and others can do (read, write, execute). Linux sets default permissions on all files/folders, but you can modify them using commands like `chmod` and `chown`.

**Why File Permissions?**
- **Multi-User Security**: On a shared Linux server, users (e.g., developers, QEs) shouldn't access or alter each other's files. Without permissions, anyone could corrupt the system (e.g., deleting `/sbin`).
- **Default Behavior**: Linux assigns basic permissions (e.g., owner can read/write, group/others can read). You can customize for organizational needs.
- **Real-World Example**: A developer creates a script in `/tmp`. By default, QEs can read it but not modify/delete. Admins can adjust (e.g., deny read access to QEs).

**Key Concepts**:
- **Permissions Types**: Read (r=4), Write (w=2), Execute (x=1).
- **Parties**: User (owner), Group, Others (ugo).
- **Representation**: 9-character string (e.g., `rw-r--r--`) or numeric (e.g., 644).
- **Inheritance**: Folder permissions take precedence over file permissions (like needing bank access to reach a locker).

This section builds on user management. Practice in a safe environment (e.g., Docker container).

---

## Understanding Permission Strings
Use `ls -l` to view permissions. Example output:
```bash
-rw-r--r-- 1 developer developer 0 Oct 10 12:00 hello.sh
drwxr-xr-x 2 root      root      4096 Oct 10 12:00 tmp/
```

- **First Character**: File type (`-` for file, `d` for directory, `l` for link).
- **Next 9 Characters**: Permissions for **ugo** (user, group, others), in sets of 3 (rwx).
  - **User (Owner)**: First 3 (e.g., `rw-` → read/write, no execute).
  - **Group**: Next 3 (e.g., `r--` → read only).
  - **Others**: Last 3 (e.g., `r--` → read only).
- **Missing Permissions**: Represented by `-` (e.g., no execute = `-`).
- **Numeric Equivalent**: Sum r/w/x values (e.g., `rw-` = 4+2+0=6).

**Example Breakdown**:
- `rw-r--r--` (644): Owner read/write, group/others read only.
- `rwxr-xr-x` (755): Owner full access, group/others read/execute.

**Default Permissions**:
- Files: 644 (owner rw, others r).
- Directories: 755 (owner rwx, others rx).
- Umask controls defaults (e.g., `umask 022` sets 644 for files).

---

## Modifying Permissions with `chmod`
`chmod` changes permissions. Use symbolic (letters) or numeric (numbers) mode. Requires ownership or root access.

### Symbolic Mode (Letters)
- **Syntax**: `chmod [ugoa][+-=][rwx] file` (comma-separated for multiple).
- **Options**:
  - `u` (user), `g` (group), `o` (others), `a` (all).
  - `+` (add), `-` (remove), `=` (set exactly).
- **Examples**:
  - `chmod u+x hello.sh` → Add execute for owner.
  - `chmod g-w,o-r hello.sh` → Remove write from group, read from others.
  - `chmod u=rwx,g=rw,o=r hello.sh` → Set specific permissions (equivalent to 764).
  - `chmod a+rwx hello.sh` → Full access for all (777).

### Numeric Mode (Numbers)
- **Syntax**: `chmod [0-7][0-7][0-7] file` (one number per ugo set).
- **Values**: r=4, w=2, x=1; sum for each set.
- **Common Examples**:
  - `chmod 644 file.txt` → Owner rw, others r (default for files).
  - `chmod 755 script.sh` → Owner rwx, others rx (executable script).
  - `chmod 777 file.txt` → Full access for all (open file).
  - `chmod 400 file.txt` → Owner read-only, others none.
  - `chmod 600 file.txt` → Owner rw, others none (private file).
  - `chmod 750 dir/` → Owner rwx, group rx, others none (secure directory).

**Demo Example**:
1. Create file as "developer": `touch hello.sh`.
2. Default: `rw-r--r--` (644).
3. Restrict others: `chmod o-r hello.sh` → `rw-r-----` (640).
4. Allow execute for owner: `chmod u+x hello.sh` → `rwxr-----` (740).
5. Numeric: `chmod 755 hello.sh` → `rwxr-xr-x`.

**Tips**:
- Recursive for directories: `chmod -R 755 dir/` (affects all contents).
- Check changes: `ls -l file`.
- Practice: Create files, change permissions, test access as different users.

---

## Changing Ownership with `chown`
`chown` transfers file/directory ownership. Requires root or ownership.

- **Syntax**: `chown [user][:group] file`.
- **Examples**:
  - `chown qe hello.sh` → Change owner to "qe".
  - `chown qe:devops hello.sh` → Change owner to "qe", group to "devops".
  - `chown :devops hello.sh` → Change group only.
- **Recursive**: `chown -R user dir/`.
- **Demo**: As `root`, `chown qe:test.sh` → Ownership changes from "developer" to "qe".

**Note**: Only `root` can change ownership. Use `chgrp` for group-only changes.

---

## Permissions on Directories vs. Files
- **Files**: Permissions control read/write/execute directly.
- **Directories**: Permissions affect navigation/access.
  - `r`: List contents.
  - `w`: Create/delete files inside.
  - `x`: Enter/access the directory.
- **Precedence**: Folder permissions override file permissions. To access a file, you must have execute on the parent directory (bank-locker analogy).
  - Example: File has 777, but parent dir has 000 → Can't access file.

**Demo**:
1. `chmod 700 /tmp` (owner only).
2. Create file inside: `touch /tmp/demo; chmod 777 /tmp/demo`.
3. As non-owner: Can't enter `/tmp`, so no access to `demo` despite 777.

---

## Additional Notes and Best Practices
- **Security Best Practices**:
  - Use least privilege (e.g., 644 for files, 755 for dirs).
  - Avoid 777 (world-writable).
  - Set umask: `umask 022` (default 644/755).
  - Audit with `find / -perm 777` (find overly permissive files).
- **Common Issues**:
  - "Permission denied": Check ugo permissions and parent dirs.
  - Sticky bit for shared dirs: `chmod +t dir/` (only owner can delete).
- **Interview Questions**:
  - What does `rw-r--r--` mean? Owner read/write, group/others read.
  - Difference between `chmod 755` and `chmod u=rwx,g=rx,o=rx`? Same (755).
  - Why can't I access a file with 777 permissions? Parent directory restrictions.
  - How to make a script executable? `chmod +x script.sh`.
- **Practice Exercises**:
  - Create users/groups, set permissions (e.g., 644, 755), test access.
  - Use `chmod` symbolic/numeric; change ownership.
  - Experiment with directories (e.g., restrict `/tmp`).

## Resources
- Official Docs: `man chmod`, `man chown`, `man ls`.
- GitHub Repo: Full examples in `README.md` (symbolic/numeric modes, demos).
- Tutorials: Linux permissions guides or `chmod` calculator tools.
- Next: Advanced topics like ACLs or SELinux.

---