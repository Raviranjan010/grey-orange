# ⚡ MASTER CHEAT SHEET — Last Hour Revision
### GreyOrange Software Support Intern | June 23-24, 2026 Drive

This document contains critical, high-yield summary points to review in the final hours before the hiring rounds. Use the links below to access the complete study guides for detailed concepts.

---

## 📅 Roadmap & Prep Files
* 📅 **[Daily Prep Roadmap](file:///D:/Temp/grey-orange/roadmap/daily_prep_schedule.md)**
* 🐧 **[Linux Prep Guide](file:///D:/Temp/grey-orange/technical/linux_guide.md)**
* 🗄️ **[SQL Query Guide](file:///D:/Temp/grey-orange/technical/sql_guide.md)**
* 🌐 **[OS & Networking Guide](file:///D:/Temp/grey-orange/technical/os_networking_guide.md)**
* 🐍 **[Programming & Scripting Guide](file:///D:/Temp/grey-orange/technical/programming_dsa_guide.md)**
* 🛠️ **[Support & Troubleshooting Scenarios](file:///D:/Temp/grey-orange/interviews/support_troubleshooting.md)**
* 💬 **[GD & HR Interview Guide](file:///D:/Temp/grey-orange/interviews/gd_and_hr.md)**
* 🏢 **[Pre-Placement Talk (PPT) Guide](file:///D:/Temp/grey-orange/interviews/ppt_guide.md)**

---

## 🐧 LINUX (45% weight) — Quick Commands

### ⭐ DCS-Confirmed Core Commands (Memorize!)
* `tail -10 logfile.log`            # Read last 10 lines of a file
* `scp file.txt user@host:/path`    # Copy file to remote server securely (scp [src] [dest])
* `chmod +x script.sh`              # Add execute permissions to script
* `chown user /var/log/app.log`     # Change file ownership (fixes permission errors)
* `free -m`                         # Check RAM usage (look at "available" column, not "free")
* `htop`                            # Interactive process viewer (sort by CPU with Shift+P, F9 to kill)

### Log Investigation Recipes
* `tail -f /var/log/app.log`                      # Follow live logs
* `tail -100 /var/log/app.log | grep ERROR`       # Search last 100 lines for "ERROR"
* `grep -A 20 "Exception" app.log`                # Print match + 20 lines after (for stack trace)
* `grep -rn "timeout" /var/log/`                  # Search recursively for "timeout" showing line numbers
* `sed -n '/14:30:00/,/14:35:00/p' app.log`       # Extract logs between specific times
* `zgrep "ERROR" app.log.1.gz`                    # Search directly inside compressed log file

### File Permissions (4-2-1 Rule)
* **Values**: `Read (r) = 4`, `Write (w) = 2`, `Execute (x) = 1`
* `chmod 755 script.sh`    # Owner rwx (7), Group rx (5), Others rx (5)
* `chmod 644 config.txt`   # Owner rw (6), Group r (4), Others r (4)
* `chmod 600 id_rsa`       # Owner rw (6) only (SSH keys require this!)
* `chmod +x script.sh`     # Make file executable for everyone

### Process & Resource Diagnostics
* `ps aux | grep app`      # Find PID of application processes
* `lsof -i :8080`          # Find process listening on port 8080
* `kill -15 PID`           # Graceful terminate (SIGTERM)
* `kill -9 PID`            # Force terminate immediately (SIGKILL)
* `df -h`                  # Disk usage of mounted filesystems
* `du -sh /var/log/* | sort -rh | head -10`   # List top 10 largest log directories

### Service Management & Cron Jobs
* `sudo systemctl status nginx`     # Check service state
* `sudo systemctl restart nginx`    # Restart service
* `sudo journalctl -u nginx -f`     # Live follow log for specific service
* **Cron format**: `minute hour day_of_month month day_of_week command`
  * `0 * * * *    /script.sh`       # Every hour
  * `0 2 * * *    /script.sh`       # Every day at 2:00 AM
  * `*/15 * * * * /script.sh`       # Every 15 minutes

---

## 🗄️ SQL (35% weight) — Key Concepts

### Logical Query Execution Order
`FROM & JOIN -> WHERE -> GROUP BY -> HAVING -> SELECT -> DISTINCT -> ORDER BY -> LIMIT`
*(Note: You cannot use a SELECT alias in WHERE, but you can in ORDER BY!)*

### JOIN Types
* **INNER JOIN**: Only returns records that match in both tables.
* **LEFT JOIN**: Returns all records from the left table, plus matched records from the right table.
* **RIGHT JOIN**: Returns all records from the right table, plus matched records from the left table.
* **FULL OUTER JOIN**: Returns all records from both tables, filling unmatched columns with `NULL`.

### Must-Know SQL Queries
* **2nd Highest Salary**:
  ```sql
  SELECT MAX(salary) FROM employees WHERE salary < (SELECT MAX(salary) FROM employees);
  ```
* **Employees Earning More than Manager**:
  ```sql
  SELECT e.name FROM employees e JOIN employees m ON e.manager_id = m.id WHERE e.salary > m.salary;
  ```
* **Departments with No Employees**:
  ```sql
  SELECT d.department_name FROM departments d LEFT JOIN employees e ON d.id = e.dept_id WHERE e.id IS NULL;
  ```
* **Find Duplicates**:
  ```sql
  SELECT name, COUNT(*) FROM employees GROUP BY name HAVING COUNT(*) > 1;
  ```
* **Salary Rank Per Department (Window Function)**:
  ```sql
  SELECT name, RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS rnk FROM employees;
  ```

---

## 🌐 OS & NETWORKING (5-10% weight)

### OSI Layers (Top to Bottom)
1. **Application**: HTTP, HTTPS, DNS, SSH, SMTP (App actions)
2. **Presentation**: SSL, TLS, ASCII (Data formatting & encryption)
3. **Session**: Sockets, RPC (Establish/maintain sessions)
4. **Transport**: TCP (Reliable, connection), UDP (Fast, connectionless)
5. **Network**: IP, ICMP (Routing & packets - ping)
6. **Data Link**: Ethernet, MAC, ARP (Physical addressing & frames)
7. **Physical**: Cables, WiFi (Bits transmission)

### TCP vs UDP
* **TCP**: Reliable, ordered, slower, 3-way handshake (`SYN` -> `SYN-ACK` -> `ACK`), connection-oriented. Uses: HTTP, SSH, SQL.
* **UDP**: Unreliable, unordered, fast, connectionless. Uses: Streaming, VoIP, DNS, Gaming.

### Standard Ports
* `22` = SSH | `53` = DNS | `80` = HTTP | `443` = HTTPS | `3306` = MySQL | `5432` = PostgreSQL | `6379` = Redis

### Network Diagnostics Commands
* `ping 8.8.8.8`            # Test network layer reachability
* `traceroute 8.8.8.8`      # Find hops where packet connection drops
* `nc -zv host 3306`        # Test if port 3306 is open (Netcat)
* `curl -I http://host`     # Retrieve HTTP response header
* `dig greyorange.com`      # DNS lookup information
* `ss -tulnp`               # List all listening ports and PIDs

---

## 🐍 PYTHON (5% weight)

### Exception Handling & File I/O
Always read files line-by-line using a generator to avoid loading gigabytes of data into RAM (Out-Of-Memory crash):
```python
try:
    with open("app.log", "r") as f:
        for line in f:      # Memory-safe line-by-line reading
            if "ERROR" in line:
                print(line.strip())
except FileNotFoundError:
    print("Log file not found")
except PermissionError:
    print("Elevated permission required")
finally:
    print("Scan completed")
```

### Time Complexities
* List Access by Index: $O(1)$ | List Search: $O(n)$
* Dict / Set Lookup: $O(1)$ | Binary Search: $O(\log n)$
* Bubble Sort: $O(n^2)$ | Merge / Quick Sort: $O(n \log n)$

---

## 🤖 GREYORANGE COMPANY FACTS
* **Founders**: Samay Kohli + Akash Gupta (Founded in 2011)
* **HQ**: Atlanta, Georgia, USA | **India Hub**: Gurugram, Haryana
* **Core Products**:
  * **AMR Ranger Series** (Autonomous Mobile Robots): Navigates dynamically using LiDAR, cameras, and onboard AI. (AGVs follow fixed magnetic floor tape/wires).
  * **Butler System (G2P)**: Goods-to-Person solution. Robots carry inventory storage PODs to pickers, saving walking time.
  * **GreyMatter**: The AI software platform coordinating robots, predicting order volumes, and optimizing warehouse layouts in real-time.
* **Support Tiers**: L1 (first contact, triage) -> L2 (technical) -> L3 (advanced engineers) -> L4/R&D (bug fixes/code patches).

---

## ⏰ SLA PRIORITY MATRIX

| Priority | System Impact | Response SLA | Resolution SLA | Example Scenario |
|---|---|---|---|---|
| **P1 Critical** | Entire system down | **15 minutes** | **2 hours** | All robots frozen, no picking possible |
| **P2 Major** | Core feature broken / slow | **30 minutes** | **4 hours** | Sorting belt running 60% slower |
| **P3 Minor** | Single user issue, workaround exists | **4 hours** | **24 hours** | Scanner formatting bug on screen |
| **P4 Low** | Info / Configuration request | **12 hours** | **5 days** | Admin requesting a backup report |

---

## 🎒 PHYSICAL CHECKLIST (June 23 — Day 1)

### Required Documents
* [ ] 2x Resumes (Printed, ATS-optimized, matching LPU layout)
* [ ] 2x Passport Photos (Recent, formal background)
* [ ] All Academic Marksheets (10th, 12th, and all B.Tech semester transcripts)
* [ ] LPU College ID Card + Aadhaar / PAN card

### Dress Code (Strictly Formal)
* **Gentlemen**: Light-colored formal shirt (white/light blue) + dark trousers (charcoal/navy/black) + dark leather belt + formal leather shoes (no sneakers/jeans). Tie recommended. Neatly groomed or clean-shaven.
* **Ladies**: Formal shirt/top with trousers, or formal Indian suit. Neat hair and formal shoes/flats.

### Day 1 Mindset
* **Pre-Placement Talk (PPT)**: Take active notes (stats, clients, product names).
* **Group Discussion (GD)**: Speak early (among the first 2-3 speakers) using the **PREP method** (Point -> Reason -> Example -> Conclude).
* **Support Perspective**: Think logically, structurally, and prioritize client communication.
