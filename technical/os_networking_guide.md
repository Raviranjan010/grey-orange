# 🌐 Operating Systems & Networking: Comprehensive Study Guide

Support engineers must diagnose system bottlenecks (memory leaks, CPU spikes) and connectivity errors (blocked ports, DNS resolution failures). This guide provides the critical OS and networking theory along with practical terminal diagnostic tools.

---

## 📋 Confirmed Technical Q&As (Networking & OS)

Please tap on any question below to expand its detailed answer.

<details>
<summary><b>Q1: What is an API? Have you worked with APIs?</b></summary>
<br>
<blockquote>
<b>Answer:</b>
<ul>
  <li><b>What is an API (Application Programming Interface):</b> An API is a set of rules and protocols that allows different software applications to communicate and exchange data with each other. In modern web services, REST APIs are most common, transferring data using HTTP requests and receiving responses in JSON or XML format.</li>
  <li><b>My Experience:</b> Yes, I have worked with APIs. In my B.Tech projects:
    <ul>
      <li>I built and integrated REST APIs using Node.js and Express for a full-stack learning platform.</li>
      <li>I fetched live financial news feeds via public news APIs, parsing the JSON payloads in Python for my financial news sentiment analysis system.</li>
      <li>I integrated APIs for cloud services (like IBM Watsonx and Oracle Cloud) to perform machine learning inferences and cloud hosting checks.</li>
    </ul>
  </li>
</ul>
</blockquote>
</details>

<details>
<summary><b>Q2: What is the difference between TCP and UDP?</b></summary>
<br>
<blockquote>
<b>Answer:</b>
Both are Transport Layer protocols (OSI Layer 4) used to transmit packets across networks, but they have major differences:
<br><br>
<table border="1" cellpadding="5">
  <thead>
    <tr>
      <th>Feature</th>
      <th>TCP (Transmission Control Protocol)</th>
      <th>UDP (User Datagram Protocol)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><b>Connection</b></td>
      <td>Connection-oriented. Requires a 3-way handshake (SYN, SYN-ACK, ACK) before data transfer.</td>
      <td>Connectionless. Sends packets directly without establishing a connection.</td>
    </tr>
    <tr>
      <td><b>Reliability</b></td>
      <td>Reliable. Guarantees packet delivery, packet ordering, and retransmits lost packets.</td>
      <td>Unreliable. Packets may be lost or arrive out of order (fire-and-forget).</td>
    </tr>
    <tr>
      <td><b>Speed</b></td>
      <td>Slower due to handshake, sequence tracking, and flow/congestion control.</td>
      <td>Faster. No overhead or delivery checking.</td>
    </tr>
    <tr>
      <td><b>Header Size</b></td>
      <td>20 bytes.</td>
      <td>8 bytes.</td>
    </tr>
    <tr>
      <td><b>Use Cases</b></td>
      <td>Web browsing (HTTP/S), Email (SMTP), File transfer (FTP), Remote login (SSH).</td>
      <td>Video streaming, online gaming, VoIP, DNS queries.</td>
    </tr>
  </tbody>
</table>
</blockquote>
</details>

<details>
<summary><b>Q3: What does the "Load Average" in the `uptime` command represent?</b></summary>
<br>
<blockquote>
<b>Answer:</b>
Load average represents the average number of processes that are in a runnable or uninterruptible state (waiting for CPU, memory, or disk I/O) over three intervals: the last <b>1 minute</b>, <b>5 minutes</b>, and <b>15 minutes</b>.
<ul>
  <li>On a single-core CPU, a load average of 1.0 means the CPU is at 100% capacity.</li>
  <li>On a quad-core (4 cores) CPU, a load average of 4.0 means the CPU is at 100% capacity.</li>
  <li>If the 1-minute load average is much higher than the 15-minute load average (e.g. 8.5 vs 1.2 on a 4-core machine), it indicates a sudden spike in system utilization that needs immediate checking.</li>
</ul>
</blockquote>
</details>

<details>
<summary><b>Q4: What is a process versus a thread?</b></summary>
<br>
<blockquote>
<b>Answer:</b>
<ul>
  <li><b>Process:</b> A process is an independent program in execution with its own isolated memory address space allocated by the OS. Processes communicate via Inter-Process Communication (IPC) (e.g. sockets or pipes) and have high context-switching overhead. If one process crashes, other processes continue running.</li>
  <li><b>Thread:</b> A thread is a lightweight unit of execution within a parent process. Multiple threads share the same memory space and resources of their parent process. Threads communicate easily by sharing global variables, but have lower overhead. If one thread crashes (e.g. segmentation fault), it can crash the entire process.</li>
</ul>
</blockquote>
</details>

---

## 7️⃣ The OSI Model Reference Chart

*   **Mnemonic to Remember**: **P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way

| Layer Number | Layer Name | Data Unit (PDU) | Key Protocols | Troubleshooting Relevance |
|---|---|---|---|---|
| **Layer 7** | **Application** | Data | HTTP, HTTPS, DNS, SSH, SMTP | Is the app running and producing error pages? |
| **Layer 6** | **Presentation** | Data | SSL, TLS, ASCII, JPEG | Are certificates expired or ciphers mismatching? |
| **Layer 5** | **Session** | Data | RPC, NetBIOS, Sockets | Are active user sessions disconnecting? |
| **Layer 4** | **Transport** | **Segments** (TCP) / Datagrams (UDP) | TCP, UDP | Are ports open? Is the TCP handshake succeeding? |
| **Layer 3** | **Network** | **Packets** | IP (IPv4/IPv6), ICMP | Can we ping the server IP? Is the router routing? |
| **Layer 2** | **Data Link** | **Frames** | Ethernet, MAC, ARP | Is the local network cable/switch working? |
| **Layer 1** | **Physical** | **Bits** | Copper cables, Fiber optics | Is the network card interface powered on? |

---

## 🔒 The SSL/TLS Handshake Protocol (HTTPS Security)

When a client connects securely via HTTPS, a 6-step handshake occurs to establish symmetric encryption:

```
Client                                                  Server
  │                                                       │
  │ ─── 1. Client Hello (TLS Version, Ciphers, Random) ─►│
  │                                                       │
  │ ◄── 2. Server Hello + Server Certificate ─────────────│
  │                                                       │
  │ ─── 3. Client Verifies Certificate with CA ──────────►│
  │                                                       │
  │ ─── 4. Client Sends Encrypted Premaster Secret ──────►│
  │                                                       │
  │ ─── 5. Both Generate Symmetric Session Keys ─────────►│
  │                                                       │
  │ ◄── 6. Finished (Encrypted Session Data begins) ──────│
  ▼                                                       ▼
```

---

## 🔌 Port Number Reference Chart (Must Memorize)

These standard ports are frequently queried in technical screenings:

| Port | Protocol / Service | Description | Support Command to check |
|---|---|---|---|
| **`22`** | **SSH** | Secure Shell (remote administration) | `ssh user@host -p 22` |
| **`23`** | **Telnet** | Unencrypted remote terminal | `telnet host 23` (used for checking port reachability) |
| **`53`** | **DNS** | Domain Name System lookup | `dig @8.8.8.8 domain.com` |
| **`80`** | **HTTP** | Unencrypted web page traffic | `curl -I http://host:80` |
| **`443`** | **HTTPS** | Secure encrypted web page traffic | `curl -I https://host:443` |
| **`3306`** | **MySQL** | Default MySQL database port | `nc -zv host 3306` |
| **`5432`** | **PostgreSQL** | Default PostgreSQL database port | `nc -zv host 5432` |
| **`6379`** | **Redis** | In-memory cache database | `redis-cli -h host -p 6379` |
| **`8080` / `8000`**| **Alternative HTTP** | Commonly used for test and application servers | `curl -I http://host:8080` |

---

## 🌐 DNS Resolution Workflow

When a client queries a domain name, the lookup follows this exact recursive hierarchy:

1.  **Local Cache**: OS checks browser cache and hosts file.
2.  **Recursive Resolver**: Sent to ISP or custom DNS (e.g. `8.8.8.8`).
3.  **Root Server (`.`)**: Returns the IP of the Top-Level Domain (TLD) server (e.g. TLD for `.com`).
4.  **TLD Server (`.com`)**: Returns the IP of the Authoritative Nameserver.
5.  **Authoritative Nameserver**: Returns the actual IP mapping of the domain (e.g., `142.250.190.46`).
6.  **Caching**: Recursive Resolver stores the record based on the TTL (Time-To-Live) and sends the IP back to the client.

---

## 🛠️ Operating System Health Commands

### Checking Memory Leaks (`free -m`)
*   **Total**: Raw physical memory installed.
*   **Used**: Memory currently consumed by processes.
*   **Available**: **Most critical metric**. Displays the actual capacity available to start new applications without swapping.

### Analyzing Disk Capacity (`df -h`)
*   If a filesystem shows **100% Use%**, applications will freeze or throw write errors immediately. Run `du -sh /var/log/* | sort -h` to isolate the directory that is consuming all disk space.

### Process Management
*   `ps aux`: List all running processes.
*   `kill -15 PID`: Gracefully terminate (SIGTERM).
*   `kill -9 PID`: Forcefully terminate immediately (SIGKILL).
*   `lsof -i :port`: Identify which process is holding a networking port open.
*   `uptime`: Review load averages.
