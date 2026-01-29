# TROUBLESHOOTING (Production)

## Real-world Debugging

| Command                           | Purpose              |
| --------------------------------- | -------------------- |
| `journalctl -xe`                  | Recent critical logs |
| `journalctl -u svc --since today` | Service logs         |
| `dmesg -T`                        | Kernel messages      |
| `strace -p pid`                   | Trace syscalls       |
| `lsof -i :80`                     | Who uses port 80     |
| `lsof -p pid`                     | Open files           |
| `free -m && swapon --show`        | Memory/swap          |
| `vmstat 1`                        | Pressure analysis    |
| `uptime`                          | Load check           |
| `ps aux --sort=-%cpu`             | CPU hogs             |
| `watch -n1 ss -s`                 | Socket pressure      |
| `curl -vk svc`                    | App reachability     |
| `kubectl logs --previous p`       | CrashLoop logs       |
