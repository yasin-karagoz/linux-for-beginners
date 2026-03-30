# Common Linux Commands

## Navigation

| Command | Description | Example |
|---------|-------------|---------|
| `pwd` | Print working directory | `pwd` → `/home/user` |
| `ls` | List directory contents | `ls -la` |
| `cd` | Change directory | `cd /etc` |
| `cd ..` | Go up one directory | `cd ..` |
| `cd ~` | Go to home directory | `cd ~` |
| `cd -` | Go to previous directory | `cd -` |

### `ls` Options
```bash
ls          # basic listing
ls -l       # long format (permissions, owner, size, date)
ls -a       # show hidden files (starting with .)
ls -la      # long format + hidden files
ls -lh      # long format with human-readable sizes (KB, MB)
ls -lt      # sort by modification time (newest first)
ls -R       # recursive listing
```

---

## File and Directory Operations

| Command | Description | Example |
|---------|-------------|---------|
| `mkdir` | Make directory | `mkdir mydir` |
| `mkdir -p` | Make nested directories | `mkdir -p a/b/c` |
| `touch` | Create empty file / update timestamp | `touch file.txt` |
| `cp` | Copy files | `cp src.txt dst.txt` |
| `cp -r` | Copy directory recursively | `cp -r dir1 dir2` |
| `mv` | Move or rename | `mv old.txt new.txt` |
| `rm` | Remove file | `rm file.txt` |
| `rm -r` | Remove directory recursively | `rm -r mydir` |
| `rm -rf` | Force remove (no prompts) | `rm -rf mydir` ⚠️ |
| `rmdir` | Remove empty directory | `rmdir emptydir` |

> ⚠️ `rm -rf` is permanent — there is no Recycle Bin in the terminal.

---

## Viewing File Contents

| Command | Description | Example |
|---------|-------------|---------|
| `cat` | Print entire file | `cat file.txt` |
| `less` | Page through file (q to quit) | `less file.txt` |
| `more` | Page through file (older) | `more file.txt` |
| `head` | Show first N lines (default 10) | `head -n 20 file.txt` |
| `tail` | Show last N lines (default 10) | `tail -n 20 file.txt` |
| `tail -f` | Follow file in real time | `tail -f /var/log/syslog` |

---

## Searching

| Command | Description | Example |
|---------|-------------|---------|
| `grep` | Search text in files | `grep "error" log.txt` |
| `grep -r` | Search recursively | `grep -r "TODO" ./src` |
| `grep -i` | Case-insensitive | `grep -i "error" log.txt` |
| `grep -n` | Show line numbers | `grep -n "error" log.txt` |
| `find` | Find files by name/type | `find /home -name "*.txt"` |
| `locate` | Fast filename search (uses index) | `locate myfile.conf` |
| `which` | Find location of a command | `which python3` |
| `whereis` | Find binary, source, man page | `whereis bash` |

### `find` Examples
```bash
find . -name "*.log"             # find all .log files here
find /etc -name "*.conf"         # find config files in /etc
find /tmp -mtime +7              # files older than 7 days
find . -type d                   # find directories only
find . -type f -size +10M        # files larger than 10MB
find . -name "*.sh" -executable  # executable shell scripts
```

---

## Permissions

### Understanding Permission Strings
```
-rwxr-xr--  1  user  group  4096  Jan 1 12:00  file.txt
│└──┴──┴──      │     │
│ u   g   o     │     └─ Group owner
│               └─ User owner
└─ File type (- = file, d = directory, l = symlink)

u = user/owner permissions (rwx)
g = group permissions      (r-x)
o = others permissions     (r--)
```

| Command | Description | Example |
|---------|-------------|---------|
| `chmod` | Change file permissions | `chmod 755 script.sh` |
| `chown` | Change file owner | `chown user:group file` |
| `chgrp` | Change group | `chgrp developers file` |
| `umask` | Default permission mask | `umask 022` |

### Numeric Permissions (Octal)
```
4 = read (r)
2 = write (w)
1 = execute (x)

755 = rwx r-x r-x  (owner: all, group: read+exec, others: read+exec)
644 = rw- r-- r--  (owner: read+write, others: read only)
700 = rwx --- ---  (owner only, full access)
777 = rwx rwx rwx  (everyone has full access) ⚠️
```

### Symbolic Permissions
```bash
chmod +x script.sh       # add execute for all
chmod u+x script.sh      # add execute for owner
chmod g-w file.txt       # remove write from group
chmod o=r file.txt       # set others to read-only
chmod a+r file.txt       # add read for all (a = all)
```

---

## Process Management

| Command | Description | Example |
|---------|-------------|---------|
| `ps` | List processes | `ps aux` |
| `top` | Live process viewer | `top` |
| `htop` | Enhanced process viewer | `htop` |
| `kill` | Send signal to process | `kill 1234` |
| `kill -9` | Force kill (SIGKILL) | `kill -9 1234` |
| `killall` | Kill by name | `killall firefox` |
| `pkill` | Kill by pattern | `pkill -f myapp` |
| `jobs` | List background jobs | `jobs` |
| `bg` | Resume job in background | `bg %1` |
| `fg` | Bring job to foreground | `fg %1` |
| `nohup` | Run immune to hangups | `nohup ./script.sh &` |
| `&` | Run command in background | `./script.sh &` |

### Common `ps` Usage
```bash
ps aux              # show all processes (all users)
ps aux | grep nginx # find nginx process
ps -ef              # full format listing
pgrep nginx         # show PIDs matching name
```

---

## Disk and Storage

| Command | Description | Example |
|---------|-------------|---------|
| `df` | Disk space usage | `df -h` |
| `du` | Disk usage of files/dirs | `du -sh /var/log` |
| `lsblk` | List block devices | `lsblk` |
| `mount` | Mount a filesystem | `mount /dev/sdb1 /mnt` |
| `umount` | Unmount a filesystem | `umount /mnt` |
| `fdisk` | Partition editor | `fdisk -l` |

```bash
df -h               # human-readable disk usage
du -sh *            # size of each item in current dir
du -sh /home/*      # size of each home directory
du -ah --max-depth=1 /var  # one level deep
```

---

## Networking

| Command | Description | Example |
|---------|-------------|---------|
| `ping` | Test connectivity | `ping google.com` |
| `curl` | Transfer data from URL | `curl https://example.com` |
| `wget` | Download files | `wget https://example.com/file.zip` |
| `ssh` | Secure shell remote login | `ssh user@192.168.1.10` |
| `scp` | Secure copy over SSH | `scp file.txt user@host:/tmp/` |
| `rsync` | Sync files efficiently | `rsync -av src/ dst/` |
| `ip addr` | Show IP addresses | `ip addr` |
| `ip route` | Show routing table | `ip route` |
| `netstat` | Network connections | `netstat -tuln` |
| `ss` | Socket statistics (modern netstat) | `ss -tuln` |
| `nslookup` | DNS lookup | `nslookup google.com` |
| `dig` | Detailed DNS lookup | `dig google.com` |
| `traceroute` | Trace network path | `traceroute google.com` |

---

## Text Processing

| Command | Description | Example |
|---------|-------------|---------|
| `echo` | Print text | `echo "Hello World"` |
| `sort` | Sort lines | `sort file.txt` |
| `uniq` | Remove duplicate lines | `sort file.txt \| uniq` |
| `wc` | Word/line/byte count | `wc -l file.txt` |
| `cut` | Extract columns | `cut -d: -f1 /etc/passwd` |
| `awk` | Pattern scanning/processing | `awk '{print $1}' file.txt` |
| `sed` | Stream editor | `sed 's/old/new/g' file.txt` |
| `tr` | Translate/delete characters | `echo "Hello" \| tr a-z A-Z` |
| `diff` | Compare files | `diff file1.txt file2.txt` |
| `tee` | Write to file and stdout | `ls \| tee output.txt` |

---

## Compression and Archives

| Command | Description | Example |
|---------|-------------|---------|
| `tar -czf` | Create gzip archive | `tar -czf archive.tar.gz dir/` |
| `tar -xzf` | Extract gzip archive | `tar -xzf archive.tar.gz` |
| `tar -cjf` | Create bzip2 archive | `tar -cjf archive.tar.bz2 dir/` |
| `zip` | Create zip archive | `zip archive.zip file1 file2` |
| `unzip` | Extract zip archive | `unzip archive.zip` |
| `gzip` | Compress a file | `gzip file.txt` |
| `gunzip` | Decompress gzip | `gunzip file.txt.gz` |

### tar Flags Explained
```
c = create archive
x = extract archive
z = use gzip compression
j = use bzip2 compression
v = verbose (show files)
f = filename follows
t = list contents without extracting

tar -tzvf archive.tar.gz   # list contents of archive
tar -xzf archive.tar.gz -C /tmp/  # extract to /tmp/
```

---

## System Information

| Command | Description | Example |
|---------|-------------|---------|
| `uname -a` | Kernel and system info | `uname -a` |
| `hostname` | Show/set hostname | `hostname` |
| `uptime` | System uptime and load | `uptime` |
| `whoami` | Current user | `whoami` |
| `id` | User and group IDs | `id` |
| `w` | Who is logged in + activity | `w` |
| `last` | Login history | `last` |
| `date` | Show/set date and time | `date` |
| `cal` | Display calendar | `cal` |
| `free` | Memory usage | `free -h` |
| `lscpu` | CPU information | `lscpu` |
| `lsmem` | Memory information | `lsmem` |
| `lspci` | PCI devices | `lspci` |
| `lsusb` | USB devices | `lsusb` |
| `dmesg` | Kernel ring buffer messages | `dmesg | tail` |

---

## User Management

| Command | Description | Example |
|---------|-------------|---------|
| `useradd` | Create user | `useradd john` |
| `usermod` | Modify user | `usermod -aG sudo john` |
| `userdel` | Delete user | `userdel john` |
| `passwd` | Change password | `passwd john` |
| `groupadd` | Create group | `groupadd developers` |
| `groups` | Show user's groups | `groups john` |
| `su` | Switch user | `su - john` |
| `sudo` | Execute as superuser | `sudo apt update` |

---

## Redirection and Pipes

```bash
# Output redirection
command > file.txt       # write stdout to file (overwrite)
command >> file.txt      # append stdout to file
command 2> error.txt     # write stderr to file
command &> all.txt       # write stdout + stderr to file

# Input redirection
command < file.txt       # read stdin from file

# Pipes
command1 | command2      # pipe stdout of cmd1 to stdin of cmd2

# Examples
ls -la | grep ".txt"                    # list only .txt files
cat /etc/passwd | cut -d: -f1 | sort   # list all usernames sorted
ps aux | grep firefox                   # find firefox processes
dmesg | tail -20                        # last 20 kernel messages
find . -name "*.log" | xargs rm        # delete all .log files found
```

---

## Shortcuts and Tips

### Keyboard Shortcuts (bash)
| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Kill current process |
| `Ctrl+Z` | Suspend current process |
| `Ctrl+D` | EOF / logout |
| `Ctrl+L` | Clear screen (same as `clear`) |
| `Ctrl+A` | Move cursor to beginning of line |
| `Ctrl+E` | Move cursor to end of line |
| `Ctrl+R` | Reverse search command history |
| `Tab` | Auto-complete command or path |
| `↑ / ↓` | Navigate command history |

### Useful One-Liners
```bash
# Find and kill a process by name
kill $(pgrep processname)

# Check what's listening on a port
ss -tlnp | grep :80

# Monitor a log file
tail -f /var/log/syslog

# Count files in a directory
ls | wc -l

# Find large files (>100MB)
find / -size +100M -type f 2>/dev/null

# Show most recently modified files
ls -lt | head -10

# Disk usage, sorted by size
du -ah /var | sort -rh | head -20

# Extract IP addresses from a file
grep -oE '\b([0-9]{1,3}\.){3}[0-9]{1,3}\b' file.txt
```

---
Previous: [How Linux Works](01-how-linux-works.md) | Next: [Shell Scripting Basics](03-shell-scripting.md)
