
# Networking Commands

Networking in Linux involves tools for connectivity, configuration, and troubleshooting. Below is a list of essential commands, with examples and notes.

### Connectivity and Testing
- **`ping hostname`** – Checks connectivity and latency to a remote server.
  - **Example**: `ping -c 4 google.com` (sends 4 packets).
  - **Sample Output**:
    ```
    PING google.com (8.8.8.8) 56(84) bytes of data.
    64 bytes from 8.8.8.8: icmp_seq=1 ttl=64 time=10 ms
    --- google.com ping statistics ---
    4 packets transmitted, 4 received, 0% packet loss, time 3004ms
    rtt min/avg/max/mdev = 10.000/10.000/10.000/0.000 ms
    ```
  - **Tip**: Use `-c` to limit packets; high loss indicates network issues.

- **`traceroute hostname`** – Traces the network path to a host, showing hops and latency.
  - **Example**: `traceroute google.com`.
  - **Sample Output** (abbreviated):
    ```
    traceroute to google.com (8.8.8.8), 30 hops max, 60 byte packets
     1  192.168.1.1 (192.168.1.1)  1.000 ms  1.000 ms  1.000 ms
     2  * * *
    ```
  - **Tip**: Useful for diagnosing routing problems.

### Interface and IP Configuration
- **`ifconfig`** – Displays network interfaces (deprecated; use `ip` instead).
  - **Example**: `ifconfig` (shows eth0, lo, etc.).
  - **Note**: Replaced by `ip` for modern systems.

- **`ip a`** – Shows IP addresses, interfaces, and their states.
  - **Example**: `ip a`.
  - **Sample Output** (abbreviated):
    ```
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
        link/ether 00:11:22:33:44:55 brd ff:ff:ff:ff:ff:ff
        inet 192.168.1.100/24 brd 192.168.1.255 scope global dynamic eth0
    ```
  - **Tip**: Use `ip link set eth0 up/down` to enable/disable interfaces.

### Connections and Ports
- **`netstat -tulnp`** – Displays open network connections and listening ports.
  - **Example**: `netstat -tulnp` (shows TCP/UDP listeners).
  - **Sample Output** (abbreviated):
    ```
    Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
    tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1234/sshd
    ```
  - **Note**: `ss -tulnp` is faster and preferred.

- **`ss -tulnp`** – Modern alternative to `netstat` for socket statistics.
  - **Example**: `ss -tulnp`.
  - **Sample Output** (abbreviated):
    ```
    Netid  State      Recv-Q Send-Q Local Address:Port               Peer Address:Port
    tcp    LISTEN     0      128       *:22                    *:*                   users:(("sshd",pid=1234,fd=3))
    ```
  - **Tip**: Use `-t` for TCP, `-u` for UDP, `-l` for listening.

### DNS and Name Resolution
- **`nslookup domain`** – Queries DNS for domain resolution.
  - **Example**: `nslookup example.com`.
  - **Sample Output**:
    ```
    Server:		8.8.8.8
    Address:	8.8.8.8#53

    Non-authoritative answer:
    Name:	example.com
    Address: 93.184.216.34
    ```
  - **Tip**: `dig example.com` is more detailed.

- **`hostname`** – Shows or sets the system's hostname.
  - **Example**: `hostname` (displays current name).
  - **Tip**: Use `hostnamectl set-hostname newname` for permanent changes.

### Data Transfer
- **`curl URL`** – Fetches content from a URL (supports HTTP, FTP, etc.).
  - **Example**: `curl -I https://example.com` (headers only).
  - **Sample Output**:
    ```
    HTTP/1.1 200 OK
    Content-Type: text/html
    ```
  - **Tip**: Use `-o file` to save output.

- **`wget URL`** – Downloads files from the internet.
  - **Example**: `wget https://example.com/file.zip`.
  - **Sample Output**:
    ```
    --2023-03-28 10:00:00--  https://example.com/file.zip
    Resolving example.com (example.com)... 93.184.216.34
    Connecting to example.com (example.com)|93.184.216.34|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 123456 (120K) [application/zip]
    Saving to: ‘file.zip’
    ```
  - **Tip**: Use `-c` to resume interrupted downloads.

## Troubleshooting and Best Practices
- **No Connectivity?** Check `ip a` for interface status; use `ping 8.8.8.8` to test local network.
- **Port Issues?** Run `ss -tulnp` as root; ensure firewalls (e.g., `ufw status`) allow traffic.
- **DNS Problems?** Try `nslookup` or `dig`; check `/etc/resolv.conf` for DNS servers.
- Use `ss` over `netstat` for better performance.
- Secure transfers with HTTPS; avoid plain HTTP for sensitive data.
- Test in a safe environment (e.g., Docker); monitor with `journalctl -f` for logs.
- For advanced networking, explore `iptables` or `nftables` for firewalls.

## Conclusion
These commands form the foundation of Linux networking for connectivity, configuration, and troubleshooting. Practice with examples to build confidence. For more, see [Ubuntu Networking Guide](https://help.ubuntu.com/community/Networking) or [curl Docs](https://curl.se/docs/).

---
