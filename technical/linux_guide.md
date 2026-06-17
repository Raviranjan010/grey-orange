# 🐧 Linux Systems & Operations Prep Guide

In a Software Support Engineer role at GreyOrange, Linux is your primary environment. You will be troubleshooting warehouse software, checking system health, reading logs, and writing scripts on Linux servers.

---

## ⭐ Confirmed 2025 Batch Interview Commands
These commands were explicitly asked in the GreyOrange interviews for the 2025 batch. Make sure you know them by heart.

### 1. ⭐ `tail -10 logfile.log`
* **Purpose**: Show the last 10 lines of a log file.
* **Usage**: Extremely common in support roles to check the latest errors or system messages.
* **Pro Tip**: Use `tail -f logfile.log` to watch logs scroll in real-time as events occur.

### 2. ⭐ `scp file.txt user@host:/path`
* **Purpose**: Securely copy a file from a local machine to a remote server (or vice-versa) over SSH.
* **Usage**: Copying logs, configuration files, or scripts between environments.

### 3. ⭐ `chmod +x filename`
* **Purpose**: Add execute permission (`+x`) to a file, making it runnable as a program or script.
* **Usage**: Preparing shell (`.sh`) or Python (`.py`) scripts for execution.

### 4. ⭐ `chown user filename`
* **Purpose**: Change the owner of a file to a specific user.
* **Usage**: Resolving "Permission Denied" errors when an application process cannot write to a file because it is owned by a different user (e.g. `root`).
* **Note**: Use `chown user:group filename` to change both owner and group.

### 5. ⭐ `free -m`
* **Purpose**: Check RAM (memory) usage on the machine in Megabytes.
* **Usage**: Displays total, used, free, shared, buffer/cache, and available physical memory and swap space.

### 6. ⭐ `htop` / `top`
* **Purpose**: Real-time system monitoring (CPU, RAM, Running Processes).
* **Usage**: `top` is the standard system monitor. `htop` is a newer, colorful, and interactive command-line interface that allows you to search, sort, and kill processes easily.

---

## ▾ Expandable Core Commands Q&A

Please tap on any question below to expand its detailed answer.

<details>
<summary><b>Q1: What is the basic difference between Linux and Unix?</b></summary>
<br>
<blockquote>
<b>Answer:</b> 
<ul>
  <li><b>Linux is a Kernel</b>: It is free and open-source, created by Linus Torvalds in 1991. It is combined with GNU utilities to form full operating systems called distributions (e.g., Ubuntu, CentOS, Red Hat).</li>
  <li><b>Unix is a Full Operating System</b>: Developed by AT&T Bell Labs in the late 1960s, it is closed-source, proprietary, and usually runs on specific hardware (e.g., IBM AIX, Oracle Solaris, HP-UX).</li>
  <li><b>Summary</b>: UNIX is a proprietary OS family, while Linux is an open-source kernel used to build free operating systems.</li>
</ul>
</blockquote>
</details>

<details>
<summary><b>Q2: How do you check running processes on a Linux machine?</b></summary>
<br>
<blockquote>
<b>Answer:</b>
<ul>
  <li>Use <code>ps aux</code> to get a detailed snapshot of all running processes on the system:
    <ul>
      <li><code>a</code> = show processes for all users.</li>
      <li><code>u</code> = display user/owner details.</li>
      <li><code>x</code> = show processes not attached to a terminal.</li>
    </ul>
  </li>
  <li>To find a specific process (e.g., a Python app), pipe it to grep: <code>ps aux | grep python</code>.</li>
  <li>For real-time interactive monitoring, use <code>top</code> or <code>htop</code>.</li>
</ul>
</blockquote>
</details>

<details>
<summary><b>Q3: How do you search for specific text inside a file?</b></summary>
<br>
<blockquote>
<b>Answer:</b>
By using the <code>grep</code> command (Global Regular Expression Print).
<ul>
  <li><b>Basic Search</b>: <code>grep "ERROR" app.log</code></li>
  <li><b>Case-Insensitive</b>: <code>grep -i "exception" app.log</code> (finds Exception, exception, EXCEPTION).</li>
  <li><b>Show Line Numbers</b>: <code>grep -n "failed" transaction.log</code>.</li>
  <li><b>Recursive Search</b>: <code>grep -r "NullPointerException" /var/log/app/</code> (searches all files inside the folder).</li>
  <li><b>Count Matches</b>: <code>grep -c "timeout" app.log</code> (returns the count of lines containing the term).</li>
</ul>
</blockquote>
</details>

<details>
<summary><b>Q4: What are aliases in Linux? How do you make an alias persistent for all users?</b></summary>
<br>
<blockquote>
<b>Answer:</b>
An alias is a customized shortcut for a long or frequently used command.
<ul>
  <li><b>Temporary Alias</b>: <code>alias ll='ls -la'</code> (lasts only for the current terminal session).</li>
  <li><b>Persistent for One User</b>: Add the alias definition to the user's <code>~/.bashrc</code> file, then run <code>source ~/.bashrc</code>.</li>
  <li><b>Persistent System-Wide (All Users)</b>: 
    <ol>
      <li>Open the system-wide configuration file using sudo: <code>sudo nano /etc/bash.bashrc</code> (on Debian/Ubuntu) or <code>sudo nano /etc/profile</code>.</li>
      <li>Append the alias at the end: <code>alias mycommand='actual_long_command'</code>.</li>
      <li>Save and exit (Ctrl+O, Enter, Ctrl+X in nano).</li>
      <li>Apply changes: <code>source /etc/bash.bashrc</code>.</li>
    </ol>
  </li>
</ul>
</blockquote>
</details>

<details>
<summary><b>Q5: How do you check available disk space on a Linux server?</b></summary>
<br>
<blockquote>
<b>Answer:</b>
<ul>
  <li><b>For Filesystems</b>: Use <code>df -h</code> (Disk Free, Human-readable). It displays the total, used, available, and usage percentage of all mounted partitions.
    <ul>
      <li><i>Critical flag</i>: <code>-h</code> shows sizes in GB/MB instead of blocks.</li>
    </ul>
  </li>
  <li><b>For Specific Directories</b>: Use <code>du -sh /path/to/folder</code> (Disk Usage, Summary, Human-readable). Shows the size of a specific folder and its contents.
    <ul>
      <li>To find which folder is consuming the most space in the current directory: <code>du -sh * | sort -h</code>.</li>
    </ul>
  </li>
</ul>
</blockquote>
</details>

<details>
<summary><b>Q6: What does chmod 755 mean? Explain the number logic.</b></summary>
<br>
<blockquote>
<b>Answer:</b>
In Linux, file permissions are divided into 3 sets of users, represented by 3 digits:
<ol>
  <li><b>First Digit (7)</b>: Owner (User who created the file).</li>
  <li><b>Second Digit (5)</b>: Group (Users belonging to the file's group).</li>
  <li><b>Third Digit (5)</b>: Others (Everyone else on the system).</li>
</ol>
Permissions have numeric values:
<ul>
  <li><b>Read (r)</b> = 4</li>
  <li><b>Write (w)</b> = 2</li>
  <li><b>Execute (x)</b> = 1</li>
</ul>
Adding these values creates the permission digits:
<ul>
  <li><code>7</code> = 4 + 2 + 1 $\rightarrow$ <b>Read, Write, and Execute (rwx)</b>.</li>
  <li><code>5</code> = 4 + 0 + 1 $\rightarrow$ <b>Read and Execute (r-x)</b>.</li>
  <li><code>4</code> = 4 + 0 + 0 $\rightarrow$ <b>Read-Only (r--)</b>.</li>
</ul>
Therefore, <b><code>chmod 755 filename</code></b> gives the owner full access (read, write, execute) and allows the group and others to only read and execute the file.
</blockquote>
</details>

<details>
<summary><b>Q7: A server's CPU utilization suddenly spikes to 100%. What are your first 5 troubleshooting steps?</b></summary>
<br>
<blockquote>
<b>Answer:</b>
If the CPU hits 100% capacity, I will isolate and resolve it using these steps:
<ol>
  <li><b>Identify the Offending Process</b>: Run <code>top</code> or <code>htop</code> and press <code>Shift + P</code> (in top) to sort processes by CPU utilization. Identify the process ID (PID) and the command running it.</li>
  <li><b>Analyze the Process</b>: Check if it is a legitimate system application (e.g., an database index run) or a stuck/runaway user script (e.g., an infinite loop in Python).</li>
  <li><b>Investigate Open Threads and Logs</b>: Check what files the process is accessing using <code>lsof -p <PID></code>, and inspect the application logs for infinite loops or massive data-dumping exceptions.</li>
  <li><b>Graceful Terminate</b>: If it is a runaway process that needs to stop, attempt to terminate it gracefully: <code>kill -15 <PID></code> (SIGTERM). This allows the application to save states and close files.</li>
  <li><b>Force Kill (If Unresponsive)</b>: If the process refuses to stop and is endangering server stability, terminate it instantly: <code>kill -9 <PID></code> (SIGKILL). Report the issue to R&D or systems leads with the logged process details.</li>
</ol>
</blockquote>
</details>

---

## 🛠️ Complete Linux Command Catalog

Refer to [Linux Commands Cheat Sheat1.pdf](file:///d:/Temp/grey-orange/Linux%20Commands%20Cheat%20Sheat1.pdf) in the root directory for a full visual index of:
* **Directory Navigation**: `ls -la`, `cd -`, `pwd`.
* **File Operations**: `mkdir -p`, `rm -rf`, `cp -r`, `ln -s`.
* **Compression**: `tar -czvf` (compress), `tar -xzvf` (extract).
* **System Metrics**: `uptime` (load averages), `free -m`, `df -h`.
* **File Transfer**: `scp`, `rsync -avz`.
* **Keyboard Shortcuts**: `Ctrl + C` (kill run), `Ctrl + Z` (suspend), `Ctrl + R` (search command history).
