# 🐍 Programming & Data Structures Prep Guide

As a Software Support Engineer, you will write automation scripts to automate repetitive checks, parse text logs, and query databases. Python is the industry standard for SRE and Support automation.

---

## 📋 Python & Programming Q&As

Please tap on any question below to expand its detailed answer.

<details>
<summary><b>Q1: What is exception handling in Python? Why is it useful in support?</b></summary>
<br>
<blockquote>
<b>Answer:</b>
Exception handling is a mechanism in Python to handle runtime errors without letting the program crash. It is implemented using <code>try</code>, <code>except</code>, <code>else</code>, and <code>finally</code> blocks.
<br><br>
<b>Why it's critical in support:</b>
When writing diagnostic tools or log parsers, the script might encounter issues like a log file that is missing, a locked database, or permission restrictions. Without exception handling, the script would crash mid-execution, interrupting automation. With exception handling, the script can log the failure, alert the team, and clean up connections.
<br><br>
<b>Example Code:</b>
<pre><code>try:
    # Attempt to open and read a log file
    with open("/var/log/app.log", "r") as file:
        data = file.read()
except FileNotFoundError:
    print("Warning: Log file not found. Proceeding with default state.")
except PermissionError:
    print("Error: Access denied. Elevate privileges (sudo).")
except Exception as e:
    print(f"Unexpected error: {e}")
finally:
    print("Execution completed.")  # This block always executes (e.g. to close connections)</code></pre>
</blockquote>
</details>

<details>
<summary><b>Q2: What Python data structures have you used, and what are their use cases?</b></summary>
<br>
<blockquote>
<b>Answer:</b>
<ul>
  <li><b>Lists (Arrays):</b> Ordered, mutable collections. Used to store sequences of items (e.g., list of server names: <code>['server-01', 'server-02']</code>).</li>
  <li><b>Dictionaries (Hash Maps):</b> Unordered key-value pairs. Used for ultra-fast lookup (O(1)) (e.g., checking status of servers: <code>{'server-01': 'Active', 'server-02': 'Down'}</code>).</li>
  <li><b>Sets:</b> Unordered collections of unique elements. Used to deduplicate items (e.g., extracting unique error codes from a log file).</li>
  <li><b>Tuples:</b> Ordered, immutable collections. Used for constant data structures (e.g., server IP and port configuration: <code>('127.0.0.1', 8080)</code>).</li>
</ul>
</blockquote>
</details>

---

## 🤖 Python Support Automation Scripts (Must Know)

### Script 1: Scan Log File for Keywords
Used to scan files line-by-line (preventing memory exhaustion for huge files) and print matches with line numbers.
```python
def scan_logs(file_path, keyword):
    try:
        with open(file_path, "r") as file:
            print(f"--- Scanning for '{keyword}' in {file_path} ---")
            found = False
            for line_num, line in enumerate(file, 1):
                if keyword.upper() in line.upper():
                    print(f"Line {line_num}: {line.strip()}")
                    found = True
            if not found:
                print("No matching keywords found.")
    except FileNotFoundError:
        print(f"File {file_path} not found.")
```

### Script 2: Monitor Disk Space & Alert
Check disk capacity and trigger an alert if utilization exceeds 90%:
```python
import shutil

def check_disk_usage(path="/", threshold=90):
    total, used, free = shutil.disk_usage(path)
    percent_used = (used / total) * 100
    print(f"Disk Space: {percent_used:.2f}% used.")
    
    if percent_used >= threshold:
        print(f"⚠️ WARNING: Disk usage on '{path}' is at {percent_used:.2f}%! Please clean up logs.")
    else:
        print("Disk health status: OK.")
```

---

## 📊 Operations Complexity (Big O)
* **Linear Search**: Time complexity: $O(n)$.
* **Binary Search**: Searches a **sorted** array. Time complexity: $O(\log n)$.
* **Bubble Sort**: Time complexity: $O(n^2)$.
* **Merge Sort**: Time complexity: $O(n \log n)$.
* **Quick Sort**: Avg Time complexity: $O(n \log n)$, Worst: $O(n^2)$.
