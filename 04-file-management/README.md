
# Section 4: File Management

2. [Section 4: File Management](#section-4-file-management)
   - [Introduction to File Systems](#introduction-to-file-systems)
   - [Listing Files and Directories](#listing-files-and-directories)
   - [Navigating the File System](#navigating-the-file-system)
   - [Creating and Deleting Directories](#creating-and-deleting-directories)
   - [Creating, Copying, and Renaming Files](#creating-copying-and-renaming-files)
   - [Reading File Contents](#reading-file-contents)
   - [Writing to Files](#writing-to-files)
   - [Common Pitfalls and Best Practices](#common-pitfalls-and-best-practices-section-4)
   - [Summary and Exercises](#summary-and-exercises-section-4)

## Introduction to File Systems
Linux organizes files in a hierarchical tree starting from `/` (root). Directories are folders; files store data. File management commands manipulate this structure efficiently, crucial for navigation, organization, and maintenance.

## Listing Files and Directories
- Command: `ls` (list).
- Options: `ls -l` (long format: permissions, size, date); `ls -a` (show hidden files, e.g., .bashrc).
- Example: `ls /home` → Lists user directories.

## Navigating the File System
- Change Directory: `cd <path>` (e.g., `cd /tmp`).
- Go Up: `cd ..`.
- Current Directory: `pwd` (Print Working Directory).
- Absolute Paths: Start with `/` (e.g., `/home/user`); Relative: From current (e.g., `../documents`).

## Creating and Deleting Directories
- Create: `mkdir <dirname>` (e.g., `mkdir projects`).
- Delete Empty: `rmdir <dirname>`.
- Delete with Contents: `rm -rf <dirname>` (force recursive; dangerous—use cautiously).

## Creating, Copying, and Renaming Files
- Create Empty File: `touch <filename>` (e.g., `touch notes.txt`).
- Copy: `cp <source> <dest>` (e.g., `cp notes.txt backup.txt`).
- Rename/Move: `mv <source> <dest>` (e.g., `mv notes.txt mynotes.txt`).

## Reading File Contents
- Print All: `cat <filename>` (for small files).
- Interactive View: `less <filename>` (scroll with arrows; 'q' to quit).
- First/Last Lines: `head -n 10 <filename>`; `tail -n 20 <filename>`.
- Reverse Print: `tac <filename>` (rarely used).

## Writing to Files
- Overwrite: `echo "text" > <filename>`.
- Append: `echo "text" >> <filename>`.
- Example: `echo "Hello World" > hello.txt` (overwrites); `echo "More text" >> hello.txt` (appends).

## Common Pitfalls and Best Practices
- **Pitfalls**: Using `rm -rf /` (deletes everything); forgetting paths.
- **Best Practices**: Use `ls` before deleting; back up files; prefer `less` for large files.

## Summary and Exercises
File management is about organizing and accessing data. Key commands: `ls`, `cd`, `mkdir`, `rm`, `cp`, `mv`, `cat`, `less`.

**Exercises**:
1. Navigate to `/tmp`, create a directory "testdir", and a file "testfile.txt" inside it.
2. Copy "testfile.txt" to "copy.txt" and rename to "renamed.txt".
3. Write "Sample text" to "renamed.txt" and view with `cat`.
4. Delete "testdir" and its contents.

---