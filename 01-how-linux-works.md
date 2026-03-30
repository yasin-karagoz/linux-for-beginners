# How Linux Works

## What is Linux?

Linux is an open-source operating system kernel created by Linus Torvalds in 1991. The full operating system (GNU/Linux) combines the Linux kernel with GNU tools and other software to form a complete OS.

## Linux Architecture

```
+---------------------------+
|     User Applications     |  (browsers, editors, scripts)
+---------------------------+
|        Shell (CLI)        |  (bash, zsh, fish)
+---------------------------+
|     System Libraries      |  (glibc, libc)
+---------------------------+
|      System Calls         |  (interface to kernel)
+---------------------------+
|          Kernel           |  (core of Linux)
|  - Process Management     |
|  - Memory Management      |
|  - File System            |
|  - Device Drivers         |
|  - Networking             |
+---------------------------+
|         Hardware          |  (CPU, RAM, Disk, Network)
+---------------------------+
```

## Key Concepts

### The Kernel
The kernel is the core of Linux. It manages:
- **Processes**: Creating, scheduling, and terminating programs
- **Memory**: Allocating and protecting RAM for processes
- **File Systems**: Reading/writing data on disks (ext4, xfs, btrfs, etc.)
- **Devices**: Communicating with hardware via drivers
- **Networking**: Managing network interfaces and protocols

### Processes
Every running program is a process. Each process has:
- A unique **PID** (Process ID)
- A **parent process** (PPID)
- Its own memory space
- One or more **threads**

The first process started by the kernel is **init** (or systemd on modern systems) with PID 1. All other processes are descendants of it.

### The File System Hierarchy
Linux uses a single root tree starting at `/`:

| Directory | Purpose |
|-----------|---------|
| `/`       | Root of the entire filesystem |
| `/bin`    | Essential user binaries (ls, cp, mv) |
| `/sbin`   | System binaries (for root/admin) |
| `/etc`    | Configuration files |
| `/home`   | User home directories |
| `/root`   | Home directory for root user |
| `/tmp`    | Temporary files (cleared on reboot) |
| `/var`    | Variable data (logs, databases) |
| `/usr`    | User programs and libraries |
| `/lib`    | Shared libraries |
| `/dev`    | Device files |
| `/proc`   | Virtual filesystem for process info |
| `/sys`    | Virtual filesystem for system/hardware info |
| `/mnt`    | Mount point for temporary mounts |
| `/media`  | Mount point for removable media |
| `/opt`    | Optional/third-party software |

### Everything is a File
In Linux, almost everything is represented as a file:
- Regular files (text, binaries)
- Directories
- Devices (`/dev/sda` = hard disk, `/dev/null` = black hole)
- Sockets and pipes (inter-process communication)
- Symbolic links

### Users and Permissions
Linux is a multi-user system:
- Every file has an **owner** (user) and a **group**
- Permissions are set for: **owner**, **group**, **others**
- Each can have: **read (r)**, **write (w)**, **execute (x)**
- The **root** user (UID 0) has unrestricted access

### Shell
The shell is a command-line interpreter. Common shells:
- **bash** (Bourne Again Shell) — most common default
- **zsh** (Z Shell) — popular with advanced users
- **sh** (POSIX shell) — minimal, portable
- **fish** — user-friendly, modern

### Package Management
Linux distros use package managers to install software:

| Distro Family      | Package Manager | Command          |
|--------------------|-----------------|------------------|
| Debian/Ubuntu      | apt             | `apt install`    |
| Fedora/RHEL/CentOS | dnf/yum         | `dnf install`    |
| Arch Linux         | pacman          | `pacman -S`      |
| openSUSE           | zypper          | `zypper install` |

### Systemd and Init
Modern Linux systems use **systemd** as the init system:
- Starts and manages services (daemons)
- Boots the system in parallel for speed
- Manages logs via `journald`
- Controls units: services, sockets, timers, mounts

---
Next: [Common Linux Commands](02-common-commands.md)
