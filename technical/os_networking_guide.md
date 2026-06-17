# 🌐 Operating Systems & Networking Prep Guide

Support engineers must diagnose system bottle-necks (memory leaks, CPU spikes) and connectivity errors (blocked ports, DNS resolution failures). This guide provides the critical OS and networking theory along with practical terminal diagnostic tools.

---

## 💻 Part 1: Operating Systems (OS) Essentials

### 1. Process vs Thread
A **Process** is an executing instance of an application, while a **Thread** is a path of execution within a process.

| Metric | Process | Thread |
|---|---|---|
| **Memory** | Has its own isolated memory space. | Shares memory and resources of the parent process. |
| **Overhead** | High creation and context switching overhead. | Low overhead (lightweight). |
| **Isolation** | If one process crashes, others are unaffected. | If a thread crashes (segmentation fault), it can crash the whole process. |
| **Communication**| Uses Inter-Process Communication (IPC) (pipes, sockets, shared memory). | Communicates easily by reading/writing shared memory variables. |

### 2. Deadlock & The 4 Coffman Conditions
A **Deadlock** is a state where a set of processes are blocked because each process is holding a resource and waiting for another resource held by some other process.
For a deadlock to occur, **all four** of the following conditions must hold:
1. **Mutual Exclusion**: At least one resource must be held in a non-shareable mode (only one process can use it at a time).
2. **Hold and Wait**: A process must be holding at least one resource and waiting to acquire additional resources held by other processes.
3. **No Preemption**: Resources cannot be forcibly taken from a process; they can only be released voluntarily.
4. **Circular Wait**: A closed chain of processes exists, where each process holds resources needed by the next process in the chain.
* **Resolution**: Detect and terminate one of the deadlocked processes, or preempt resources to break the loop.

### 3. CPU Scheduling Algorithms
* **FCFS (First-Come, First-Served)**: Simple FIFO queue. Can lead to "convoy effect" where short jobs wait behind long jobs.
* **SJF (Shortest Job First)**: Picks the job with the shortest execution time. Provides optimal average waiting time but can cause starvation of long jobs.
* **Round Robin (RR)**: Each process gets a small unit of CPU time (time quantum), then is preempted and put at the end of the queue. Great for time-sharing systems.
* **Priority Scheduling**: CPU is allocated to the process with the highest priority. Can lead to starvation (solved by "aging" where priority increases over time).

### 4. Memory Management & Troubleshooting Commands
* **Virtual Memory**: A technique that allows the execution of processes that are not completely in physical memory, using disk storage (Swap space) as extension RAM.
* **Paging**: Division of physical memory into fixed-size blocks (frames) and logical memory into blocks of the same size (pages).
* **Thrashing**: A state where the OS spends more time swapping pages in and out of Swap space than executing actual instructions, causing system performance to drop to zero.

#### Diagnostic OS Commands (Must Memorize):
* **Memory Check**: `free -m` (displays physical and swap memory in MB). Look at the `available` column.
* **Disk Check**: `df -h` (displays free disk space on mounted drives).
* **Disk Space per folder**: `du -sh /var/log/` (disk usage summary of directory `/var/log`).
* **Active Processes**: `ps aux` (snapshot of all running processes).
  * Check for process sorting: `ps aux --sort=-%cpu | head -5` (shows top 5 CPU-consuming processes).
* **Kill Process**: `kill -9 <PID>` (sends SIGKILL to immediately terminate a stuck process).
* **Open Files**: `lsof -i :8080` (lists which process is listening on network port 8080).

---

## 🌐 Part 2: Computer Networking Essentials

### 1. OSI Model vs TCP/IP Suite
The **OSI Model** is a 7-layer theoretical framework. The **TCP/IP Model** is a 4-layer practical framework actually used on the internet.

| Layer | OSI Layer Name | Primary Function | Protocols / Hardware |
|---|---|---|---|
| **7** | Application | Network access for applications | HTTP, HTTPS, DNS, FTP, SSH, SMTP |
| **6** | Presentation | Data formatting, encryption, compression | SSL, TLS, ASCII, JPEG |
| **5** | Session | Manages connections (sessions) | NetBIOS, RPC |
| **4** | Transport | End-to-end reliability, port mapping | **TCP, UDP** |
| **3** | Network | Routing and packet addressing | **IP**, ICMP, Routers |
| **2** | Data Link | Physical addressing (MAC), error checking | Ethernet, Switches |
| **1** | Physical | Binary transmission over media | Cables, Hubs, Fiber optics |

* **TCP/IP Mapping**: 
  * OSI 5, 6, 7 $\rightarrow$ TCP/IP **Application Layer**
  * OSI 4 $\rightarrow$ TCP/IP **Transport Layer**
  * OSI 3 $\rightarrow$ TCP/IP **Internet Layer**
  * OSI 1, 2 $\rightarrow$ TCP/IP **Network Access Layer**

### 2. TCP vs UDP
* **TCP (Transmission Control Protocol)**:
  * **Connection-Oriented**: Requires a 3-way handshake to establish a connection (`SYN` $\rightarrow$ `SYN-ACK` $\rightarrow$ `ACK`) before sending data.
  * **Reliable**: Guarantees delivery using sequence numbers and retransmissions.
  * **Flow & Congestion Control**: Slows down transmission speed if the receiver or network is overwhelmed.
  * **Use Cases**: Web browsing (HTTP/S), Email (SMTP), File Transfer (FTP), Remote Login (SSH).
* **UDP (User Datagram Protocol)**:
  * **Connectionless**: Sends packets (datagrams) directly without establishing a connection.
  * **Unreliable**: No delivery guarantees, no sequence numbers, no retransmissions (fire-and-forget).
  * **Fast**: No connection overhead or flow control.
  * **Use Cases**: Video streaming, Online gaming, VoIP (voice calls), DNS queries.

### 3. How DNS (Domain Name System) Works
DNS acts as the phonebook of the internet, translating human-readable domain names (like `google.com`) into computer-readable IP addresses (like `142.250.190.46`).
**Steps in DNS Resolution**:
1. You type `example.com` in your browser.
2. The browser checks its **local cache** (and `hosts` file). If not found, it queries the **Recursive Resolver** (usually your ISP).
3. The Resolver queries the **Root Nameserver** (`.`), which points to the **TLD (Top-Level Domain) Nameserver** (like `.com`).
4. The Resolver queries the **TLD Nameserver**, which points to the domain's **Authoritative Nameserver**.
5. The Resolver queries the **Authoritative Nameserver** and retrieves the IP address, caching it, and returning it to your browser.

### 4. HTTP vs HTTPS
* **HTTP (Hypertext Transfer Protocol)**: Operates on **Port 80**. Sends data in plain text. If intercepted (Man-in-the-Middle attack), data is fully readable.
* **HTTPS**: Operates on **Port 443**. Wraps HTTP inside an **SSL/TLS encrypted layer**. Ensures:
  1. *Encryption*: Data cannot be read by interceptors.
  2. *Integrity*: Data cannot be modified during transit.
  3. *Authentication*: Verifies that you are communicating with the actual owner of the domain (via certificates).

---

## 🛠️ Network Diagnostic CLI Tools (Troubleshooting Cheat Sheet)

### 1. Reachability
* **Command**: `ping <ip_or_domain>`
* **Use Case**: Check if a server is online and responding. If it succeeds, basic network connectivity is active. If it fails, either the server is down, there is a routing failure, or ICMP packets are blocked by a firewall.

### 2. Route Pathing
* **Command**: `traceroute <ip_or_domain>` (Linux) / `tracert <ip_or_domain>` (Windows)
* **Use Case**: Lists each router hop the packet takes to reach its destination. Useful for identifying *where* a packet is dropping or which network hop is adding latency.

### 3. Active Connections & Listening Ports
* **Command**: `netstat -tuln` or `ss -tuln`
* **Flags**: `-t` (TCP), `-u` (UDP), `-l` (listening), `-n` (numeric ports).
* **Use Case**: Verify if the application server is actually running and listening on its designated port. For example, if you deploy an API on port 8080, run `netstat -tuln | grep 8080` to check if it's active.

### 4. Interface Configuration
* **Command**: `ip addr show` or `ifconfig`
* **Use Case**: Find your local IP address, MAC address, and check if your network interfaces (like `eth0` or `wlan0`) are up and configured.

### 5. DNS Queries
* **Command**: `dig <domain>` or `nslookup <domain>`
* **Use Case**: Fetch DNS records for a domain. If a customer says "your app is down," but `ping 8.8.8.8` works and `ping myapp.com` fails, run `nslookup myapp.com` to see if the DNS server is resolving the domain name.

### 6. HTTP API Testing
* **Command**: `curl -I https://google.com`
* **Use Case**: Fetch only the headers of an HTTP response. Great for checking API status codes (`200 OK`, `404 Not Found`, `500 Internal Server Error`) directly from the command line.
