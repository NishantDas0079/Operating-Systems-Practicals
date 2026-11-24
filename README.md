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

| # | Practical | Type |
|---|-----------|:--------:|
| 1 | [Demonstration of Operating System Functions using an OS Simulator](#practical-1) | Demo |
| 2 | [LINUX Commands for Information, File and Directory Management](#practical-2) | Commands |
| 3 | [LINUX Commands for Process Control, Communication and Protection](#practical-3) | Commands |
| 4 | [Parent and Child Processes using fork() and exec()](#practical-4) | Program |
| 5 | [Kernel Version and CPU Information](#practical-5) | Program |
| 6 | [Configured, Free and Used Memory Information](#practical-6) | Program |
| 7 | [Copying a File using System Calls](#practical-7) | Program |
| 8 | [First Come First Serve (FCFS) Scheduling Algorithm](#practical-8) | Program |
| 9 | [Shortest Job First (SJF) Scheduling Algorithm](#practical-9) | Program |
| 10 | [Non-preemptive Priority Based Scheduling Algorithm](#practical-10) | Program |
| 11 | [Sum of n Numbers using Pthreads](#practical-11) | Program |
| 12 | [First-Fit, Best-Fit and Worst-Fit Allocation Strategies](#practical-12) | Program |

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

### Solution

> Outputs that depend on the machine — the working directory, the date, the logged-in user and file ownership — are shown illustratively. Everything else below was captured from a real Linux session.

#### (i) Information Maintenance

| Command | Purpose | Example |
|---|---|---|
| `pwd` | Prints the absolute path of the current working directory | `pwd` |
| `date` | Displays the current system date and time | `date` |
| `cal` | Displays a calendar — the current month by default, or a given month/year | `cal`, `cal 2026` |
| `who` | Lists the users currently logged in to the system | `who` |
| `wc` | Counts lines, words and bytes in a file | `wc notes.txt` |
| `clear` | Clears the terminal screen | `clear` |

```console
$ pwd
/home/nishant/os-practical

$ date
Tue Nov 24 07:15:19 UTC 2026

$ who
nishant  tty7         2026-11-24 09:02 (:0)

$ wc notes.txt
 3  7 56 notes.txt

$ wc -l notes.txt
3 notes.txt

$ wc -w notes.txt
7 notes.txt

$ wc -c notes.txt
56 notes.txt
```

`wc` prints the line, word and byte counts in that order; the options `-l`, `-w` and `-c` select just one of them.

`cal` prints a month grid, and `cal 2026` prints the whole year. On Debian and Ubuntu it is provided by the `ncal` package. `clear` produces no output — it resets the terminal display.

#### (ii) File Management

| Command | Purpose | Example |
|---|---|---|
| `cat` | Displays the contents of a file, or concatenates several files | `cat notes.txt` |
| `cp` | Copies a file (`-r` for a directory and its contents) | `cp notes.txt notes_backup.txt` |
| `rm` | Removes files (`-r` recursive, `-f` force) | `rm notes_backup.txt` |
| `mv` | Moves a file, or renames it if the destination is in the same directory | `mv notes_backup.txt renamed.txt` |
| `cmp` | Compares two files byte by byte and reports the first difference | `cmp fruits1.txt fruits2.txt` |
| `comm` | Compares two **sorted** files line by line and prints three columns | `comm s1.txt s2.txt` |
| `diff` | Shows the line-by-line differences needed to turn one file into the other | `diff fruits1.txt fruits2.txt` |
| `find` | Searches the directory tree for files by name, type or other attributes | `find . -name "*.txt"` |
| `grep` | Searches for a pattern inside files and prints the matching lines | `grep "an" fruits1.txt` |
| `awk` | Pattern-scanning language used for field extraction and arithmetic on columns | `awk '{ print NR": "$0 }' fruits1.txt` |

```console
$ cat notes.txt
Operating Systems
Linux commands practical
Semester III

$ cp notes.txt notes_backup.txt
$ mv notes_backup.txt renamed.txt

$ cmp fruits1.txt fruits1_copy.txt
$ echo $?
0

$ cmp fruits1.txt fruits2.txt
fruits1.txt fruits2.txt differ: byte 1, line 1
$ echo $?
1

$ comm s1.txt s2.txt
apple
		banana
		cherry
	date

$ diff fruits1.txt fruits2.txt
1d0
< apple
3a3
> date

$ find . -name "*.txt" -type f
./notes.txt
./fruits1.txt
./fruits2.txt
./fruits1_copy.txt
./renamed.txt

$ grep "an" fruits1.txt
banana

$ awk '{ print NR": "$0 }' fruits1.txt
1: apple
2: banana
3: cherry

$ awk '{ s += $1 } END { print "Sum =", s }' nums.txt
Sum = 60
```

Points worth remembering:

- `cmp` exits with status `0` when the files are identical and `1` when they differ; it prints nothing at all when they match.
- `comm` requires **sorted** input. Its three columns are: lines only in the first file, lines only in the second, and lines common to both. `comm -12` shows only the common lines.
- `diff` uses `<` for lines from the first file and `>` for lines from the second.
- `grep -c` prints only the count of matching lines; `grep -i` ignores case; `grep -r` searches a whole directory tree.
- `awk` reads input line by line; `NR` is the current line number, `$0` the whole line, `$1` the first field.

#### (iii) Directory Management

| Command | Purpose | Example |
|---|---|---|
| `cd` | Changes the current working directory (`cd ..` moves to the parent) | `cd demo` |
| `mkdir` | Creates a directory (`-p` creates missing parent directories) | `mkdir -p demo/sub` |
| `rmdir` | Removes an **empty** directory only | `rmdir demo/sub` |
| `ls` | Lists directory contents (`-l` long format, `-a` includes hidden files, `-R` recursive) | `ls -l` |

```console
$ mkdir -p demo/sub
$ ls -d demo demo/sub
demo
demo/sub

$ cd demo
$ pwd
/home/nishant/os-practical/demo
$ cd ..

$ ls -R demo
demo:
sub

demo/sub:

$ rmdir demo/sub
$ rmdir demo
```

`rmdir` refuses to remove a directory that still contains files:

```console
$ rmdir demo/x
rmdir: failed to remove 'demo/x': Directory not empty
```

Use `rm -r demo/x` when the directory is not empty.

A long listing shows the file type and permissions in the first column:

```console
$ ls -l
-rw-rw-r--. 1 nishant nishant   56 Nov 24 07:15 notes.txt
drwxrwxr-x. 2 nishant nishant 4096 Nov 24 07:15 demo
```

The leading `-` marks a regular file and `d` marks a directory; the next nine characters are the owner, group and other permissions.

---

<a id="practical-3"></a>

## Practical 3 — LINUX Commands for Process Control, Communication and Protection

### Question

Execute various LINUX commands for:

1. **Process Control:** `fork`, `getpid`, `ps`, `kill`, `sleep`
2. **Communication:** Input-output redirection, Pipe
3. **Protection Management:** `chmod`, `chown`, `chgrp`

### Solution

#### (i) Process Control

| Command | Purpose | Example |
|---|---|---|
| `fork` | **System call** (not a shell command) that creates a new child process | called from a program, see below |
| `getpid` | **System call** returning the calling process's PID; `getppid` returns the parent's PID | called from a program, see below |
| `ps` | Prints a snapshot of the currently running processes | `ps -f` |
| `kill` | Sends a signal to a process; `-9` sends SIGKILL, `-l` lists signal names | `kill -9 2841` |
| `sleep` | Suspends execution for the given number of seconds | `sleep 2` |

> `fork` and `getpid` are system calls rather than shell utilities, so there is no `fork` command on the command line. They are demonstrated with a short program that calls them directly.

```console
$ ps -f
UID        PID  PPID  C STIME TTY          TIME CMD
nishant   2841  2798  0 09:02 pts/0    00:00:00 bash
nishant   3105  2841  0 09:12 pts/0    00:00:00 ps -f
```

`PID` is the process id, `PPID` the parent process id — here `ps` was started by `bash`, so `2841` is its parent.

`fork()` and `getpid()` called from Python:

```python
import os

print("parent pid =", os.getpid(), flush=True)
pid = os.fork()

if pid == 0:
    print("child  pid =", os.getpid(), " parent pid =", os.getppid(), flush=True)
    os._exit(0)
else:
    os.wait()
    print("child finished, parent pid =", os.getpid(), flush=True)
```

```console
$ python3 fork_demo.py
parent pid = 60
child  pid = 61  parent pid = 60
child finished, parent pid = 60
```

`fork()` returns `0` in the child and the child's PID in the parent, which is how the two branches are distinguished. The child's `getppid()` is the parent's PID.

```console
$ sleep 2
$ kill -l
 1) SIGHUP	 2) SIGINT	 3) SIGQUIT	 4) SIGILL	 5) SIGTRAP
 6) SIGABRT	 7) SIGBUS	 8) SIGFPE	 9) SIGKILL	10) SIGUSR1

$ kill -0 2841
$ echo $?
0
```

`kill -0` sends no signal at all — it only checks whether the process exists, returning `0` if it does. To actually terminate a process, use `kill <PID>`, or `kill -9 <PID>` to send SIGKILL, which cannot be caught or ignored.

#### (ii) Communication — redirection and pipes

| Operator | Purpose | Example |
|---|---|---|
| `>` | Redirects standard output to a file, **overwriting** it | `echo "line one" > out.txt` |
| `>>` | Redirects standard output to a file, **appending** to it | `echo "line two" >> out.txt` |
| `<` | Redirects standard input from a file | `wc -l < out.txt` |
| `\|` | Pipes the output of one command into the input of the next | `cat out.txt \| grep line \| wc -l` |

```console
$ echo "line one" > out.txt
$ cat out.txt
line one

$ echo "line two" >> out.txt
$ cat out.txt
line one
line two

$ wc -l < out.txt
2

$ cat out.txt | grep line | wc -l
2
```

With `<` the command reads the file as its standard input, so `wc -l` prints only the count and not the filename. A pipe connects two processes directly through an in-kernel buffer, so no temporary file is created — `cat out.txt | grep line | wc -l` counts the lines containing `line` without writing anything to disk.

The common redirections are `0` for standard input, `1` for standard output and `2` for standard error, so `command 2> error.log` captures only the error stream.

#### (iii) Protection Management

| Command | Purpose | Example |
|---|---|---|
| `chmod` | Changes a file's permissions, in octal or symbolic form | `chmod 755 script.sh`, `chmod u+x script.sh` |
| `chown` | Changes the owner of a file — requires root privileges | `sudo chown root script.sh` |
| `chgrp` | Changes the group of a file — requires ownership or root | `sudo chgrp root script.sh` |

Permissions are three bits for each of three classes — owner, group and others — where read is `4`, write is `2` and execute is `1`. So `755` means owner `rwx`, group `r-x`, others `r-x`.

```console
$ touch script.sh

$ chmod 644 script.sh
$ ls -l script.sh
-rw-r--r--. 1 nishant nishant 0 Nov 24 07:15 script.sh

$ chmod 755 script.sh
$ ls -l script.sh
-rwxr-xr-x. 1 nishant nishant 0 Nov 24 07:15 script.sh

$ chmod 600 script.sh
$ ls -l script.sh
-rw-------. 1 nishant nishant 0 Nov 24 07:15 script.sh
```

| Octal | Symbolic | Meaning |
|:---:|:---:|---|
| `644` | `rw-r--r--` | owner reads and writes; group and others read only |
| `755` | `rwxr-xr-x` | owner reads, writes and executes; group and others read and execute |
| `600` | `rw-------` | owner reads and writes; nobody else has any access |

The same changes can be made symbolically: `chmod u+x script.sh` adds execute permission for the owner, `chmod g-w script.sh` removes write permission from the group, and `chmod o=r script.sh` sets others to read only.

`chown` and `chgrp` need elevated privileges, and fail without them:

```console
$ chown root script.sh
chown: changing ownership of 'script.sh': Operation not permitted

$ sudo chown root script.sh
$ sudo chgrp root script.sh
```

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
- Practical 1 is an OS simulator demonstration and its output is recorded as screenshots in the practical file. Practicals 2 and 3 are Linux command sessions, documented in full above with the purpose and output of each command.
- Practical 4 uses the `multiprocessing` module to create the child process, and Practical 11 uses the `threading` module for thread creation.

---

*Practical file submitted as part of the Operating Systems (DSC08) course, B.Sc. (H) Computer Science, Semester III, Ramanujan College, University of Delhi.*
