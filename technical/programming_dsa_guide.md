# 🐍 Programming & Data Structures Prep Guide

As a Software Support Engineer, you will write automation scripts to automate repetitive checks, parse text logs, and query databases. While you can use Java or C++, Python is the industry standard for SRE and Support automation.

---

## 🐍 Part 1: Python Basics for Support Engineering

### 1. Key Data Structures
* **Lists**: Ordered, mutable sequence of items.
  ```python
  servers = ["server-01", "server-02", "server-03"]
  servers.append("server-04")
  print(servers[0])  # Output: server-01
  ```
* **Dictionaries (Hash Maps)**: Key-value pairs. Very fast lookup.
  ```python
  server_status = {"server-01": "Active", "server-02": "Down"}
  server_status["server-03"] = "Active"
  print(server_status["server-01"])  # Output: Active
  ```
* **Tuples**: Ordered, immutable sequence of items.
  ```python
  port_config = ("127.0.0.1", 8080)
  ```
* **Sets**: Unordered collection of unique items. Excellent for finding unique errors.
  ```python
  unique_errors = {"TimeoutError", "ConnectionRefused"}
  unique_errors.add("TimeoutError")  # Will not duplicate
  ```

### 2. Exception Handling (Critical for Support Scripts)
Your scripts must not crash when they encounter file access errors or network timeouts. Always wrap operations in try-except blocks.
```python
try:
    with open("/var/log/app.log", "r") as file:
        data = file.read()
except FileNotFoundError:
    print("Error: The log file was not found.")
except PermissionError:
    print("Error: You do not have permissions to read this file.")
except Exception as e:
    print(f"An unexpected error occurred: {e}")
finally:
    print("Execution completed.")  # Always runs
```

---

## 🤖 Python Automation Script Examples

### Script 1: Scan Log File for Keywords
A production script to find and extract error logs with line numbers:
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

# Usage:
scan_logs("app_log.txt", "Exception")
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

# Usage:
check_disk_usage()
```

---

## 📊 Part 2: Data Structures & Algorithms (DSA) Theory

### 1. Operations Complexity (Big O)
| Data Structure | Access | Search | Insertion | Deletion |
|---|---|---|---|---|
| **Array / List** | $O(1)$ | $O(n)$ | $O(n)$ | $O(n)$ |
| **Singly Linked List** | $O(n)$ | $O(n)$ | $O(1)$ | $O(1)$ |
| **Stack (LIFO)** | $O(n)$ | $O(n)$ | $O(1)$ | $O(1)$ |
| **Queue (FIFO)** | $O(n)$ | $O(n)$ | $O(1)$ | $O(1)$ |
| **Hash Map / Dict** | $O(1)$ | $O(1)$ | $O(1)$ | $O(1)$ |

### 2. Searching & Sorting Algorithms
* **Linear Search**: Checks each element sequentially. Time complexity: $O(n)$.
* **Binary Search**: Searches a **sorted** array by repeatedly dividing the search interval in half. Time complexity: $O(\log n)$.
* **Bubble Sort**: Repeatedly swaps adjacent elements if they are in the wrong order. Time complexity: $O(n^2)$.
* **Merge Sort**: Divide-and-conquer algorithm. Splits array, sorts halves, merges them. Time complexity: $O(n \log n)$.
* **Quick Sort**: Picks a pivot, partitions array around it. Avg Time complexity: $O(n \log n)$, Worst: $O(n^2)$.

---

## 📝 Coding Questions & Solutions (Must Practice)

### Q1: Reverse a string without using slicing.
```python
def reverse_string(s):
    reversed_str = ""
    for char in s:
        reversed_str = char + reversed_str
    return reversed_str

print(reverse_string("greyorange"))  # egnaroyerg
```

### Q2: Check if a string is a Palindrome.
```python
def is_palindrome(s):
    # Clean the string (remove spaces and convert to lowercase)
    cleaned = "".join(char.lower() for char in s if char.isalnum())
    return cleaned == cleaned[::-1]

print(is_palindrome("A man a plan a canal Panama"))  # True
```

### Q3: FizzBuzz problem.
```python
def fizz_buzz(n):
    for i in range(1, n + 1):
        if i % 3 == 0 and i % 5 == 0:
            print("FizzBuzz")
        elif i % 3 == 0:
            print("Fizz")
        elif i % 5 == 0:
            print("Buzz")
        else:
            print(i)
```

### Q4: Find the duplicate elements in an array.
```python
def find_duplicates(arr):
    seen = set()
    duplicates = set()
    for num in arr:
        if num in seen:
            duplicates.add(num)
        else:
            seen.add(num)
    return list(duplicates)

print(find_duplicates([1, 2, 3, 2, 4, 5, 1]))  # [1, 2]
```

### Q5: Check if two strings are Anagrams.
```python
def are_anagrams(str1, str2):
    return sorted(str1.lower()) == sorted(str2.lower())

print(are_anagrams("listen", "silent"))  # True
```
