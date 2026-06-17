# 🐍 Programming & Data Structures: Comprehensive Study Guide

As a Software Support Engineer, you will write automation scripts to automate repetitive checks, parse text logs, and query databases. Python is the industry standard for SRE and Support automation.

---

## 📋 Python & Programming Q&As

Please tap on any question below to expand its detailed answer.

<details>
<summary><b>Q1: What is exception handling in Python? Why is it useful in support?</b></summary>
<br>
<blockquote>
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

## 📊 Python Data Structure Time Complexities

| Data Structure | Operation | Average Time Complexity | Explanation |
|---|---|---|---|
| **List (Array)** | Access by index | $O(1)$ | Direct pointer arithmetic in memory. |
| | Search by value | $O(n)$ | Must scan the entire list element-by-element. |
| | Append | $O(1)$ | Adds to the end of the memory block. |
| | Insert / Delete | $O(n)$ | Must shift all subsequent elements in memory. |
| **Dictionary (Hash)**| Search key | $O(1)$ | Uses a hashing function to jump directly to memory. |
| | Insert / Delete | $O(1)$ | Direct memory mapping. |
| **Set** | Add / Remove | $O(1)$ | Implemented internally as a hash map with dummy values. |
| | Membership (`in`) | $O(1)$ | Ultra-fast checking if a value exists (deduplication). |

---

## 📂 File I/O: Log Reading Memory Optimization
When reading log files in production, **never** read the entire file into memory using `.read()` or `.readlines()`. If the log file is 10GB, the server will crash due to Out-Of-Memory (OOM).

*   **Bad Approach (Loads whole file to RAM)**:
    ```python
    # Warning: Will freeze the server if file is large!
    with open("huge_app.log", "r") as file:
        lines = file.readlines()
        for line in lines:
            if "ERROR" in line:
                print(line)
    ```
*   **Good Approach (Line-by-line Generator)**:
    ```python
    # Recommended: Memory footprint is extremely small (loads 1 line at a time)
    with open("huge_app.log", "r") as file:
        for line in file:
            if "ERROR" in line:
                print(line.strip())
    ```

---

## 📝 10 Classic Coding Interview Questions with Solutions

Use these Python templates to practice. Be ready to explain the complexity of each.

### Q1: Reverse a String Iteratively
*   **Logic**: Start with an empty string and loop through each character of the input, prepending it to the result.
*   **Code**:
    ```python
    def reverse_string(s):
        result = ""
        for char in s:
            result = char + result
        return result
    print(reverse_string("greyorange"))  # Output: egnaroyerg
    ```
*   **Complexity**: Time: $O(n)$, Space: $O(n)$.

---

### Q2: Check if a String is a Palindrome
*   **Logic**: Strip non-alphanumeric characters, convert to lowercase, and check if the string reads the same backwards.
*   **Code**:
    ```python
    def is_palindrome(s):
        cleaned = "".join(c.lower() for c in s if c.isalnum())
        return cleaned == cleaned[::-1]
    print(is_palindrome("Racecar"))  # Output: True
    ```
*   **Complexity**: Time: $O(n)$, Space: $O(n)$.

---

### Q3: Check if Two Strings are Anagrams
*   **Logic**: Sort the characters of both strings and compare if the sorted lists are identical.
*   **Code**:
    ```python
    def are_anagrams(s1, s2):
        return sorted(s1.lower()) == sorted(s2.lower())
    print(are_anagrams("silent", "listen"))  # Output: True
    ```
*   **Complexity**: Time: $O(n \log n)$ due to sorting. Space: $O(n)$.

---

### Q4: Find Duplicate Elements in a List
*   **Logic**: Maintain a `seen` set and a `duplicates` set. If an element exists in `seen`, add it to `duplicates`.
*   **Code**:
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
*   **Complexity**: Time: $O(n)$, Space: $O(n)$.

---

### Q5: FizzBuzz Implementation
*   **Logic**: Loop from 1 to N. Check divisibility by both 3 and 5 first, then individually.
*   **Code**:
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
*   **Complexity**: Time: $O(n)$, Space: $O(1)$.

---

### Q6: Fibonacci Sequence (Iterative vs. Recursive)
*   **Iterative ($O(n)$ time - Best)**:
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
*   **Recursive ($O(2^n)$ time - starves resources)**:
    ```python
    def fib_recursive(n):
        if n <= 1: return n
        return fib_recursive(n-1) + fib_recursive(n-2)
    ```
*   **Interview Tip**: Explain that recursive Fibonacci has exponential time complexity ($O(2^n)$) and can cause stack overflow. The iterative approach is linear ($O(n)$) and much safer.

---

### Q7: Binary Search on a Sorted List
*   **Logic**: Set low and high pointers. Loop while `low <= high`. Check middle element. Shift pointers.
*   **Code**:
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
    print(binary_search([10, 20, 30, 40, 50], 30))  # Output: 2 (index)
    ```
*   **Complexity**: Time: $O(\log n)$, Space: $O(1)$.

---

### Q8: Find the Factorial of a Number
*   **Logic**: Maintain an accumulator and multiply values decrementing to 1.
*   **Code**:
    ```python
    def factorial(n):
        result = 1
        for i in range(1, n + 1):
            result *= i
        return result
    print(factorial(5))  # Output: 120
    ```
*   **Complexity**: Time: $O(n)$, Space: $O(1)$.

---

### Q9: Merge Two Sorted Lists into One Sorted List
*   **Logic**: Use two pointers to compare elements from both lists, appending the smaller value to the result.
*   **Code**:
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
*   **Complexity**: Time: $O(n + m)$, Space: $O(n + m)$.

---

### Q10: Find Max and Min in an Array with a Single Scan
*   **Logic**: Initialize max and min to the first element, then scan the rest of the array, updating them.
*   **Code**:
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
*   **Complexity**: Time: $O(n)$, Space: $O(1)$.
