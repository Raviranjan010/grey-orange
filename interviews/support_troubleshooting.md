# 🛠️ Support & Troubleshooting — COMPLETE Guide
### GreyOrange Software Support Engineer Intern | Drive: June 23-24, 2026

Production environments are high-stakes. Outages in warehouse systems block operations and result in direct financial losses. This guide outlines the diagnostic mindset, SLA lifecycle, and communication standards expected of a GreyOrange Software Support Engineer.

---

# PART 1: 5 Critical Troubleshooting Scenarios

These scenarios represent common issues support engineers troubleshoot. Memorize the step-by-step protocols:

### Scenario 1: "Your application is not working" — 5-Step Protocol
When a warehouse manager reports an application freeze or crash, follow this flow:
1. **GATHER**: Ask clarifying questions. Which module is failing? Since when? How many users are affected? Were there any recent software deployments or configuration updates?
2. **LOGS**: Search for errors and stack traces in the application logs:
   ```bash
   tail -100 /var/log/app.log | grep ERROR
   ```
3. **HEALTH**: Check server hardware resources to identify constraints:
   * CPU: `htop` / `top`
   * RAM: `free -m` (check the "available" column)
   * Disk: `df -h`
4. **DB**: Verify database connectivity by running a basic sanity query:
   ```sql
   SELECT 1;
   ```
5. **COMMUNICATE & ESCALATE**: Acknowledge the ticket. Update the client every 15 minutes. If unresolved within the target SLA limit, escalate to the L3/R&D engineering team with your gathered logs.

---

### Scenario 2: Database Running Slow — Investigation Flow
If queries are taking a long time to return results, run these checks:
1. **Slow Query Log**: Check the database logs for queries taking longer than 2 seconds to execute.
2. **Explain Plan**: Analyze the query's execution plan to check if it's scanning the whole table sequentially instead of using an index:
   ```sql
   EXPLAIN ANALYZE SELECT ...;
   ```
3. **Server Resources**: Check CPU usage and disk I/O bottlenecks on the database node:
   ```bash
   htop
   iostat -xz 1 5
   ```
4. **Locks**: Search for database locks or blocked transactions:
   * *PostgreSQL query*:
     ```sql
     SELECT pid, query, state, age(clock_timestamp(), query_start) 
     FROM pg_stat_activity 
     WHERE state != 'idle' ORDER BY age DESC;
     ```
5. **Action**: Report findings to R&D/DBA. Suggest adding index keys or optimizing the query structure.

---

### Scenario 3: CPU at 100% — Resolution Flow
If a server becomes slow and resource metrics show CPU utilization at 100%:
1. **Identify PID**: Open `htop` and sort by CPU usage (`Shift+P` in top). Note the Process ID (PID) of the offending process.
2. **Analyze**: Check the process command name. Is it a legitimate system application (e.g. database reindex) or a runaway process (e.g. infinite loop)?
3. **Inspect Active Descriptors**: Check what files or network ports the process is holding open:
   ```bash
   lsof -p PID
   ```
4. **Inspect Logs**: Check the application logs for a tight loop throwing continuous exceptions.
5. **Terminate**: 
   * Try terminating gracefully: `kill -15 PID` (SIGTERM).
   * If it remains unresponsive, force kill: `kill -9 PID` (SIGKILL).
6. **Report**: Save the diagnostic dump and report the bug to R&D.

---

### Scenario 4: Disk 100% Full — Emergency Response
When a server disk is full, the database and applications will fail to write data and freeze.
1. **Confirm Partition**: Check which mount partition is exhausted:
   ```bash
   df -h
   ```
2. **Locate Large Folders**: Find the biggest files or directories in the log paths:
   ```bash
   du -sh /var/log/* | sort -rh | head -10
   ```
3. **Compress Old Logs**: Free up space immediately by compressing old rotated text logs:
   ```bash
   gzip /var/log/app.log.1
   ```
4. **Clean `/tmp`**: Delete old temporary files older than 7 days:
   ```bash
   find /tmp -type f -mtime +7 -delete
   ```
5. **Alerting**: Set up disk space alerts: *"Disk usage at 92% on server grey-prod-01. Log directories must be archived."*

---

### Scenario 5: Server Not Responding — Network Diagnosis
When you cannot connect to a remote client installation or backend server:
1. **Ping**: Test if the host is reachable at the network layer:
   ```bash
   ping server-ip
   ```
2. **Trace Route**: If ping fails, run a traceroute to find where the packets are dropping:
   ```bash
   traceroute server-ip
   ```
3. **Port Check**: Verify if the SSH daemon (port 22) or web port is open:
   ```bash
   nc -zv server-ip 22
   nc -zv server-ip 443
   ```
4. **SSH Access**: If reachable, SSH into the machine and check system logs for kernel crashes or Out-Of-Memory terminations:
   ```bash
   dmesg | grep -i oom
   ```
5. **Service Check**: Verify if the application daemon is active:
   ```bash
   sudo systemctl status service-name
   ```
6. **Restart**: Restart the service if it has stopped:
   ```bash
   sudo systemctl restart service-name
   ```

---

# PART 2: SLA & Incident Management

Support engineers operate within strict timelines defined by **Service Level Agreements (SLAs)**.

* **Response SLA**: The maximum time allowed to officially acknowledge a ticket and start working.
* **Resolution SLA**: The maximum time allowed to resolve the issue or provide a temporary workaround.

## SLA Priority Matrix

| Priority | Severity | Response SLA | Resolution SLA | Example Scenario |
|---|---|---|---|---|
| **P1 Critical** | Entire system down | **15 minutes** | **2 hours** | All robots frozen on the warehouse floor; dispatch screen is unresponsive. |
| **P2 Major** | Core feature broken / slow | **30 minutes** | **4 hours** | Sorting belt throughput drops by 60%, slowing down peak fulfillment. |
| **P3 Minor** | Single terminal issue | **4 hours** | **24 hours** | Handheld scanner displays a layout formatting bug, but scanning works. |
| **P4 Low** | Info / Config request | **12 hours** | **5 days** | Warehouse administrator requests a database backup report from last week. |

---

# PART 3: Client Email Communication Templates

During P1 outages, clear and timely updates are critical to manage client expectations.

### Template 1: P1 Acknowledgment (Send within 15 mins of incident)
```text
Subject: [P1 INCIDENT #88201] Warehouse Dispatch Screen Freeze — Under Investigation

Dear Operations Team,

We have received your report regarding the freeze on the inventory dispatch screen at the Chicago-02 location (Ticket #88201). This incident has been classified as P1 Critical.

Our team has initiated investigation of server resources and database transaction logs. 

We will provide status updates every 15 minutes until operations are restored.

Regards,
Ravi
Software Support Engineer, GreyOrange
```

### Template 2: Mid-Investigation Status Update
```text
Subject: [UPDATE 1 - #88201] Dispatch Screen Freeze — Root Cause Identified

Dear Operations Team,

Status: Root cause identified. We observed database connection pool exhaustion causing API timeouts.
Action: We are terminating idle database sessions and restarting the dispatcher service.
ETA: Recovery is expected within 10 minutes.

We will send another update in 15 minutes or immediately upon service recovery.

Regards,
Ravi
Software Support Engineer, GreyOrange
```

### Template 3: Resolution & Verification Request
```text
Subject: [RESOLVED - #88201] Dispatch Screen Freeze — Service Restored

Dear Operations Team,

Ticket #88201 is now resolved. The database connection pool has normalized (15% utilization).

Our internal tests confirm that the dispatch screen is responsive, and API requests are completing in under 200ms. 

Please verify on your end and let us know if we have your permission to close this ticket. 

Regards,
Ravi
Software Support Engineer, GreyOrange
```

---

# PART 4: Production Log Anatomy

Here are two examples of typical production log snippets and how to diagnose them:

### Log Example 1: Connection Pool Exhaustion
```text
2026-06-17 14:02:15.302 [ERROR] org.hibernate.engine.jdbc.connections.internal.BasicConnectionCreator - Connection pool exhausted. Active connections: 100/100. Timeout waiting for connection (5000ms).
2026-06-17 14:02:15.305 [ERROR] com.greyorange.wms.dispatch.OrderService - Failed to retrieve dispatch queue.
org.postgresql.util.PSQLException: FATAL: remaining connection slots are reserved for non-replication superuser connections
```
* **Diagnosis**: The application server has hit its database connection limit. Every database slot is taken. New web requests are timing out waiting for a connection, throwing errors.
* **Resolution**: Increase the max connections in database configuration (`postgresql.conf`), or audit the application code to ensure connections are being closed in a `finally` block.

### Log Example 2: Disk Space Full
```text
2026-06-17 14:05:10.119 [WARN]  org.elasticsearch.cluster.routing.allocation.decider.DiskThresholdDecider - flood stage disk watermark [95%] exceeded on node [grey-node-01], all indices are marked as read-only.
2026-06-17 14:05:10.125 [ERROR] com.greyorange.wms.search.InventorySearch - Failed to index inventory item ID 99201.
java.io.IOException: No space left on device
```
* **Diagnosis**: The server's disk space utilization has exceeded the critical 95% threshold. ElasticSearch has automatically locked the databases to read-only mode to prevent corruption.
* **Resolution**: Free up disk space immediately using `rm -rf` on old log archives in `/var/log`, or compress files using `gzip`. Once space is freed, unlock the read-only index state.
