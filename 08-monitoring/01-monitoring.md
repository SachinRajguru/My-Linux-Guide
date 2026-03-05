
# Linux System Monitoring

## Introduction to System Monitoring
Monitoring system resources is essential to ensure optimal performance, detect issues, and troubleshoot problems in Linux. Various tools allow us to monitor CPU, memory, disk usage, network activity, and running processes. Tools range from real-time viewers to historical analyzers.

**Key Concepts**:
- **Load Average**: System load (from `uptime`); values >1 indicate overload.
- **I/O Wait**: Time CPU waits for I/O (visible in `top` as %wa).

## Index of Commands Covered

### CPU and Memory Monitoring
- `top` – Real-time system monitoring
- `htop` – Interactive process viewer (install with `apt install htop`)
- `vmstat` – Report system performance statistics
- `free -m` – Show memory usage in MB
- `uptime` – Show system load and uptime

### Disk Monitoring
- `df -h` – Check disk space usage
- `du -sh /path` – Show disk usage of a specific directory
- `iostat` – Display CPU and disk I/O statistics (install `sysstat`)

### Network Monitoring
- `ip a` – Show network interface details
- `ss -tulnp` – Show active connections and listening ports
- `ping hostname` – Test network connectivity
- `traceroute hostname` – Show network path to a host
- `nslookup domain` – Get DNS resolution details

### Log Monitoring
- `tail -f /var/log/syslog` – Live monitoring of system logs
- `journalctl -f` – Live system logs for systemd-based distros
- `dmesg | tail` – View kernel logs

### Advanced Monitoring
- `sar -u 1 5` – Historical CPU stats (install `sysstat`)

## CPU and Memory Monitoring
### Using `top`
To view real-time CPU and memory usage:
```bash
top
```
- **Sample Output** (abbreviated):
  ```
  top - 10:00:00 up 1 day,  1:00,  2 users,  load average: 0.50, 0.40, 0.30
  Tasks: 100 total,   1 running,  99 sleeping,   0 stopped,   0 zombie
  %Cpu(s):  5.0 us,  2.0 sy,  0.0 ni, 92.0 id,  1.0 wa,  0.0 hi,  0.0 si,  0.0 st
  MiB Mem :   4096.0 total,   2048.0 free,   1024.0 used,   1024.0 buff/cache
  ```
- Press `q` to quit; `1` for per-CPU stats.

### Using `htop`
A user-friendly alternative:
```bash
htop
```
- Use arrow keys to navigate and `F9` to kill processes.

### Using `vmstat`
To check CPU, memory, and I/O stats:
```bash
vmstat 1 5  # Update every 1 sec, show 5 updates
```
- **Sample Output**:
  ```
  procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
   r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
   1  0      0 2048000 102400 1024000    0    0     0     0  100  200  5  2 92  1  0
  ```

### Checking Memory Usage
```bash
free -m
```
- **Sample Output**:
  ```
                total        used        free      shared  buff/cache   available
  Mem:            4096        1024        2048         128        1024        3072
  Swap:           2048           0        2048
  ```

### Checking System Load
```bash
uptime
```
- **Sample Output**: `10:00:00 up 1 day, 1:00, 2 users, load average: 0.50, 0.40, 0.30`

## Disk Monitoring
### Using `df`
Check available disk space:
```bash
df -h
```
- **Sample Output**:
  ```
  Filesystem      Size  Used Avail Use% Mounted on
  /dev/sda1        50G   20G   28G  42% /
  ```

### Using `du`
Find the size of a directory:
```bash
du -sh /var/log
```
- **Sample Output**: `1.2G	/var/log`

### Using `iostat`
Check disk and CPU usage (install `sysstat` if needed):
```bash
iostat
```
- **Sample Output**:
  ```
  avg-cpu:  %user   %nice %system %iowait  %steal   %idle
             5.00    0.00    2.00    1.00    0.00   92.00

  Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
  sda               10.00        50.00       100.00     50000     100000
  ```

## Network Monitoring
### Checking Network Interfaces
```bash
ip a  # Show IP addresses and interfaces
```
- **Sample Output** (abbreviated):
  ```
  1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
      link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
      inet 127.0.0.1/8 scope host lo
  2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
      link/ether 00:11:22:33:44:55 brd ff:ff:ff:ff:ff:ff
      inet 192.168.1.100/24 brd 192.168.1.255 scope global dynamic eth0
  ```

### Viewing Open Ports and Connections
```bash
ss -tulnp  # Show listening ports (modern alternative to netstat)
```
- **Sample Output** (abbreviated):
  ```
  Netid  State      Recv-Q Send-Q Local Address:Port               Peer Address:Port
  tcp    LISTEN     0      128       *:22                    *:*                   users:(("sshd",pid=1234,fd=3))
  ```

### Testing Connectivity
```bash
ping -c 4 google.com  # Test internet connection (4 packets)
traceroute google.com  # Trace the path to Google
```
- **Ping Sample**: `64 bytes from 8.8.8.8: icmp_seq=1 ttl=64 time=10 ms`

### Checking DNS Resolution
```bash
nslookup example.com
```
- **Sample Output**:
  ```
  Server:		8.8.8.8
  Address:	8.8.8.8#53

  Non-authoritative answer:
  Name:	example.com
  Address: 93.184.216.34
  ```

## Log Monitoring
### Live Monitoring of System Logs
```bash
tail -f /var/log/syslog  # Follow logs in real-time (non-systemd)
journalctl -f  # Systemd logs
```
- **Sample Output** (journalctl):
  ```
  Mar 28 10:00:00 host systemd[1]: Started Network Manager.
  ```

### Checking Kernel Logs
```bash
dmesg | tail
```
- **Sample Output**:
  ```
  [  123.456789] usb 1-1: new high-speed USB device number 2 using ehci-pci
  ```

## Advanced Monitoring
### Historical Stats with `sar`
Collect and view historical CPU stats (install `sysstat`):
```bash
sar -u 1 5  # CPU usage every 1 sec, 5 times
```
- **Sample Output**:
  ```
  10:00:00 AM     CPU     %user     %nice   %system   %iowait    %steal     %idle
  10:00:01 AM     all      5.00      0.00      2.00      1.00      0.00     92.00
  ```

## Troubleshooting and Best Practices
- **High CPU?** Use `top` to identify processes; renice or kill if needed.
- **Low Memory?** Check `free -m`; clear cache with `echo 3 > /proc/sys/vm/drop_caches` (as root).
- **Disk Full?** Run `du -sh /*` to find large dirs; clean logs in `/var/log`.
- **Network Issues?** Use `ping` and `traceroute`; check `ip a` for interface status.
- Install tools like `htop` or `sysstat` for advanced monitoring.
- Set up alerts with tools like `monit` or integrate with Prometheus for production.
- Monitor regularly; automate with cron (e.g., `*/5 * * * * df -h >> /var/log/disk_usage.log`).
- Test in a safe environment (e.g., Docker container) before production.

## Conclusion
System monitoring is crucial for maintaining Linux performance. Tools like `top`, `df`, `ss`, and `journalctl` provide real-time and historical insights. For more, see [Ubuntu Monitoring Guide](https://help.ubuntu.com/community/SystemMonitoring) or [sysstat Docs](https://github.com/sysstat/sysstat).

---
