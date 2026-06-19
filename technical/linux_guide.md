# 🐧 Linux Systems & Operations — COMPLETE Study Guide
### GreyOrange Software Support Engineer Intern | Drive: June 23-24, 2026

> **Weight in Interview**: 45% | **Priority**: HIGHEST  
> All ⭐ marked items were confirmed asked in 2025 batch interviews.

---

## PART 1: Linux File System Hierarchy

Understanding where files live is crucial. Here is a directory map of a typical Linux system:

| Directory | Purpose | Support Context |
|---|---|---|
| **`/`** | Root — all folders branch from here | Starting point of the system directory tree. |
| **`/etc`** | Configuration files | DB configs (`postgresql.conf`), network configs, service scripts. |
| **`/var`** | Variable data | Databases, mail queues, and dynamically changing files. |
| **`/var/log`** | System & App Logs | **PRIMARY troubleshooting location**. Look here first. |
| **`/bin` & `/sbin`** | Binary executables | Standard system commands (`ls`, `ping`, `ifconfig`, `reboot`). |
| **`/proc`** | Virtual filesystem | Kernel state and processes represented as files (`/proc/cpuinfo`). |
| **`/tmp`** | Temporary files (cleared on reboot) | Storage for temp scripts, locks, and temporary run data. |
| **`/home`** | User home directories | Personal workspace directory for normal users (e.g. `/home/ravi`). |
| **`/root`** | Superuser home | Admin/root workspace directory. |
| **`/opt`** | Optional/third-party software | Location where enterprise applications and robot drivers are installed. |
| **`/usr`** | User system resources | Shared libraries, system binaries, and read-only user data. |
| **`/dev`** | Device files | System hardware interfaces (e.g. `/dev/sda` for disk, `/dev/null`). |

---

## PART 2: Command Flag Master Matrix

Support engineers must know how to modify commands using flags to parse large volumes of system data.

### `ls` — List Directory Contents
| Flag | Meaning | Example |
|---|---|---|
| `-l` | Long listing (permissions, owner, size, date) | `ls -l` |
| `-a` | All files including hidden (starting with `.`) | `ls -a` (displays `.bashrc`) |
| `-h` | Human-readable sizes (KB, MB, GB instead of bytes) | `ls -lh` |
| `-t` | Sort by time modified (newest first) | `ls -lt` |
| `-S` | Sort by file size (largest first) | `ls -lhS` |
| `-R` | Recursive listing of all subdirectories | `ls -R /var/log` |

### `grep` — Search Text in Files
| Flag | Meaning | Example |
|---|---|---|
| `-i` | Case-insensitive search | `grep -i "error" app.log` |
| `-v` | Invert match (exclude lines containing keyword) | `grep -v "DEBUG" app.log` |
| `-rn` | Recursive search with line numbers | `grep -rn "timeout" /var/log/` |
| `-c` | Count matching lines | `grep -c "Exception" app.log` |
| `-A N` | Show N lines **After** match (context) | `grep -A 5 "NullPointer" app.log` |
| `-B N` | Show N lines **Before** match (context) | `grep -B 3 "Timeout" app.log` |
| `-l` | Print only filenames containing matches | `grep -rl "ERROR" /var/log/` |
| `-w` | Match whole word only | `grep -w "fail" app.log` |

### `find` — Search for Files
| Flag | Meaning | Example |
|---|---|---|
| `-name` | Match by filename pattern | `find /var/log -name "*.log"` |
| `-size` | Find files matching size threshold | `find . -size +100M` (files > 100MB) |
| `-mtime` | Modified in the last N days | `find . -mtime -2` (last 48 hours) |
| `-type f` | Search for files only | `find /etc -type f` |
| `-type d` | Search for directories only | `find /var -type d` |

### `tail` & `head` — File Snippets
| Command | Meaning | Example |
|---|---|---|
| `tail -n N file` | Print last N lines of a file | `tail -n 50 app.log` |
| `tail -f file` | Follow file modifications in real-time | `tail -f /var/log/syslog` |
| `head -n N file` | Print first N lines of a file | `head -n 20 app.log` |

---

## PART 3: ⭐ DCS-Confirmed Interview Commands (MEMORIZE ALL 6)

These six commands were confirmed as explicitly asked in the 2025 batch interviews. Memorize their syntax and context:

### ⭐ 1. `tail -10 logfile.log`
* **Purpose**: Show the last 10 lines of a log file.
* **Support Context**: Used to inspect the latest error or stack trace written right before an application crashed.

### ⭐ 2. `scp file.txt user@host:/path`
* **Purpose**: Securely copy file to/from a remote server.
* **Support Context**: Transfers logs from production nodes to a local machine for analysis, or uploads patches.
* **Syntax**: `scp [source] [destination]`. Add `-r` to copy directories recursively: `scp -r /var/log/ ravi@server:/backup/`

### ⭐ 3. `chmod +x filename`
* **Purpose**: Add execute permission to make a script runnable.
* **Support Context**: Run when automating checks using a custom Shell or Python script.
* **Usage**: `chmod +x health_check.sh` followed by `./health_check.sh`.

### ⭐ 4. `chown user filename`
* **Purpose**: Change ownership of a file.
* **Support Context**: Fixes "Permission Denied" errors when an application runs under a specific user and cannot write to log or database directories.
* **Usage**: `sudo chown greyuser /var/log/app.log` or `sudo chown greyuser:greygroup /var/log/app.log` (user:group).

### ⭐ 5. `free -m`
* **Purpose**: Check system memory (RAM) in Megabytes.
* **Support Context**: Look at the **"available"** column rather than "free". Linux utilizes unused RAM for buffer cache, which is automatically freed when applications request it.

### ⭐ 6. `htop` / `top`
* **Purpose**: Real-time process and system resource monitor.
* **Support Context**: Debugs CPU spikes. `htop` is preferred as it is interactive, visualizes individual cores, allows sorting (Shift+P for CPU, Shift+M for memory), and supports process termination using `F9` (kill).

---

## PART 4: File Permissions — The 4-2-1 Rule

File permissions in directory listings (`ls -l`) look like: `-rwxr-xr--`
The first character is the type (`-` = file, `d` = directory, `l` = link). The next 9 characters represent three sets of permissions: User (owner), Group, and Others.

```
Type  User(Owner)  Group      Others
-      r w x        r - x      r - -
       4+2+1=7      4+0+1=5    4+0+0=4    =>  chmod 754
```

### The Binary Value Mapping:
* **Read (r)** = 4
* **Write (w)** = 2
* **Execute (x)** = 1
* **No Permission (-)** = 0

### Common Permission Patterns:
| Numeric | Symbolic | Use Case | Safety / Access |
|---|---|---|---|
| **777** | `rwxrwxrwx` | Full access for everyone | **NEVER in production**. Extremely dangerous security risk. |
| **755** | `rwxr-xr-x` | Owner has full; Group/Others can read/execute | Standard for executable shell/python scripts. |
| **644** | `rw-r--r--` | Owner can edit; Group/Others can read | Standard for configuration files and static logs. |
| **600** | `rw-------` | Only owner can read/edit; others blocked | Mandatory for private files like SSH private keys (`id_rsa`). |
| **400** | `r--------` | Only owner can read; all edits blocked | Used for highly sensitive read-only credentials or keys. |

### Symbolic Changes:
* `chmod u+x file`     # Add execute permission for owner
* `chmod g-w file`     # Remove write permission for group
* `chmod o=r file`     # Set others permission to read-only
* `chmod a+r file`     # Add read permission for ALL (User, Group, Others)

---

## PART 5: I/O Redirection & Pipes

Redirection lets you capture logs, clean outputs, and write automation scripts.

| Operator | Action | Example |
|---|---|---|
| **`>`** | Overwrite stdout to file | `echo "Active" > status.txt` |
| **`>>`** | Append stdout to file | `echo "$(date)" >> status.txt` |
| **`2>`** | Redirect stderr only | `script.sh 2> errors.log` |
| **`&>`** | Redirect stdout AND stderr | `script.sh &> full.log` |
| **`\|`** | Feed stdout of left cmd as stdin to right cmd | `ps aux \| grep java \| wc -l` |
| **`/dev/null`**| Discard output (black hole) | `noisy_cmd &> /dev/null` |

### Power Pipe Combos:
* **Count ERROR lines**: `grep -c "ERROR" app.log`
* **Find top 10 largest directories**: `du -sh /var/log/* | sort -rh | head -10`
* **Identify port owner**: `lsof -i :8080`
* **Live filter logs**: `tail -f app.log | grep --line-buffered "ERROR"`
* **Deduplicate and count unique IPs**: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -20`

---

## PART 6: Advanced Log-Parsing Recipes

Use these recipes in high-stress production environments:

### Recipe 1: Search Compressed (Rotated) Logs
Standard logs are rotated and compressed (ending in `.gz`) to save disk space.
```bash
zgrep -i "exception" /var/log/app.log.1.gz
zcat /var/log/app.log.1.gz | head -100
```

### Recipe 2: Extract Full Stack Trace Context
To troubleshoot code exceptions, look at the lines immediately following the error.
```bash
grep -n -A 20 "NullPointerException" app.log
```

### Recipe 3: Extract Events in specific Time Range
```bash
sed -n '/14:30:00/,/14:35:00/p' app.log
```

### Recipe 4: Real-time Error Monitoring with Pipes
```bash
tail -f /var/log/app.log | grep --line-buffered -i "error\|fatal"
```

---

## PART 7: System Health Commands

### CPU & Processes
* `top`                         # Interactive monitor (Shift+P sorts by CPU)
* `htop`                        # Modern interactive monitor
* `ps aux`                      # Snapshot of all processes
* `ps aux | grep python`        # Filter for Python scripts
* `uptime`                      # System uptime and load averages (1, 5, 15 minutes)
* `nproc`                       # Print number of CPU cores available

### Memory (RAM)
* `free -m`                     # RAM in Megabytes
* `free -h`                     # RAM in human-readable format
* `dmesg | grep -i oom`         # Scan system logs for Out-Of-Memory kills
* `vmstat 1 5`                  # Memory, swap, and system resource statistics every 1s

### Disk I/O & Storage
* `df -h`                       # Disk space usage on mounted filesystems
* `df -i`                       # Free Inode counts (if inodes are 100% full, no new files can be written)
* `du -sh /var/log/`            # Total size of a directory
* `du -sh /var/log/* | sort -h` # Sort folder items by size
* `lsblk`                       # List block devices (disks, partitions)
* `iostat`                      # Disk input/output statistics

### Network Connectivity
* `ip addr show`                # Display IP addresses and interfaces
* `ss -tulnp`                   # Modern active listening ports with PIDs
* `netstat -tulnp`              # Legacy active ports with PIDs
* `ping 8.8.8.8`                # Test Layer 3 connectivity
* `traceroute 8.8.8.8`          # Trace network routing hops
* `nc -zv host 3306`            # Test port TCP connection (Netcat)
* `curl -I https://google.com`  # Fetch HTTP headers
* `dig greyorange.com`          # DNS details lookup

---

## PART 8: SSH & Remote Operations

* **Standard Login**: `ssh username@192.168.1.100`
* **Custom Port**: `ssh -p 2222 username@host`
* **Local to Remote copy**: `scp local_file.txt user@host:/remote/path/`
* **Remote to Local copy**: `scp user@host:/remote/file.txt ./local/`
* **Key Generation**: `ssh-keygen -t rsa -b 4096`
* **Passwordless Setup**: `ssh-copy-id user@host` (copies public key to remote host)
* **Execute remote command**: `ssh user@host "df -h && free -m"`

---

## PART 9: Service Management (`systemctl`)

* `sudo systemctl status nginx`           # Check service status
* `sudo systemctl start nginx`            # Start service
* `sudo systemctl stop nginx`             # Stop service
* `sudo systemctl restart nginx`          # Restart service
* `sudo systemctl reload nginx`           # Reload config without terminating connection
* `sudo systemctl enable nginx`           # Enable start-on-boot
* `sudo journalctl -u nginx -n 100`       # View last 100 logs for Nginx service
* `sudo journalctl -u nginx -f`           # Live follow service logs

---

## PART 10: Cron Jobs (Scheduled Tasks)

* `crontab -e`              # Edit cron configuration
* `crontab -l`              # List scheduled cron jobs

Syntax format:
`minute  hour  day_of_month  month  day_of_week  command`

| Expression | Meaning |
|---|---|
| `0 * * * *` | Every hour at minute 0 |
| `*/15 * * * *` | Every 15 minutes |
| `0 2 * * *` | Every day at 2:00 AM |
| `0 9 * * 1` | Every Monday at 9:00 AM |
| `0 0 1 * *` | First of every month at midnight |

---

## PART 11: Soft Links vs Hard Links

| Feature | Soft Link (Symlink) | Hard Link |
|---|---|---|
| **Command** | `ln -s source target` | `ln source target` |
| **Reference** | Points to path of the original file | Points to raw disk data blocks (inode) |
| **Deletion Impact**| If original file is deleted, link is broken | If original is deleted, data remains accessible |
| **Cross-Filesystem**| Yes | No (restricted to same partition) |
| **Directory Support**| Yes | No |

---

## PART 12: Complete Q&A Bank (30 Questions)

#### Q1: Difference between Linux and Unix?
* **Answer**: Linux is a free, open-source kernel created by Linus Torvalds in 1991. It is compiled with GNU tools to create distributions (Ubuntu, RedHat). Unix is a proprietary, closed-source operating system created by AT&T Bell Labs in 1969, usually tied to specific hardware (IBM AIX, HP-UX).

#### Q2: How do you check running processes?
* **Answer**: Use `ps aux` to output a snapshot of all active processes. Use `ps aux | grep nginx` to filter for specific processes, or open `htop` / `top` for a real-time interactive CPU/memory utilization feed.

#### Q3: How do you search for text in a file?
* **Answer**: Use `grep`. Use `grep "ERROR" app.log` to scan. Add `-i` for case-insensitivity: `grep -i "exception" app.log`. Add `-rn` to search recursively inside a folder with line numbers: `grep -rn "timeout" /var/log/`.

#### Q4: What is an alias? How to make it persistent?
* **Answer**: An alias is a customized command shortcut. Set temporarily using `alias ll='ls -la'`. To make it persistent for a user, write `alias ll='ls -la'` to `~/.bashrc` and run `source ~/.bashrc`. For all system users, append it to `/etc/bash.bashrc`.

#### Q5: How to check available disk space?
* **Answer**: Check overall filesystem usage using `df -h`. For checking individual directories, execute `du -sh /var/log/` or sort subdirectories by size: `du -sh * | sort -h`.

#### Q6: What does chmod 755 mean?
* **Answer**: User gets full permissions (`7` = 4+2+1 = rwx). Group gets read and execute (`5` = 4+0+1 = r-x). Others get read and execute (`5` = 4+0+1 = r-x).

#### Q7: CPU spikes to 100% — first 5 steps?
* **Answer**:
  1. Open `htop` and press `Shift+P` to identify the PID consuming the most CPU.
  2. Analyze the command: is it a runaway process (infinite loop) or a normal reindexing task?
  3. Run `lsof -p PID` to examine what files and ports the process has open.
  4. Inspect logs of the application for continuous exceptions.
  5. Send `kill -15 PID` (SIGTERM) to stop it gracefully. If unresponsive, send `kill -9 PID` (SIGKILL).

#### Q8: Server is unresponsive — what steps?
* **Answer**:
  1. Run `ping server-ip` to check Layer 3 connectivity. If it fails, run `traceroute` to find the hop failure.
  2. Run `nc -zv server-ip 22` to check if SSH daemon is listening.
  3. If reachable, SSH in and run `df -h` (check for 100% disk utilization) and `free -m` (check for memory exhaustion).
  4. Run `dmesg | grep -i oom` to see if the kernel terminated services.
  5. Check service health with `sudo systemctl status service-name` and view logs with `tail -100 /var/log/syslog`.

#### Q9: Difference between kill -9 and kill -15?
* **Answer**: `kill -15` (SIGTERM) requests the process to terminate. The process can catch this signal, clean up temporary resources, save state, and close open files. `kill -9` (SIGKILL) is sent to the kernel to terminate the process immediately. The process cannot catch or ignore this signal, risking data corruption.

#### Q10: What is `/dev/null`?
* **Answer**: A virtual device file (often called the black hole). Any data written to it is discarded. Used to silence verbose commands: `command &> /dev/null`.

#### Q11: How to check which process uses port 8080?
* **Answer**: Run `lsof -i :8080` or execute `ss -tulnp | grep 8080`.

#### Q12: Difference between `cp` and `mv`?
* **Answer**: `cp` copies the data to a new location, leaving the original intact. `mv` renames the file or moves it to a new path, deleting the original entry from the source directory.

#### Q13: What is `2>/dev/null`?
* **Answer**: Redirects standard error output (File Descriptor 2) to `/dev/null` while allowing standard output (stdout) to print normally in the terminal.

#### Q14: How to find the top 5 largest files in `/var/log`?
* **Answer**: Run `du -sh /var/log/* | sort -rh | head -5` or execute `find /var/log -type f -exec du -h {} + | sort -rh | head -5`.

#### Q15: What does `ps aux | grep java | wc -l` do?
* **Answer**: 
  1. `ps aux` lists all running processes.
  2. `grep java` filters for lines containing the word "java".
  3. `wc -l` counts the number of lines. The full command returns the number of active Java processes plus one (for the grep process itself).

#### Q16: Difference between `>` and `>>`?
* **Answer**: `>` redirects output to a file, overwriting its existing content. `>>` redirects output to the end of a file, appending new data and preserving old content.

#### Q17: How do you monitor a log file in real-time?
* **Answer**: Use `tail -f /var/log/app.log`. To filter for error lines in real-time, execute `tail -f /var/log/app.log | grep --line-buffered "ERROR"`.

#### Q18: How do you run a script in the background?
* **Answer**: Append `&` to the end of the command: `./script.sh &`. To keep it running after the terminal session closes, prepend `nohup`: `nohup ./script.sh &`.

#### Q19: How to schedule a task every day at 3 AM?
* **Answer**: Run `crontab -e` and append: `0 3 * * * /path/to/script.sh`.

#### Q20: What is the purpose of `/proc`?
* **Answer**: It is a virtual pseudo-filesystem containing real-time kernel variables, hardware statistics, and process structures represented as file hierarchies (e.g. `/proc/meminfo`, `/proc/cpuinfo`).

#### Q21: How to check open network connections?
* **Answer**: Execute `ss -an` (modern, fast) or use the legacy `netstat -an` command.

#### Q22: What is `lsof`?
* **Answer**: "List Open Files". In Linux, everything is treated as a file. `lsof` lists files, network sockets, directories, and pipes opened by active processes. Use `lsof -i` for network ports or `lsof -u username` for files opened by a specific user.

#### Q23: What does `uptime` show?
* **Answer**: Displays the current system time, how long the system has been running, the number of logged-in users, and system load averages over the last 1, 5, and 15 minutes.

#### Q24: How to find which user is logged in?
* **Answer**: Run `who` to list active users. Run `w` to view who is logged in and their current process activity. Run `whoami` to verify the user account you are currently logged in with.

#### Q25: How to redirect both stdout and stderr to a file?
* **Answer**: Use `command &> file.log` (modern bash) or the POSIX-compliant syntax: `command > file.log 2>&1`.

#### Q26: What is a daemon process?
* **Answer**: A background process running continuously without being attached to an active terminal session. Daemons typically handle services (e.g. `sshd`, `cron`, `nginx`).

#### Q27: How to check Linux version?
* **Answer**: Read OS release parameters with `cat /etc/os-release` or check the kernel version using `uname -r`.

#### Q28: What does `wc -l` do?
* **Answer**: Word Count - Lines. It counts the total number of lines in a text file or stream input.

#### Q29: How to compress a file in Linux?
* **Answer**: Run `gzip file.log` (replaces with `file.log.gz`). To compress while keeping the original file, execute `gzip -k file.log`.

#### Q30: How do you read a compressed log without extracting?
* **Answer**: Open the file in the terminal using `zless file.log.gz` or run `zcat file.log.gz` to pipe its output. Use `zgrep "pattern" file.log.gz` to search inside it.

---

## PART 13: Quick Revision Cheat Sheet (Last Hour Revision)

```bash
# LOG INSPECTION
tail -f /var/log/app.log                          # Follow live
tail -100 /var/log/app.log | grep ERROR           # Last 100, filter
grep -A 10 "Exception" app.log                    # Stack trace context
sed -n '/14:30/,/14:35/p' app.log                 # Time range

# PERMISSIONS
chmod 755 script.sh                               # Standard script
chmod +x script.sh                                # Make executable
chown appuser:appgroup /var/log/app.log           # Fix ownership

# PROCESS INVESTIGATION
ps aux | grep python                              # Find python process
lsof -i :3306                                     # Who uses MySQL port
kill -15 PID                                      # Graceful kill
kill -9 PID                                       # Force kill

# DISK EMERGENCY
df -h                                             # Check disk usage
du -sh /var/log/* | sort -rh | head -10           # Find log hogs
gzip /var/log/old_app.log                         # Compress to save space

# NETWORK DIAGNOSTICS
ping 8.8.8.8                                      # Basic connectivity
nc -zv hostname 5432                              # Test PostgreSQL port
curl -I https://api.greyorange.com                # HTTP header check
```
