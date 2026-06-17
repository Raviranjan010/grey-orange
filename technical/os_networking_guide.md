# 🌐 Operating Systems & Networking Prep Guide

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

## 🛠️ Network & OS Diagnostic Command Suite

*   **Memory Health**: `free -m` (displays physical and swap RAM in Megabytes).
*   **Disk capacity check**: `df -h` (displays storage metrics on mounted partitions).
*   **Process diagnostics**: `ps aux | grep application_name` (list running instances of the app).
*   **Terminate stuck program**: `kill -9 PID` (forcefully stops runaway tasks).
*   **Check listening ports**: `netstat -tuln` or `ss -tuln` (confirm if services are running on ports).
*   **Verify network reachability**: `ping IP_ADDRESS`.
*   **Route tracking**: `traceroute IP_ADDRESS` (locates routing packet drops).
*   **HTTP endpoint checks**: `curl -I URL` (verifies HTTP response status codes).
