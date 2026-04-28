# Linux Commands Cheat Sheet
> DevOps Toolkit — Process Management · File System · Networking

---

## Process Management

| Command | Usage |
|---|---|
| `ps aux` | List all running processes with CPU/memory usage |
| `top` | Live view of processes sorted by resource usage |
| `htop` | Interactive process viewer (prettier than top) |
| `kill <PID>` | Terminate a process by its PID |
| `kill -9 <PID>` | Force-kill an unresponsive process |
| `pkill nginx` | Kill all processes matching a name |
| `jobs` | List background jobs in current shell |
| `bg %1` | Resume job 1 in the background |
| `fg %1` | Bring job 1 to the foreground |
| `nohup ./script.sh &` | Run script that survives terminal close |
| `systemctl status nginx` | Check service status (systemd) |
| `systemctl restart nginx` | Restart a service |
| `journalctl -xe` | View recent system logs with full context |
| `journalctl -u nginx -f` | Follow logs for a specific service live |

---

## File System

| Command | Usage |
|---|---|
| `ls -lah` | List files with sizes in human-readable format |
| `find / -name "*.log" 2>/dev/null` | Find all .log files, suppress errors |
| `du -sh *` | Show disk usage of each item in current dir |
| `df -h` | Show disk space usage of all mounted filesystems |
| `tail -f /var/log/syslog` | Follow a log file in real time |
| `grep -r "error" /var/log/` | Search for "error" recursively in logs |
| `cat /etc/os-release` | Show Linux distro info |
| `chmod 755 script.sh` | Set file permissions (rwxr-xr-x) |
| `chown user:group file` | Change file owner and group |
| `ln -s /path/to/file link` | Create a symbolic link |
| `tar -czvf archive.tar.gz dir/` | Compress a directory into tar.gz |
| `tar -xzvf archive.tar.gz` | Extract a tar.gz archive |

---

## Networking Troubleshooting

| Command | Usage |
|---|---|
| `ping google.com` | Test basic connectivity to a host |
| `ip addr` | Show all network interfaces and IP addresses |
| `ip route` | Show routing table (default gateway, etc.) |
| `curl -I https://example.com` | Fetch HTTP headers only — debug response codes |
| `curl -v https://example.com` | Verbose curl — full request/response detail |
| `dig google.com` | DNS lookup — see what IP a domain resolves to |
| `nslookup google.com` | Alternative DNS lookup tool |
| `ss -tuln` | Show open ports and listening sockets |
| `netstat -an` | List all active connections and ports |
| `traceroute google.com` | Trace the network path to a host |
| `wget https://example.com/file` | Download a file from a URL |

---

## Quick Reference — When Things Break

```
Service down?     → systemctl status <service>
                  → journalctl -xe

High CPU/RAM?     → top or htop
                  → ps aux --sort=-%cpu | head

Can't connect?    → ping <host>          # Is it reachable?
                  → curl -I <url>        # Is the service responding?
                  → dig <domain>         # Is DNS resolving?
                  → ss -tuln             # Is the port open?

Disk full?        → df -h               # Which filesystem?
                  → du -sh /*           # What's eating space?

Find a log?       → journalctl -u <service> -f
                  → tail -f /var/log/syslog
                  → grep -r "error" /var/log/
```

---

*Built on Day 03 of #90DaysOfDevOps · #TrainWithShubham*
