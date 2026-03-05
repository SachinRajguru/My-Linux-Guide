
- [Section 8: Monitoring (Section 8)](#Section-2-monitoring-section-8)
  - [Why Monitor Systems?](#why-monitor-systems)
  - [Key Monitoring Areas](#key-monitoring-areas)
    - [CPU Monitoring](#cpu-monitoring)
    - [Memory Monitoring](#memory-monitoring)
    - [Disk Monitoring](#disk-monitoring)
  - [Advanced Monitoring](#advanced-monitoring)

# Section 8: Monitoring
Monitoring tracks system health, focusing on CPU, memory, and disk usage. It helps identify bottlenecks and prevent issues. Use these tools for quick checks or integrate with advanced systems like Prometheus and Grafana for alerts.

## Why Monitor Systems?
- Detect bottlenecks (e.g., slow performance due to resource overuse).
- Prevent crashes (e.g., out-of-memory errors).
- Enable proactive fixes (e.g., scaling resources).

## Key Monitoring Areas
- **CPU**: Check usage and core count.
- **Memory**: Free/used RAM.
- **Disk**: Storage utilization.

### CPU Monitoring
- **Commands**:
  - `nproc`: Shows CPU core count (e.g., 1 for a basic VM).
  - `top`: Real-time view of processes by CPU/memory usage. Press `q` to quit. (updates every second; sorted by highest usage).
  - `htop`: Enhanced `top` with graphs, colors, and uptime (install via `apt install htop`). 
- Example: `top` displays PID, CPU%, MEM%, and updates live.

**Interview Questions**:
- What command do you use to monitor CPU usage in real-time?
  - `top` or `htop` for real-time monitoring.

### Memory Monitoring
- **Commands**:
  - `free -h`: Human-readable memory stats (total, used, free, cached).
  - `vmstat`: System performance report (free memory, etc.).
  - `top` or `htop`: Includes memory details.
- Example: `free -h` might show 1GB total, 360MB used, 220MB free.

**Interview Questions**:
- How do you check memory usage on a Linux system?
  - Use `free -h` for a summary or `top`/`htop` for real-time details.

### Disk Monitoring
- **Commands**:
  - `df -h`: Disk usage by partition (e.g., root filesystem at 16GB, 3% used).
  - `du -sh <directory>`: Folder/file sizes (e.g., `du -sh /opt` for /opt usage).
- Example: `df -h` lists partitions; `du -sh /var/log` checks log folder size.

**Interview Questions**:
- What commands do you use to check disk usage?
  - `df -h` for partition usage and `du -sh <folder>` for folder sizes.

## Advanced Monitoring
- For production, integrate with **Prometheus** (scrapes metrics via node exporter) and **Grafana** (visual dashboards with alerts via email/Slack).
- Workflow: Linux server → Prometheus → Grafana → Alerts for issues.
- Quick Troubleshooting: Use these commands; for ongoing monitoring, use tools.
- Reference: Detailed production setup, configurations, and best practices are documented in the GitHub repository → Observability-Practices.

---