# 🌐 Operating Systems & Networking — COMPLETE Study Guide
### GreyOrange Software Support Engineer Intern | Drive: June 23-24, 2026

> **Weight in Interview**: 5-10% | **Priority**: MEDIUM  
> As a support engineer, you will resolve system bottlenecks (CPU spikes, memory exhaustion) and trace connectivity errors (blocked network ports, DNS lookup failures, SSL certificate issues).

---

## PART 1: The 7 OSI Model Layers
##
To remember the layers from top to bottom (7 down to 1), use this mnemonic:  
**P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way (*Physical, Data Link, Network, Transport, Session, Presentation, Application*).

| Layer Number | Layer Name | Protocol Data Unit (PDU) | Key Protocols | Troubleshooting Relevance & Examples |
|---|---|---|---|---|
| **Layer 7** | **Application** | Data | HTTP, HTTPS, DNS, SSH, SMTP, FTP | Is the web server active? Are requests returning application error pages? |
| **Layer 6** | **Presentation**| Data | SSL, TLS, ASCII, JPEG, MPEG | Are SSL/TLS certificates expired or is there a cipher suite mismatch? |
| **Layer 5** | **Session** | Data | NetBIOS, RPC, Sockets | Are active client-robot sockets terminating prematurely? |
| **Layer 4** | **Transport** | Segments (TCP) / Datagrams (UDP) | TCP, UDP | Are target ports open? Is the TCP 3-way handshake completing successfully? |
| **Layer 3** | **Network** | Packets | IP (IPv4/IPv6), ICMP, ARP | Can we ping the server IP address? Is the router routing packets across subnets? |
| **Layer 2** | **Data Link** | Frames | Ethernet, MAC, Switches | Is the local network card working? Is the Ethernet cable plugged into the switch? |
| **Layer 1** | **Physical** | Bits | Fiber optics, Cables, Hubs, WiFi | Is the physical link active? Are network lights flashing on the NIC? |

---

## PART 2: TCP vs UDP (Transport Layer Protocols)

| Feature | TCP (Transmission Control Protocol) | UDP (User Datagram Protocol) |
|---|---|---|
| **Connection** | Connection-oriented. Requires a 3-way handshake before sending data. | Connectionless. Sends packets directly without establishing a connection. |
| **Reliability** | Reliable. Guarantees packet delivery, packet order, and retransmits lost packets. | Unreliable. Packets may be lost or arrive out of order (fire-and-forget). |
| **Speed** | Slower due to handshake, sequence tracking, and congestion control. | Faster. Minimal overhead, no delivery checking. |
| **Header Size** | 20 bytes. | 8 bytes. |
| **Data Stream** | Byte stream (keeps track of boundaries). | Datagram packet boundaries are preserved. |
| **Use Cases** | Web browsing (HTTP/S), remote administration (SSH), databases, email. | Live video streaming, VoIP, online gaming, DNS queries. |

### The TCP 3-Way Handshake
1. **`SYN`**: Client sends a Synchronize packet with a random sequence number (e.g. `X`) to the server.
2. **`SYN-ACK`**: Server acknowledges by sending an ACK packet (`X + 1`) and its own random sequence number `Y`.
3. **`ACK`**: Client sends an Acknowledge packet (`Y + 1`) back to the server. Connection is established.

---

## PART 3: Key Port Numbers

Memorize these standard ports. In interviews, you may be asked how to check if they are listening.

| Port | Service / Protocol | Description | Troubleshooting Check |
|---|---|---|---|
| **`22`** | **SSH** | Secure Shell (remote administration) | `nc -zv host 22` |
| **`53`** | **DNS** | Domain Name System (names to IPs) | `dig @8.8.8.8 domain.com` |
| **`80`** | **HTTP** | Unencrypted web page traffic | `curl -I http://host:80` |
| **`443`** | **HTTPS** | Encrypted web page traffic | `curl -I https://host:443` |
| **`3306`**| **MySQL** | Default MySQL database port | `nc -zv host 3306` |
| **`5432`**| **PostgreSQL**| Default PostgreSQL database port | `nc -zv host 5432` |
| **`6379`**| **Redis** | In-memory cache database | `redis-cli -h host -p 6379` |

---

## PART 4: Network Diagnostics Toolset

* `ping 8.8.8.8`            # Test connectivity at the Network layer (uses ICMP Echo Requests).
* `traceroute 8.8.8.8`      # Identifies routing hops and where packets drop (uses TTL increments).
* `nc -zv host 3306`        # Netcat port scanner. Checks if a TCP port is open (z = zero I/O, v = verbose).
* `curl -I http://host`     # Fetches HTTP headers to check status codes and response headers without downloading content.
* `dig greyorange.com`      # DNS lookup tool. Returns A, MX, NS, CNAME records.
* `ss -tulnp`               # Lists listening ports (t=TCP, u=UDP, l=listening, n=numeric ports, p=processes/PIDs).

---

## PART 5: HTTP/API Status Codes

API status codes indicate the result of a server request. Memorize these ranges and codes:

### Status Code Categories
* **`2xx` (Success)**: The request was successfully received, understood, and accepted.
* **`3xx` (Redirection)**: Further action needs to be taken by the client to complete the request.
* **`4xx` (Client Error)**: The request contains bad syntax or cannot be fulfilled by the client.
* **`5xx` (Server Error)**: The server failed to fulfill an apparently valid request.

### Core Status Codes
* **`200 OK`**: Request succeeded. The payload is returned in the response.
* **`201 Created`**: Request succeeded and a new resource was created (typically after a `POST` request).
* **`400 Bad Request`**: Server cannot process request due to client error (e.g. malformed JSON body).
* **`401 Unauthorized`**: Client must authenticate itself to get the requested response (missing or invalid token).
* **`403 Forbidden`**: Client does not have access rights to the content (authenticated but not authorized).
* **`404 Not Found`**: The server cannot find the requested resource.
* **`500 Internal Server Error`**: The server encountered a condition it does not know how to handle (unhandled exception in code).
* **`503 Service Unavailable`**: The server is not ready to handle the request (common during maintenance or system overload).

---

## PART 6: The DNS Resolution Workflow

When you type `greyorange.com` in a browser, the following steps occur to resolve the name to an IP:

```
[Browser] ──► 1. Checks Local Cache (Browser/OS hosts file)
    │
    ├──(Miss)──► 2. Queries ISP Recursive Resolver (e.g. 8.8.8.8)
                      │
                      ├──► 3. Queries Root Nameserver (.) ──► Returns TLD Server IP
                      │
                      ├──► 4. Queries TLD Nameserver (.com) ──► Returns Auth Nameserver IP
                      │
                      └──► 5. Queries Authoritative Nameserver ──► Returns IP address (e.g. 192.0.2.1)
```

---

## PART 7: The SSL/TLS Handshake Protocol

For secure HTTPS connections, the client and server negotiate keys using this 6-step handshake:

1. **Client Hello**: Client sends supported TLS version, cipher suites list, and a random string.
2. **Server Hello**: Server responds with the chosen TLS version, cipher suite, random string, and its **SSL Certificate** (containing public key).
3. **Certificate Verification**: Client validates the certificate against its list of trusted Certificate Authorities (CAs).
4. **Premaster Secret**: Client generates a random premaster secret, encrypts it using the server's public key, and sends it to the server.
5. **Key Generation**: Server decrypts the secret using its private key. Both parties generate symmetric session keys using the random values.
6. **Finished**: Both exchange encrypted messages confirming handshake completion. Further communication is encrypted using the symmetric keys.

---

## PART 8: OS & Networking Interview Q&As

<details>
<summary><b>Q1: What is an API? Have you worked with APIs?</b></summary>
<br>
<blockquote>
<b>Answer:</b> An API (Application Programming Interface) is a set of rules allowing different systems to communicate. Yes, I built REST APIs using Express/PostgreSQL for my full-stack Devliq platform, and consumed FastAPI backends combined with public financial APIs for parsing JSON response streams.
</blockquote>
</details>

<details>
<summary><b>Q2: What is a process vs a thread?</b></summary>
<br>
<blockquote>
<b>Answer:</b> A process is an independent program in execution with its own isolated memory address space allocated by the OS. Processes communicate via IPC. A thread is a lightweight execution unit inside a process. Threads share the parent process's memory space, which makes communication fast but means a crash in one thread can terminate the entire process.
</blockquote>
</details>

<details>
<summary><b>Q3: What does the "Load Average" in the uptime command represent?</b></summary>
<br>
<blockquote>
<b>Answer:</b> It represents the average number of active processes (waiting for CPU or disk I/O) over the last 1, 5, and 15 minutes. A load of 4.0 on a 4-core machine represents 100% system utilization.
</blockquote>
</details>

<details>
<summary><b>Q4: What is the difference between ping and traceroute?</b></summary>
<br>
<blockquote>
<b>Answer:</b> `ping` uses ICMP Echo Request packets to test if a target IP is reachable and measures round-trip time. `traceroute` uses packets with incrementing TTL (Time-To-Live) values to map the exact route path and identify which specific router hop is failing or causing latency.
</blockquote>
</details>
