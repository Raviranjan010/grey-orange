# 🐍 Programming & Data Structures — COMPLETE Study Guide
### GreyOrange Software Support Engineer Intern | Drive: June 23-24, 2026

> **Weight in Interview**: 5% | **Priority**: MEDIUM  
> As a support engineer, you will write scripts to automate repetitive diagnostic tasks, parse log files, and query data endpoints. Python is the industry standard for SRE and support automation.

---

## PART 1: Python Exception Handling

Exception handling allows scripts to handle runtime errors (like missing files or network drops) without crashing, which is critical for background automation scripts.

```python
try:
    with open("app.log", "r") as f:
        for line in f:      # Read line-by-line (memory safe!)
            if "ERROR" in line:
                print(line.strip())
except FileNotFoundError:
    print("File not found")
except PermissionError:
    print("Access denied - elevate privileges (sudo)")
except Exception as e:
    print(f"An unexpected error occurred: {e}")
finally:
    print("Scan completed")  # This block ALWAYS runs, used for resource cleanup
```

---

## PART 2: Python Data Structure Time Complexities

| Data Structure | Operation | Average Time Complexity | Worst Case Complexity | Support / Automation Use Case |
|---|---|---|---|---|
| **List (Array)** | Access by index | $O(1)$ | $O(1)$ | Accessing elements when index is known. |
| | Search by value | $O(n)$ | $O(n)$ | Scanning a list of servers one by one. |
| | Append | $O(1)$ | $O(1)$ | Adding a new log line to a results list. |
| | Insert / Delete | $O(n)$ | $O(n)$ | Modifying elements inside the middle of a list. |
| **Dictionary (Hash)**| Search by key | $O(1)$ | $O(n)$ | Fast lookup of server states: `{'server-01': 'UP'}`. |
| | Insert / Delete | $O(1)$ | $O(n)$ | Updating configuration parameters dynamically. |
| **Set** | Add / Remove | $O(1)$ | $O(n)$ | Maintaining a list of unique error codes seen. |
| | Membership (`in`) | $O(1)$ | $O(n)$ | Checking if an IP is in a blocklist: `if ip in blocked:`. |

---

## PART 3: Log Reading Memory Optimization (CRITICAL)-

When writing automation scripts, **never** read entire log files into memory using `.read()` or `.readlines()` on production servers. If a log file is 10GB, the system will run out of memory (OOM) and crash.

* **❌ Bad Approach (Loads entire file into RAM)**:
  ```python
  # WARNING: Will crash the server if file is large!
  with open("huge_app.log", "r") as file:
      lines = file.readlines()
      for line in lines:
          if "ERROR" in line:
              print(line)
  ```

* **✅ Good Approach (Generator: loads 1 line at a time)**:
  ```python
  # Memory consumption remains near zero regardless of log size!
  with open("huge_app.log", "r") as file:
      for line in file:  # Iterator loads line-by-line
          if "ERROR" in line:
              print(line.strip())
  ```

---

## PART 4: 10 Classic Coding Questions with Solutions

Practice these Python templates. Be ready to explain the time and space complexity of each.

### Q1: Reverse a String Iteratively
* **Logic**: Iterate through the string and prepend each character to a new string.
* **Code**:
  ```python
  def reverse_string(s):
      result = ""
      for char in s:
          result = char + result
      return result
  print(reverse_string("greyorange"))  # Output: egnaroyerg
  ```
* **Complexity**: Time: $O(n)$, Space: $O(n)$.

---

### Q2: Check if a String is a Palindrome
* **Logic**: Clean the string of non-alphanumeric characters, convert to lowercase, and check if it equals its reverse.
* **Code**:
  ```python
  def is_palindrome(s):
      cleaned = "".join(c.lower() for c in s if c.isalnum())
      return cleaned == cleaned[::-1]
  print(is_palindrome("Racecar"))  # Output: True
  ```
* **Complexity**: Time: $O(n)$, Space: $O(n)$.

---

### Q3: Check if Two Strings are Anagrams
* **Logic**: Compare if the sorted character lists of both strings are identical.
* **Code**:
  ```python
  def are_anagrams(s1, s2):
      return sorted(s1.lower()) == sorted(s2.lower())
  print(are_anagrams("silent", "listen"))  # Output: True
  ```
* **Complexity**: Time: $O(n \log n)$ due to sorting. Space: $O(n)$.

---

### Q4: Find Duplicate Elements in a List
* **Logic**: Use a set to keep track of elements we have seen. If we see an element again, add it to the duplicates set.
* **Code**:
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
  print(find_duplicates([1, 2, 3, 1, 2, 4]))  # Output: [1, 2]
  ```
* **Complexity**: Time: $O(n)$, Space: $O(n)$.

---

### Q5: FizzBuzz Implementation
* **Logic**: Loop from 1 to N. Check divisibility by both 3 and 5 first, then check individually.
* **Code**:
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
* **Complexity**: Time: $O(n)$, Space: $O(1)$.

---

### Q6: Fibonacci Sequence (Iterative vs Recursive)
* **Iterative Approach ($O(n)$ time - Best)**:
  ```python
  def fib_iterative(n):
      if n <= 0: return []
      if n == 1: return [0]
      seq = [0, 1]
      for i in range(2, n):
          seq.append(seq[-1] + seq[-2])
      return seq
  print(fib_iterative(6))  # Output: [0, 1, 1, 2, 3, 5]
  ```
* **Recursive Approach ($O(2^n)$ time - Not Recommended)**:
  ```python
  def fib_recursive(n):
      if n <= 1: return n
      return fib_recursive(n-1) + fib_recursive(n-2)
  ```
* **Interview Tip**: Explain that recursive Fibonacci has exponential time complexity $O(2^n)$ and can lead to stack overflow. The iterative approach is linear $O(n)$ and memory-safe.

---

### Q7: Binary Search on a Sorted List
* **Logic**: Set low and high pointers. While `low <= high`, calculate the mid point. If mid equals target, return index. Otherwise, shift pointers.
* **Code**:
  ```python
  def binary_search(arr, target):
      low, high = 0, len(arr) - 1
      while low <= high:
          mid = (low + high) // 2
          if arr[mid] == target:
              return mid
          elif arr[mid] < target:
              low = mid + 1
          else:
              high = mid - 1
      return -1
  print(binary_search([10, 20, 30, 40, 50], 30))  # Output: 2
  ```
* **Complexity**: Time: $O(\log n)$, Space: $O(1)$.

---

### Q8: Find the Factorial of a Number
* **Logic**: Accumulate product by multiplying values from 1 to N.
* **Code**:
  ```python
  def factorial(n):
      result = 1
      for i in range(1, n + 1):
          result *= i
      return result
  print(factorial(5))  # Output: 120
  ```
* **Complexity**: Time: $O(n)$, Space: $O(1)$.

---

### Q9: Merge Two Sorted Lists
* **Logic**: Compare elements from both lists using two pointers, appending the smaller value to the result.
* **Code**:
  ```python
  def merge_sorted_lists(l1, l2):
      result = []
      i, j = 0, 0
      while i < len(l1) and j < len(l2):
          if l1[i] < l2[j]:
              result.append(l1[i])
              i += 1
          else:
              result.append(l2[j])
              j += 1
      result.extend(l1[i:])
      result.extend(l2[j:])
      return result
  print(merge_sorted_lists([1, 3, 5], [2, 4, 6]))  # Output: [1, 2, 3, 4, 5, 6]
  ```
* **Complexity**: Time: $O(n + m)$, Space: $O(n + m)$.

---

### Q10: Find Max and Min in an Array with a Single Scan
* **Logic**: Track min and max values starting with the first element and update them during a single pass.
* **Code**:
  ```python
  def find_max_min(arr):
      if not arr: return None, None
      min_val = max_val = arr[0]
      for num in arr[1:]:
          if num < min_val:
              min_val = num
          elif num > max_val:
              max_val = num
      return min_val, max_val
  print(find_max_min([5, 2, 9, 1, 7]))  # Output: (1, 9)
  ```
* **Complexity**: Time: $O(n)$, Space: $O(1)$.
