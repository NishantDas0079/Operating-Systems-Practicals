# Operating Systems Practical File

**B.Sc. (H) Computer Science — Semester III**  
Ramanujan College, University of Delhi  
Paper: Operating Systems (DSC08)

| | |
|---|---|
| **Name** | Nishant Das |
| **Roll No.** | 25020570008 |
| **Course** | B.Sc. (H) Computer Science |
| **Semester** | III |
| **Paper** | Operating Systems (DSC08) |
| **Language** | Python 3 |

---

## Contents

| # | Practical | Program |
|---|-----------|:-------:|
| 1 | [Demonstration of Operating System Functions using an OS Simulator](#practical-1) | — |
| 2 | [LINUX Commands for Information, File and Directory Management](#practical-2) | — |
| 3 | [LINUX Commands for Process Control, Communication and Protection](#practical-3) | — |
| 4 | [Parent and Child Processes using fork() and exec()](#practical-4) | Yes |
| 5 | [Kernel Version and CPU Information](#practical-5) | Yes |
| 6 | [Configured, Free and Used Memory Information](#practical-6) | Yes |
| 7 | [Copying a File using System Calls](#practical-7) | Yes |
| 8 | [First Come First Serve (FCFS) Scheduling Algorithm](#practical-8) | Yes |
| 9 | [Shortest Job First (SJF) Scheduling Algorithm](#practical-9) | Yes |
| 10 | [Non-preemptive Priority Based Scheduling Algorithm](#practical-10) | Yes |
| 11 | [Sum of n Numbers using Pthreads](#practical-11) | Yes |
| 12 | [First-Fit, Best-Fit and Worst-Fit Allocation Strategies](#practical-12) | Yes |

---

## Requirements

- **Python 3.x** — all programs are written in Python 3.
- **Linux** — Practicals 4, 5, 6 and 7 are written for a Linux system. Practicals 5 and 6 read kernel and memory information through `platform` and `os.sysconf()`, Practical 7 uses the file-descriptor system calls `os.open`, `os.read`, `os.write` and `os.close`, and Practical 4 creates its child process with the `multiprocessing` module.
- No third-party packages are required. Only the Python standard library is used (`os`, `sys`, `platform`, `time`, `threading`, `multiprocessing`).

## How to run

Save each program as a `.py` file and run it from a terminal:

```bash
python3 <program>.py
```

Programs that read input (Practicals 8, 9, 10, 11 and 12) prompt for values at the terminal. Practical 7 takes its arguments on the command line.

---

<a id="practical-1"></a>

## Practical 1 — Demonstration of Operating System Functions using an OS Simulator

### Question

Demonstration of various operating system functions using an OS simulator.

> **Note:** Demonstrated using an OS simulator, covering Process Scheduling, Memory Management, Disk Scheduling and File Management. Output is recorded as screenshots in the practical file.

---

<a id="practical-2"></a>

## Practical 2 — LINUX Commands for Information, File and Directory Management

### Question

Execute various LINUX commands for:

1. **Information Maintenance:** `wc`, `clear`, `cal`, `who`, `date`, `pwd`
2. **File Management:** `cat`, `cp`, `rm`, `mv`, `cmp`, `comm`, `diff`, `find`, `grep`, `awk`
3. **Directory Management:** `cd`, `mkdir`, `rmdir`, `ls`

> **Note:** Executed on a Linux terminal. Output is recorded as screenshots in the practical file.

---

<a id="practical-3"></a>

## Practical 3 — LINUX Commands for Process Control, Communication and Protection

### Question

Execute various LINUX commands for:

1. **Process Control:** `fork`, `getpid`, `ps`, `kill`, `sleep`
2. **Communication:** Input-output redirection, Pipe
3. **Protection Management:** `chmod`, `chown`, `chgrp`

> **Note:** Executed on a Linux terminal. Output is recorded as screenshots in the practical file.

---

<a id="practical-4"></a>

## Practical 4 — Parent and Child Processes using fork() and exec()

### Question

Write a program (using `fork()` and/or `exec()` commands) where the parent and the child execute:

1. the same program, the same code;
2. the same program, different code;
3. the parent waits for the child to finish its task before terminating.

### Program

#### (i) Same program, same code

```python
import multiprocessing
import os


def task():
    print("Process id =", os.getpid(), " parent process id =", os.getppid())
    print("This line is executed by both the parent and the child.")


if __name__ == "__main__":
    p = multiprocessing.Process(target=task)
    p.start()

    task()

    p.join()
```

#### (ii) Same program, different code

```python
import multiprocessing
import os


def child_task():
    print("Child  : pid =", os.getpid())
    print("Child  : executing the child code.")


def parent_task():
    print("Parent : pid =", os.getpid())
    print("Parent : executing the parent code.")


if __name__ == "__main__":
    p = multiprocessing.Process(target=child_task)
    p.start()

    parent_task()

    p.join()
```

#### (iii) Parent waits for the child to finish

```python
import multiprocessing

import time


def child_task():
    print("Child  : started, performing its task.", flush=True)
    time.sleep(3)
    print("Child  : task finished, child is terminating.")


if __name__ == "__main__":
    p = multiprocessing.Process(target=child_task)
    p.start()

    print("Parent : waiting for the child to finish.", flush=True)

    p.join()

    print("Parent : child has terminated, parent is now terminating.")
```

---

<a id="practical-5"></a>

## Practical 5 — Kernel Version and CPU Information

### Question

Write a program to report the behaviour of the Linux kernel including the kernel version, the CPU type and the CPU information.

### Program

```python
import os
import platform

print("System name    :", platform.system())
print("Node name      :", platform.node())
print("Kernel release :", platform.release())
print("Kernel version :", platform.version())
print("Machine (CPU)  :", platform.machine())
print("Processor      :", platform.processor())
print("Architecture   :", platform.architecture()[0])
print("Number of CPUs :", os.cpu_count())
```

---

<a id="practical-6"></a>

## Practical 6 — Configured, Free and Used Memory Information

### Question

Write a program to report the behaviour of the Linux kernel including information on the configured memory and the amount of free and used memory.

### Program

```python
import os
import platform

if platform.system() == "Windows":
    import ctypes

    class MemoryStatus(ctypes.Structure):
        _fields_ = [("length", ctypes.c_ulong),
                    ("load", ctypes.c_ulong),
                    ("total_phys", ctypes.c_ulonglong),
                    ("free_phys", ctypes.c_ulonglong),
                    ("total_page", ctypes.c_ulonglong),
                    ("free_page", ctypes.c_ulonglong),
                    ("total_virtual", ctypes.c_ulonglong),
                    ("free_virtual", ctypes.c_ulonglong),
                    ("avail_extended", ctypes.c_ulonglong)]

    status = MemoryStatus()
    status.length = ctypes.sizeof(MemoryStatus)
    ctypes.windll.kernel32.GlobalMemoryStatusEx(ctypes.byref(status))

    total = status.total_phys / (1024 * 1024)
    free = status.free_phys / (1024 * 1024)

else:
    page_size = os.sysconf("SC_PAGE_SIZE")
    total_pages = os.sysconf("SC_PHYS_PAGES")
    free_pages = os.sysconf("SC_AVPHYS_PAGES")

    total = (page_size * total_pages) / (1024 * 1024)
    free = (page_size * free_pages) / (1024 * 1024)

used = total - free

print("Configured memory : %.2f MB" % total)
print("Free memory       : %.2f MB" % free)
print("Used memory       : %.2f MB" % used)
```

---

<a id="practical-7"></a>

## Practical 7 — Copying a File using System Calls

### Question

Write a program to copy the contents of one file into another file using system calls.

### Program

```python
import os
import sys

if len(sys.argv) != 3:
    print("Usage: python copy.py <source file> <destination file>")
    sys.exit(1)

src = os.open(sys.argv[1], os.O_RDONLY)
dst = os.open(sys.argv[2], os.O_WRONLY | os.O_CREAT | os.O_TRUNC, 0o644)

while True:
    data = os.read(src, 1024)
    if not data:
        break
    os.write(dst, data)

os.close(src)
os.close(dst)

print("File copied successfully from", sys.argv[1], "to", sys.argv[2])
```

> **Note:** The program takes the source and destination file names as command-line arguments:
> 
> ```bash
> python3 practical_07_copy_file.py source.txt destination.txt
> ```

---

<a id="practical-8"></a>

## Practical 8 — First Come First Serve (FCFS) Scheduling Algorithm

### Question

Write a program to implement the First Come First Serve (FCFS) scheduling algorithm and to compute the waiting time and the turnaround time of each process.

### Program

```python
n = int(input("Enter the number of processes: "))

pid = []
bt = []

for i in range(n):
    pid.append(i + 1)
    bt.append(int(input("Enter the burst time of process P%d: " % (i + 1))))

wt = [0] * n
tat = [0] * n

for i in range(1, n):
    wt[i] = wt[i - 1] + bt[i - 1]

print("\nProcess\tBurst Time\tWaiting Time\tTurnaround Time")

total_wt = 0
total_tat = 0

for i in range(n):
    tat[i] = wt[i] + bt[i]
    total_wt = total_wt + wt[i]
    total_tat = total_tat + tat[i]
    print("P%d\t%d\t\t%d\t\t%d" % (pid[i], bt[i], wt[i], tat[i]))

print("\nAverage waiting time    = %.2f" % (total_wt / n))
print("Average turnaround time = %.2f" % (total_tat / n))
```

---

<a id="practical-9"></a>

## Practical 9 — Shortest Job First (SJF) Scheduling Algorithm

### Question

Write a program to implement the Shortest Job First (SJF) non-preemptive scheduling algorithm and to compute the waiting time and the turnaround time of each process.

### Program

```python
n = int(input("Enter the number of processes: "))

pid = list(range(1, n + 1))
bt = []

for i in range(n):
    bt.append(int(input("Enter the burst time of process P%d: " % (i + 1))))

processes = sorted(zip(bt, pid))
bt = [item[0] for item in processes]
pid = [item[1] for item in processes]

wt = [0] * n
tat = [0] * n

for i in range(1, n):
    wt[i] = wt[i - 1] + bt[i - 1]

print("\nProcess\tBurst Time\tWaiting Time\tTurnaround Time")

total_wt = 0
total_tat = 0

for i in range(n):
    tat[i] = wt[i] + bt[i]
    total_wt = total_wt + wt[i]
    total_tat = total_tat + tat[i]
    print("P%d\t%d\t\t%d\t\t%d" % (pid[i], bt[i], wt[i], tat[i]))

print("\nAverage waiting time    = %.2f" % (total_wt / n))
print("Average turnaround time = %.2f" % (total_tat / n))
```

---

<a id="practical-10"></a>

## Practical 10 — Non-preemptive Priority Based Scheduling Algorithm

### Question

Write a program to implement the non-preemptive priority based scheduling algorithm and to compute the waiting time and the turnaround time of each process. A smaller priority number denotes a higher priority.

### Program

```python
n = int(input("Enter the number of processes: "))

pid = list(range(1, n + 1))
bt = []
pr = []

for i in range(n):
    bt.append(int(input("Enter the burst time of process P%d: " % (i + 1))))
    pr.append(int(input("Enter the priority of process P%d: " % (i + 1))))

processes = sorted(zip(pr, pid, bt))
pr = [item[0] for item in processes]
pid = [item[1] for item in processes]
bt = [item[2] for item in processes]

wt = [0] * n
tat = [0] * n

for i in range(1, n):
    wt[i] = wt[i - 1] + bt[i - 1]

print("\nProcess\tBurst Time\tPriority\tWaiting Time\tTurnaround Time")

total_wt = 0
total_tat = 0

for i in range(n):
    tat[i] = wt[i] + bt[i]
    total_wt = total_wt + wt[i]
    total_tat = total_tat + tat[i]
    print("P%d\t%d\t\t%d\t\t%d\t\t%d" % (pid[i], bt[i], pr[i], wt[i], tat[i]))

print("\nAverage waiting time    = %.2f" % (total_wt / n))
print("Average turnaround time = %.2f" % (total_tat / n))
```

---

<a id="practical-11"></a>

## Practical 11 — Sum of n Numbers using Pthreads

### Question

Write a program to calculate the sum of n numbers using Pthreads.

### Program

```python
import threading

n = int(input("Enter the number of elements: "))

print("Enter %d numbers:" % n)

numbers = []

for i in range(n):
    numbers.append(int(input()))

total = 0
lock = threading.Lock()


def add(value):
    global total
    lock.acquire()
    total = total + value
    lock.release()


threads = []

for value in numbers:
    t = threading.Thread(target=add, args=(value,))
    threads.append(t)
    t.start()

for t in threads:
    t.join()

print("Sum of the %d numbers = %d" % (n, total))
```

---

<a id="practical-12"></a>

## Practical 12 — First-Fit, Best-Fit and Worst-Fit Allocation Strategies

### Question

Write a program to implement the first-fit, best-fit and worst-fit memory allocation strategies and to display the block allotted to each process under each strategy.

### Program

```python
nb = int(input("Enter the number of blocks: "))

print("Enter the size of each block:")

block = []

for i in range(nb):
    block.append(int(input()))

np = int(input("Enter the number of processes: "))

print("Enter the size of each process:")

process = []

for i in range(np):
    process.append(int(input()))

b1 = block[:]
b2 = block[:]
b3 = block[:]

first = [-1] * np
best = [-1] * np
worst = [-1] * np

for i in range(np):
    for j in range(nb):
        if b1[j] >= process[i]:
            first[i] = j
            b1[j] = b1[j] - process[i]
            break

for i in range(np):
    index = -1
    for j in range(nb):
        if b2[j] >= process[i]:
            if index == -1 or b2[j] < b2[index]:
                index = j
    if index != -1:
        best[i] = index
        b2[index] = b2[index] - process[i]

for i in range(np):
    index = -1
    for j in range(nb):
        if b3[j] >= process[i]:
            if index == -1 or b3[j] > b3[index]:
                index = j
    if index != -1:
        worst[i] = index

        b3[index] = b3[index] - process[i]

print("\nProcess\tSize\tFirst Fit\tBest Fit\tWorst Fit")

for i in range(np):
    row = "P%d\t%d\t" % (i + 1, process[i])
    if first[i] != -1:
        row = row + "Block %d" % (first[i] + 1)
    else:
        row = row + "Not allocated"
    if best[i] != -1:
        row = row + "\t\tBlock %d" % (best[i] + 1)
    else:
        row = row + "\t\tNot allocated"
    if worst[i] != -1:
        row = row + "\t\tBlock %d" % (worst[i] + 1)
    else:
        row = row + "\t\tNot allocated"
    print(row)
```

---

## Notes

- All programs are written in Python 3 and use only the standard library.
- Practicals 1, 2 and 3 are demonstrations and Linux command sessions; their output is recorded as screenshots in the practical file.
- Practical 4 uses the `multiprocessing` module to create the child process, and Practical 11 uses the `threading` module for thread creation.

---

*Practical file submitted as part of the Operating Systems (DSC08) course, B.Sc. (H) Computer Science, Semester III, Ramanujan College, University of Delhi.*
