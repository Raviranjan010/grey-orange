# 🛠️ Software Support & Troubleshooting Scenarios

This guide details the structural and analytical mindset required for a Software Support role. GreyOrange will assess how you handle critical production incidents under pressure and how you communicate with clients.

---

## 📋 Collapsible Situation-Based Scenarios (Critical for Interviews)

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

## 🌟 How to Use the "STAR" Method in Interviews

When the interviewer asks: **"Tell me about a time you solved a complex technical bug."** or **"Describe a conflict you resolved with a teammate."**

* **S - Situation**: Provide context. Set the scene.
  * *Example*: "During our college hackathon, we built a web app for inventory tracking. 2 hours before the final demo, the server started throwing 500 Internal Server Errors for all users."
* **T - Task**: Describe the challenge/responsibility.
  * *Example*: "I was responsible for the backend API and server deployments, so I had to find and fix the bug immediately without losing database data."
* **A - Action**: Explain the logical steps you took.
  * *Example*: "I SSH'd into the server, checked disk space using `df -h`, and saw it was healthy. I then ran `tail -f` on the API log file and monitored HTTP requests. I noticed that when users uploaded product images, the backend tried to write files to a temporary directory that lacked write permissions. I executed `chmod 755 /uploads` and updated the Python script to catch permission exceptions."
* **R - Result**: State the positive outcome.
  * *Example*: "The server stabilized, the 500 errors disappeared, and we successfully presented our live demo, ultimately winning 2nd place in the hackathon."

---

## 🚨 Incident Priority Levels (SLA)
Software Support Engineers operate under Service Level Agreements (SLAs) based on incident severity:

| Priority | Severity | Impact | Target Resolution | Support Example |
|---|---|---|---|---|
| **P1** | Critical | Entire system/warehouse is down. Operations halted. | < 1-2 Hours | Database server crashed; robots stopped moving. |
| **P2** | Major | System runs, but core features are failing or extremely slow. | < 4 Hours | Orders are getting stuck in the queue; UI loads after 30 seconds. |
| **P3** | Minor | Minor bugs, typos, workaround exists. | < 24-48 Hours | A specific user cannot change their profile picture. |
