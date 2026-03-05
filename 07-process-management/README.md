
# Study Guide on Process Management, Monitoring, Networking, and Disk Management

## Introduction
This study guide documents and explores advanced Linux administration topics from four key sections of the Linux course: **Process Management** (Section 7), **Monitoring** (Section 8), **Networking** (Section 9), and **Disk Management** (Section 10). These concepts are interconnected and essential for DevOps engineers, system administrators, and developers. The focus is on managing running programs (processes), tracking system performance, understanding network fundamentals, and handling storage.

---

- [Section 7: Process Management (Section 7)](#Section-1-process-management-section-7)
  - [What is a Process?](#what-is-a-process)
  - [Why Manage Processes?](#why-manage-processes)
  - [Key Activities in Process Management](#key-activities-in-process-management)
    - [1. Viewing and Checking Processes](#1-viewing-and-checking-processes)
    - [2. Killing Processes](#2-killing-processes)
    - [3. Stopping and Resuming Processes](#3-stopping-and-resuming-processes)
    - [4. Prioritizing and De-Prioritizing Processes](#4-prioritizing-and-de-prioritizing-processes)
  - [Processes vs. Services](#processes-vs-services)
  - [Additional Tips](#additional-tips)

## Section 7: Process Management
Process management is a fundamental skill for Linux administrators and DevOps engineers. It involves overseeing running programs (processes) to ensure efficient use of system resources like CPU, memory, and disk. Without proper management, a single resource-intensive process can slow down or crash the entire system.

### What is a Process?
- A **process** is a running instance of a program or application that interacts with hardware via the OS.
- Examples:
  - A Python script executing on a server.
  - A web server (e.g., Apache or Nginx) serving requests.
  - Even a shell command or script while it's running.
- Processes interact with hardware (CPU, memory, disk) through the operating system (OS), which schedules resources. If one process consumes too much (e.g., a CPU-intensive AI model), it can starve others, leading to issues like slow performance or crashes.

### Why Manage Processes?
- **Resource Allocation**: The OS schedules CPU time, but it prioritizes based on usage. A process using 90% CPU might leave only 10% for others, causing failures (e.g., out-of-memory errors in a web server).
- **System Stability**: Unmanaged processes can hang the server, preventing logins or other operations.
- **Troubleshooting**: Identify and handle problematic processes quickly.

### Key Activities in Process Management
As an administrator, you perform four main activities:
1. **View/Check Processes**: List and inspect running processes.
2. **Kill Processes**: Terminate unwanted or problematic ones.
3. **Stop/Resume Processes**: Pause and restart temporarily.
4. **Prioritize/De-prioritize Processes**: Adjust CPU scheduling priority for better resource distribution.

#### 1. Viewing and Checking Processes
- Command: `ps` (Use the `ps` command to list processes).
- Common variations:
  - `ps aux`: Shows all processes with details like user, PID (Process ID), CPU%, memory%, start time, and command. This is preferred for detailed output.
  - `ps -ef`: Similar to `ps aux`, but without CPU/memory percentages. Use when you need a quick list.
- Example Output Explanation:
  - **USER**: The user who started the process (e.g., root, ubuntu).
  - **PID**: Unique Process ID (e.g., 1234). Use this to manage the process.
  - **%CPU** and **%MEM**: Resource usage percentages.
  - **COMMAND**: The program or script running (e.g., python app.py).
    - **Example**: On an EC2 instance, `ps aux` reveals processes like `systemd` (system daemon) and user commands.
- Count Processes: `ps aux | wc -l` (gives total count, e.g., 109 processes).
- Filter Processes: `ps aux | grep <process_name>` (e.g., `ps aux | grep java` to find Java processes). To exclude the grep command itself from results, use `ps aux | grep -v grep | grep <process_name>` (e.g., `ps aux | grep -v grep | grep java`). This ensures you only see actual matches.

**Interview Questions**:
- What is the difference between `ps aux` and `ps -ef`?
  - `ps aux` shows CPU and memory usage percentages, while `ps -ef` does not. Use `ps aux` for detailed resource monitoring.
- How do you find the number of running processes on a Linux system?
  - Use `ps aux | wc -l` to count them.

#### 2. Killing Processes
- **Purpose**: Terminate processes causing issues (e.g., hanging servers or resource hogging).
- **Commands**:
  - `kill <PID>`: Gracefully kills a process.
  - `kill -9 <PID>`: Forcefully kills a process (use when `kill` fails).
  - `kill -3 <PID>`: Generates thread dumps for Java applications (for debugging).
- **Example**: To kill a process with PID 4055: `kill 4055`. If unresponsive, use `kill -9 4055`.

**Interview Questions**:
- What is the difference between `kill` and `kill -9`?
  - `kill` sends a signal to gracefully terminate; `kill -9` (SIGKILL) forcefully terminates without cleanup, used when the process ignores other signals.
- How do you generate thread dumps for a Java process?
  - Use `kill -3 <PID>` to send SIGQUIT, which generates thread dumps for debugging.

#### 3. Stopping and Resuming Processes
- **Stop Temporarily**: `kill -STOP <PID>` (pauses the process).
- **Resume**: `kill -CONT <PID>` (continues from where it stopped).
- Example: Pause a process with `kill -STOP 1234`, then resume with `kill -CONT 1234`. The process remains in the list but doesn't consume resources while stopped.

**Interview Questions**:
- How do you temporarily stop a process and then resume it?
  - Use `kill -STOP <PID>` to pause and `kill -CONT <PID>` to resume.

#### 4. Prioritizing and De-Prioritizing Processes
- The OS assigns priority using a scale from -20 (highest) to 19 (lowest). Lower numbers mean higher priority.
- Command: `renice -n <priority> -p <PID>` (e.g., `renice -n 10 -p 1234` to lower priority).
- Examples:
  - Increase priority: `renice -n -5 -p 1234` (makes it more important).
  - Decrease priority: `renice -n 10 -p 1234` (reduces importance, freeing CPU for others).
- Use cautiously: Only adjust if you understand the system's processes to avoid instability.

**Interview Questions**:
- How do you change the priority of a process in Linux?
  - Use `renice -n <priority> -p <PID>`, where priority ranges from -20 (highest) to 19 (lowest).

### Processes vs. Services
- **Processes**: Standard running programs that stop when the server restarts (e.g., a Python app).
- **Services**: Background processes that auto-start on boot (e.g., web servers like Nginx). They ensure continuity.
- Manage Services with `systemctl`:
  - List: `systemctl list-units --type=service`.
  - Stop: `systemctl stop <service_name>` (e.g., `systemctl stop cron`).
  - Start: `systemctl start <service_name>` (e.g., `systemctl start cron`).
- Most modern apps (e.g., web servers) install as services for reliability.

**Interview Questions**:
- What is the difference between a process and a service in Linux?
  - A process is a running instance of a program that stops on reboot; a service is a background process that auto-starts on boot, managed via `systemctl`.

### Additional Tips
- Run processes in the background: Use `&` (e.g., `python app.py &`).
- Check background jobs: `jobs`.
- Bring to foreground: `fg <job_number>`.
- Practice on a test server (e.g., EC2 sandbox). Refer to the GitHub repo for more commands.

---