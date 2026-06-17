# 🐧 Linux Systems & Operations Preparation Guide

In a Software Support Engineer role at GreyOrange, Linux is your primary environment. You will be troubleshooting warehouse software, checking system health, reading logs, and writing scripts on Linux servers.

---

## 🔍 Core Concept: Linux vs Unix
* **Unix**: A proprietary operating system developed in the late 1960s at AT&T Bell Labs (e.g., AIX, Solaris, HP-UX). It is closed-source and has specific hardware requirements.
* **Linux**: An open-source, free kernel created by Linus Torvalds in 1991. Linux is combined with GNU utilities to form full operating systems (distributions or "distros") like Ubuntu, Red Hat, CentOS, and Debian.
* **Key Difference**: Unix is a full, proprietary OS, while Linux is an open-source kernel that powers various OS distributions.

---

## 📋 Specific LPU DCS Email Questions & Answers

### Q1: What is the basic difference between Linux and Unix?
* **Answer**: UNIX is a proprietary, full-fledged operating system family, whereas Linux is a free and open-source kernel. Linux distributions are built on top of the Linux kernel combined with GNU tools, making Linux a popular, free alternative to Unix.

### Q2: How can you copy a log file from one Linux server to another?
* **Answer**: By using the `scp` (Secure Copy) command.
  * **Syntax**: `scp /path/to/local/logfile.log username@remote_host:/path/to/destination/`
  * Alternatively, `rsync` can be used for syncing directories and large log files efficiently: `rsync -avz logfile.log username@remote_host:/destination/`

### Q3: If you have a log file and you need to see the last 10 lines of that log file, how can you do it?
* **Answer**: By using the `tail` command.
  * **Syntax**: `tail -10 logfile.log` (or `tail -n 10 logfile.log`).
  * *Tip for Support*: To view logs in real-time as they update, use `tail -f logfile.log`.

### Q4: If you need to change permissions of any file to executable and change its ownership, how can we do that?
* **Answer**:
  * **Changing Permission**: Use `chmod` command.
    * `chmod +x filename` (makes it executable for all).
    * `chmod 755 filename` (owner can read/write/execute; group/others can read/execute).
  * **Changing Owner**: Use `chown` command.
    * `sudo chown newowner filename`.
    * To change both owner and group: `sudo chown newowner:newgroup filename`.

### Q5: If you need to check the memory consumption and CPU utilization of any Linux machine, how can you do that?
* **Answer**:
  * **Memory Consumption**: Use `free -m` (displays memory in Megabytes) or `free -h` (human-readable format).
  * **CPU Utilization**: Use `top` (standard utility) or `htop` (interactive, colorful, and user-friendly interface).

### Q6: What are aliases in Linux, and how can you make an alias that is usable by all users on a Linux server?
* **Answer**: An alias is a shortcut for a long command.
  * To make an alias for a single user, edit `~/.bashrc`.
  * To make an alias **usable by all users (system-wide)**, edit `/etc/bash.bashrc` (Debian/Ubuntu) or `/etc/profile` by adding:
    `alias mycommand='actual_long_command'`
  * Then run `source /etc/bash.bashrc` to apply the changes immediately.

---

## 🛠️ Essential Linux Command Reference

### 1. Directory Navigation & Information
* `pwd`: Print Working Directory. Shows your current location.
* `ls`: List files in the current directory.
  * `ls -a`: List all files (including hidden files starting with `.`).
  * `ls -l`: List files in long format (displays permissions, owner, size, date).
  * `ls -lh`: List files in long format with human-readable sizes (KB, MB, GB).
* `cd`: Change directory.
  * `cd ..`: Go up one directory level.
  * `cd ~`: Go to home directory.
  * `cd -`: Go to the previous directory you were working in.

### 2. File and Directory Operations
* `touch filename`: Create an empty file or update timestamps.
* `mkdir dirname`: Create a new directory.
* `cp source destination`: Copy files. Use `cp -r` to copy directories recursively.
* `mv source destination`: Move or rename files/directories.
* `rm filename`: Remove a file.
  * `rm -r dirname`: Remove a directory recursively.
  * `rm -rf dirname`: Forceful recursive removal (use with extreme caution!).
* `ln -s target link_name`: Create a symbolic link (shortcut) to a file.

### 3. File Permissions & Ownership Deep Dive
In Linux, files have 3 permission types for 3 user categories:
* **User categories**: Owner (`u`), Group (`g`), Others (`o`).
* **Permission types**: Read (`r` = 4), Write (`w` = 2), Execute (`x` = 1).

| Permission String | Numeric | Description |
|---|---|---|
| `rwx------` | 700 | Only owner can read, write, and execute. |
| `rwxr-xr-x` | 755 | Owner can read/write/execute. Group & others can read/execute. (Common for scripts). |
| `rw-r--r--` | 644 | Owner can read/write. Group & others can only read. (Common for configs/logs). |
| `rwxrwxrwx` | 777 | Everyone can do everything. (High security risk!). |

* **Commands**:
  * `chmod 755 script.sh`: Set permissions using octal numbers.
  * `chmod u+x script.sh`: Add execute permission only to the owner.
  * `chown ravish:dev team_log.log`: Change owner to `ravish` and group to `dev`.

### 4. Text Processing & Log Analysis (Crucial for Support)
As a support engineer, you will spend 50% of your time reading logs.
* `cat filename`: Print the entire file contents to the screen.
* `less filename`: Open file for page-by-page reading (use arrow keys, press `q` to exit). Much safer for large files than `cat`.
* `head -n 20 filename`: Show the first 20 lines of a file.
* `tail -n 20 filename`: Show the last 20 lines of a file.
* `tail -f /var/log/app.log`: Monitor incoming logs in real-time (press `Ctrl+C` to stop).
* `grep "keyword" file`: Search for a specific pattern inside a file.
  * `grep -i "error" app.log`: Case-insensitive search for "error".
  * `grep -c "exception" app.log`: Count the occurrences of "exception".
  * `grep -rn "failed" /var/log/`: Recursively search for "failed" in all files in `/var/log` and display line numbers.
* `find /path -name "*.log"`: Find all files matching the pattern in the path.
  * `find . -size +100M`: Find files larger than 100MB in the current directory.
* `awk '{print $1}' logfile.log`: Print the first column of each line (useful for extracting IP addresses or timestamps from logs).
* `sed -i 's/old/new/g' config.txt`: Replace all occurrences of "old" with "new" directly in `config.txt`.

### 5. System Health & Performance
* `df -h`: Shows disk space usage of all mounted filesystems.
* `du -sh *`: Shows the size of all directories and files in the current folder.
* `free -m`: Shows free and used RAM in Megabytes.
* `uptime`: Shows how long the system has been running, number of users, and system load average (over 1, 5, and 15 minutes).
* `top`: Live task manager showing CPU, Memory, and active processes.
* `htop`: Interactive task manager (often preferred for better readability).
* `ps aux`: Displays details of all running processes in the system.
  * `ps aux | grep python`: Find all running Python processes.
* `kill -9 PID`: Force-kill a process using its Process ID (PID).
* `lsof -i :8080`: List files (and processes) using port 8080.

---

## 📝 30+ Linux Practice Q&As (Self-Test)

### File Operations
#### Q1: What happens if you run `cd -`?
* **A**: It returns you to the directory you were in immediately before the current one.

#### Q2: How do you create a directory structure like `project/src/tests` with a single command?
* **A**: Use the `-p` (parents) flag: `mkdir -p project/src/tests`.

#### Q3: Difference between `rm -r` and `rm -f`?
* **A**: `-r` deletes directories recursively (prompting for write-protected files). `-f` forces deletion without prompting and ignores non-existent files. Combining them (`rm -rf`) deletes everything instantly.

#### Q4: How do you count the number of lines, words, and characters in a file?
* **A**: Use the `wc` command: `wc filename` (or `wc -l filename` to get only the line count).

#### Q5: What is a symbolic link, and how is it different from a hard link?
* **A**: A symbolic link (symlink) is like a desktop shortcut; it points to the path of another file. If the original file is deleted, the symlink breaks. A hard link points directly to the inode (raw data block) on the disk; even if the original file is deleted, the hard link still accesses the content.

### Searching & Filtering
#### Q6: How do you search for files modified in the last 24 hours?
* **A**: Use `find . -mtime -1`.

#### Q7: How do you search for the word "database" in all files within a directory, ignoring case?
* **A**: Use `grep -ri "database" /path/to/directory/`.

#### Q8: How can you find all process IDs running a service named "java"?
* **A**: Use `pgrep java` or `ps aux | grep java | grep -v grep`.

#### Q9: How do you view lines 50 to 60 of a text file?
* **A**: Use a combination of head and tail: `head -n 60 file.txt | tail -n 11`. (Or use sed: `sed -n '50,60p' file.txt`).

#### Q10: How do you redirect the output of a command to a file, overwriting it? And how do you append to it?
* **A**: Use `>` to overwrite: `echo "hello" > file.txt`. Use `>>` to append: `echo "world" >> file.txt`.

### Permissions & System Admin
#### Q11: Explain what `-rwxr-xr--` permission means.
* **A**:
  * Owner has read, write, and execute (`rwx` = 7).
  * Group has read and execute (`r-x` = 5).
  * Others have read-only (`r--` = 4).
  * In numeric form: `754`.

#### Q12: How do you run a command with root (administrator) privileges?
* **A**: Prefix the command with `sudo` (e.g., `sudo systemctl restart apache2`).

#### Q13: What does the command `chmod +x script.py` do?
* **A**: It adds execute permissions for the owner, group, and others on `script.py`.

#### Q14: How do you check which user you are currently logged in as?
* **A**: Run the `whoami` command.

#### Q15: How do you list all members of a specific group?
* **A**: Run `getent group group_name` or view the `/etc/group` file.

### System Health & Troubleshooting
#### Q16: How do you check if your server's root disk partition is full?
* **A**: Run `df -h /`. Look at the "Use%" column.

#### Q17: What does the "Load Average" in `uptime` mean?
* **A**: It shows the average system load over 1, 5, and 15 minutes. It represents the number of processes that are either running or waiting for CPU time. On a 4-core system, a load average of 4.0 means the CPU is at 100% capacity.

#### Q18: How do you check which process is consuming the most RAM?
* **A**: Run `top` and press `Shift + M` to sort by memory usage (or use `htop` and click on the MEM% column).

#### Q19: If a service is stuck and not responding, how do you find its PID and terminate it immediately?
* **A**: Find PID with `ps aux | grep service_name` or `pgrep service_name`. Terminate it immediately with `kill -9 PID`.

#### Q20: How do you check the IP address of your Linux machine?
* **A**: Run `ip addr show` or `ifconfig` (if net-tools is installed) or `hostname -I`.

### Networking & Log management
#### Q21: How do you check if a remote server is reachable?
* **A**: Use `ping IP_ADDRESS` or `ping domain.com`.

#### Q22: How do you download a file from the internet directly using the terminal?
* **A**: Use `wget URL` or `curl -O URL`.

#### Q23: How do you check which network ports are currently open and listening on your server?
* **A**: Run `netstat -tuln` or `ss -tuln`.
  * `-t` = TCP, `-u` = UDP, `-l` = Listening, `-n` = Numeric ports.

#### Q24: What is the purpose of the `/var/log` directory?
* **A**: It stores system, application, and daemon log files (e.g., `/var/log/auth.log` for authentication/login events, `/var/log/syslog` for general messages).

#### Q25: How do you view log updates in real-time?
* **A**: Run `tail -f /var/log/app.log`.

### Advanced Support Operations
#### Q26: How do you compress a directory named `logs/` into a compressed archive named `logs.tar.gz`?
* **A**: Run `tar -czvf logs.tar.gz logs/`.
  * `-c` = create, `-z` = gzip compression, `-v` = verbose, `-f` = file name.

#### Q27: How do you extract `logs.tar.gz`?
* **A**: Run `tar -xzvf logs.tar.gz`.
  * `-x` = extract.

#### Q28: How do you find and list all files larger than 50MB and delete them in one command?
* **A**: Run `find . -type f -size +50M -exec rm -f {} \;` (use with extreme caution!).

#### Q29: What is `cron`, and how do you schedule a script to run every day at midnight?
* **A**: `cron` is a time-based job scheduler. Run `crontab -e` and add the line:
  `0 0 * * * /path/to/script.sh`

#### Q30: How do you check the last 5 commands you ran in the terminal?
* **A**: Run the `history` command and look at the last few lines, or run `history | tail -5`.
