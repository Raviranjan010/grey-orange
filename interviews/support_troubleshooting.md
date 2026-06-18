# 🛠️ Software Support & Troubleshooting: Comprehensive Guide

This guide details the structural and analytical mindset required for a Software Support role. GreyOrange will assess how you handle critical production incidents under pressure and how you communicate with clients.

---

## 📋 Situation-Based Scenarios (Critical for Interviews)

Please tap on any scenario below to view the exact 5-step investigation protocol.

<details>
<summary><b>Scenario 1: Customer says: "Your application is not working." What do you do?</b></summary>
<br>
<blockquote>
When a customer reports an application failure, I follow this 5-step protocol:
<ol>
  <li><b>Gather details:</b> Identify which module is failing, since when the issue started, how many users are affected, and whether any recent configurations or changes were deployed.</li>
  <li><b>Check application logs:</b> Inspect log entries in real-time or search history using command lines like:
    <pre><code>tail -100 app.log | grep ERROR</code></pre>
  </li>
  <li><b>Verify server health:</b> Check hardware and system metrics on the host:
    <ul>
      <li>CPU usage: <code>top</code> / <code>htop</code></li>
      <li>Memory utilization: <code>free -m</code></li>
      <li>Disk space: <code>df -h</code></li>
    </ul>
  </li>
  <li><b>Check database connectivity:</b> Execute a quick and simple query to verify connection to the database:
    <pre><code>SELECT 1;</code></pre>
  </li>
  <li><b>Maintain communication & escalate:</b> Update the customer every 15 minutes even if there is no fix yet. If the issue remains unresolved within the target SLA, escalate to L4 engineering.</li>
</ol>
</blockquote>
</details>
//

<details>
<summary><b>Scenario 2: Database is running slow. How will you investigate?</b></summary>
<br>
<blockquote>
If database latency is high and lagging response times, I follow these 5 steps:
<ol>
  <li><b>Run slow query log analysis:</b> Check the database logs to identify queries taking more than 2 seconds to execute.</li>
  <li><b>Examine query plan:</b> Use the explain command to check if indexes are being used or if the database is running sequential table scans:
    <pre><code>EXPLAIN SELECT ...;</code></pre>
  </li>
  <li><b>Check server resources:</b> Inspect CPU, RAM, and disk read/write load on the database server using commands like:
    <pre><code>htop</code>
<code>iostat</code></pre>
  </li>
  <li><b>Inspect locks & transactions:</b> Look for active table locks, deadlocks, or long-running database transactions blocking other queries.</li>
  <li><b>Report & recommend:</b> Put together a full diagnostics report and recommend adding necessary indexes or optimizing queries to the R&D/engineering team.</li>
</ol>
</blockquote>
</details>

---

## 🎟️ Incident Ticketing & SLA Lifecycle (Production SRE Model)

Enterprise software support operates within strict timelines called **Service Level Agreements (SLAs)**, tracking tickets in platforms like **Jira Service Desk** or **ServiceNow**.

### SLA Metric Definitions:
*   **Response SLA**: The maximum time allowed to officially acknowledge a ticket and start working on it.
*   **Resolution SLA**: The maximum time allowed to resolve the issue or provide a temporary workaround.

| Priority | Severity | System Impact | Response SLA | Resolution SLA | Example Scenario |
|---|---|---|---|---|---|
| **P1** | **Critical** | Entire warehouse system is down. Operations completely stopped. | **15 Mins** | **2 Hours** | Robots are completely frozen and cannot receive dispatch paths. |
| **P2** | **Major** | Core feature is failing or extremely slow, but system runs. | **30 Mins** | **4 Hours** | The sorting belt is functioning, but processing speed has dropped by 60%. |
| **P3** | **Minor** | Single user/terminal issue. Workaround exists. | **4 Hours** | **24 Hours** | A handheld scanner is showing a layout formatting bug on its screen. |
| **P4** | **Low** | Request for information, copy changes, minor config. | **12 Hours** | **5 Days** | The warehouse admin wants a backup of last week's non-critical report. |

---

## 📝 What Real Server Logs Look Like (Log Anatomy)

In interviews, you may be shown a snippet of a log file and asked to diagnose the issue. Here are three typical production logs:

### 1. Database Connection Pool Exhaustion Log
```
2026-06-17 14:02:15.302 [ERROR] org.hibernate.engine.jdbc.connections.internal.BasicConnectionCreator - Connection pool exhausted. Active connections: 100/100. Timeout waiting for connection (5000ms).
2026-06-17 14:02:15.305 [ERROR] com.greyorange.wms.dispatch.OrderService - Failed to retrieve dispatch queue.
org.postgresql.util.PSQLException: FATAL: remaining connection slots are reserved for non-replication superuser connections
    at org.postgresql.core.v3.QueryExecutorImpl.receiveErrorResponse(QueryExecutorImpl.java:2440)
    at org.postgresql.core.v3.QueryExecutorImpl.readStartupMessages(QueryExecutorImpl.java:2559)
```
*   **Diagnosis**: The application server has hit its database connection limit. Every database slot is taken. New web requests are timing out waiting for a connection, throwing errors.
*   **Resolution**: Increase the max connections in database configuration (`postgresql.conf`), or audit the application code to ensure connections are being closed in a `finally` block.

### 2. Disk Space Full Log
```
2026-06-17 14:05:10.119 [WARN]  org.elasticsearch.cluster.routing.allocation.decider.DiskThresholdDecider - flood stage disk watermark [95%] exceeded on node [grey-node-01], all indices are marked as read-only.
2026-06-17 14:05:10.125 [ERROR] com.greyorange.wms.search.InventorySearch - Failed to index inventory item ID 99201.
java.io.IOException: No space left on device
```
*   **Diagnosis**: The server's disk space utilization has exceeded the critical 95% threshold. ElasticSearch has automatically locked the databases to read-only mode to prevent corruption.
*   **Resolution**: Free up disk space immediately using `rm -rf` on old log archives in `/var/log`, or compress files using `gzip`. Once space is freed, unlock the read-only index state.

---

## 📧 Client Email Communication Templates

During a major production outage (P1), communication is as critical as the technical fix. Support engineers must bridge the technical gap.

### Template 1: Acknowledgment Email (Sent within 15 mins of P1 Outage)
```
Subject: [P1 INCIDENT ID: 88201] Investigate: Warehouse Dispatch Screen Freeze
To: Amazon Warehouse Operations Lead <ops-lead@amazon.com>

Dear Operations Team,

We have received your report regarding the freeze on the main inventory dispatch screen at the Chicago-02 location (Ticket ID: 88201). 

Our engineering team has classified this as a P1 Critical incident and has initiated active investigations. We are currently analyzing server resource utilization and database transaction logs to isolate the root cause.

We will provide a status update every 15 minutes, or sooner if a workaround is established.

Sincerely,
Ravi
Software Support Engineer, GreyOrange
```

### Template 2: Mid-Investigation Status Update
```
Subject: [UPDATE 1 - INCIDENT ID: 88201] Warehouse Dispatch Screen Freeze
To: Amazon Warehouse Operations Lead <ops-lead@amazon.com>

Dear Operations Team,

This is our 15-minute status update for Ticket ID: 88201.

Status:
Our team has isolated the issue. We observed a database connection pool exhaustion on the main application server, causing API timeouts. 

Action:
We are currently terminating idle database sessions to free up slots and are preparing to restart the dispatcher service. 

Next Update:
A further update will be sent in 15 minutes, or immediately upon service recovery.

Sincerely,
Ravi
Software Support Engineer, GreyOrange
```

### Template 3: Resolution & Verification Request
```
Subject: [RESOLVED - INCIDENT ID: 88201] Warehouse Dispatch Screen Freeze
To: Amazon Warehouse Operations Lead <ops-lead@amazon.com>

Dear Operations Team,

We are pleased to inform you that Ticket ID: 88201 is now resolved.

Action Taken:
We cleared locked database connections and optimized the pool threshold. The database connection slots have returned to normal levels (15% utilization).

Verification:
Our internal tests show that the dispatch screen is now fully responsive, and API requests are completing in under 200ms. 

Could you please verify operations on your end and let us know if we have your permission to close this ticket?

Sincerely,
Ravi
Software Support Engineer, GreyOrange
```
