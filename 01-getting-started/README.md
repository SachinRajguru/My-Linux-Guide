
# Section 1: Getting Started

This section covers OS basics, Linux history/structure/distributions, setup, and package managers.

## Why Operating Systems?
- **Hardware Components**: CPU, memory, storage, motherboard, network interfaces.
- **Actors Needing Hardware Access**: Users (e.g., creating files, checking CPU status) and software applications (e.g., YouTube needing CPU/memory/display).
- **Problem**: Neither can directly interact with hardware; apps lack low-level code, users need interfaces (GUI/CLI).
- **Solution**: Operating System (OS) as an intermediary software layer that manages hardware resources (process, memory, device, network management).

## What is an Operating System?
- Acts as a bridge between users/software and hardware.
- Provides CLI/GUI for user interaction (e.g., Windows has rich GUI; Linux has rich CLI).
- Key OS Types: Windows, Linux, Mac OS.

Operating systems act as an intermediary layer between users/software and hardware (CPU, memory, storage, etc.). They handle process management, memory allocation, device/network management, and provide CLI/GUI interfaces for interaction. Without them, direct hardware access is impossible for users or apps.

## History of Operating Systems
- **1960s**: Unix – First OS, simplified hardware interaction for developers.
- **1970s**: Minix – Built on Unix, partially open-source.
- **1980s**: Windows – Proprietary, introduced rich GUI, made user interaction easy.
- **1990s**: Linux – Open-source kernel by Linus Torvalds, competitive with Windows. Became popular due to being free, secure, and community-backed.
- **Today**: Linux dominates (90% of production workloads) for its open-source nature, security, and cost-effectiveness.

## Linux Structure
- **Core Component**: Linux Kernel – Handles hardware interactions (process/memory/disk/network management).
- **Layers on Top**:
  - System libraries and utilities (commands, basic packages).
  - Shell/CLI (mandatory for user interaction); optional GUI.

Linux can run minimally with just the kernel, but full OS includes utilities/libraries/shell for usability.
Kernel is essential; other components enhance usability and security.

## Linux Distributions
- Linux is open-source, so anyone can modify and redistribute it.
- **Examples**: Ubuntu (Canonical), Red Hat, Fedora, Debian, Alpine – All based on core Linux but with added features/wrappers.
- **Compatibility**: Scripts/apps often work across distributions due to shared core libraries.
- **Recommendation**: Ubuntu for beginners – easy, comes with pre-installed packages/libraries.

Distributions (e.g., Ubuntu, Red Hat, Fedora, Debian, Alpine) are customized versions of open-source Linux, with added features/wrappers. They share common libraries, so scripts often work across them (unlike Windows). Ubuntu is recommended for beginners due to its ease and pre-installed packages.

## Setting Up Linux
- **No Need for Cloud Instances**: Avoid costs; set up locally for free.
- **Option 1: WSL (Windows Subsystem for Linux)**   
  - Run `wsl --install` in PowerShell/Command Prompt.
  - Creates a Linux-like environment (not a VM); restart and run `wsl` to access Ubuntu.
- **Option 2: Docker Container (for Windows/Mac)**
  - Use the provided Docker command (from GitHub repo) to run an Ubuntu container.
  - Ensure a local folder (e.g., `C:\Ubuntu-data` on Windows or `/tmp/Ubuntu-data` on Mac) exists for storage binding.
  - Run the command, then `docker exec -it <container_id> /bin/bash` to enter the Linux environment.
  - Simulates a full Linux machine for learning.
- **Fallback**: Cloud instances (e.g., AWS EC2) if above fail (rare).

## Package Managers
- Tools to install, update, remove, and manage software dependencies (e.g., Python, Java).
- **For Ubuntu**: `apt` (pre-installed).
  - `apt list`: Lists installed packages.
  - `apt search <package>`: Searches for packages.
  - `apt update`: Updates package list (run first after setup).
  - `apt install <package>`: Downloads and installs from trusted repos (e.g., `apt install python3`).
- Downloads from centralized locations (e.g., archive.ubuntu.com).
- For unavailable packages, use commands like `curl` or `wget` (covered later).

### Recap of Section 1
- OS fundamentals and history.
- Linux kernel, structure, and distributions (Ubuntu recommended).
- Free setup via WSL or Docker.
- Package managers (e.g., apt for Ubuntu).

---