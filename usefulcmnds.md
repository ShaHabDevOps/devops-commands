# DevOps CLI Reference (Ubuntu)

> Compact, terminal-first command reference for DevOps engineers. Optimized for fast scanning and GitHub README usage.

---

## Format

`command` | short purpose

---

## 1. File & Directory Operations

| Command               | Purpose                                   |
| --------------------- | ----------------------------------------- |
| `ls -lah`             | List files with permissions, size, hidden |
| `pwd`                 | Show current directory                    |
| `cd /path`            | Change directory                          |
| `mkdir -p a/b`        | Create directory tree                     |
| `rm -rf dir`          | Delete directory recursively              |
| `cp -r src dst`       | Copy recursively                          |
| `mv a b`              | Move / rename                             |
| `touch file`          | Create empty file                         |
| `stat file`           | File metadata                             |
| `file file`           | Detect file type                          |
| `ln -s src link`      | Create symlink                            |
| `du -sh *`            | Directory sizes                           |
| `df -h`               | Disk usage                                |
| `tree -L 2`           | Directory tree                            |
| `rsync -av src/ dst/` | Fast sync                                 |

---

## 2. Text & Log Processing

| Command                   | Purpose              |               |
| ------------------------- | -------------------- | ------------- |
| `cat file`                | Print file           |               |
| `less file`               | Paginated view       |               |
| `head -n 50 file`         | First lines          |               |
| `tail -F log`             | Follow rotating logs |               |
| `grep pat file`           | Search text          |               |
| `grep -Ri pat /dir`       | Recursive search     |               |
| `awk '{print $1}' file`   | Column extract       |               |
| `sed 's/a/b/g' file`      | Replace text         |               |
| `cut -d: -f1 /etc/passwd` | Field extract        |               |
| `sort                     | uniq -c`             | Count uniques |
| `wc -l file`              | Line count           |               |
| `jq '.' file.json`        | JSON view            |               |
| `yq e '.' file.yml`       | YAML view            |               |

---

## 3. Users & Permissions

| Command                 | Purpose         |
| ----------------------- | --------------- |
| `whoami`                | Current user    |
| `id`                    | UID/GID         |
| `adduser user`          | Create user     |
| `passwd user`           | Change password |
| `usermod -aG sudo user` | Grant sudo      |
| `groups user`           | Show groups     |
| `chown -R u:g path`     | Ownership       |
| `chmod 750 file`        | Permissions     |
| `visudo`                | Edit sudoers    |
| `ssh-copy-id user@host` | Push SSH key    |

---

## 4. Networking & Troubleshooting

| Command                           | Purpose           |
| --------------------------------- | ----------------- |
| `ip a`                            | IP addresses      |
| `ip r`                            | Routing table     |
| `ss -tulnp`                       | Listening ports   |
| `ping -c 5 host`                  | Connectivity test |
| `traceroute host`                 | Path trace        |
| `curl -I url`                     | HTTP headers      |
| `dig domain`                      | DNS lookup        |
| `tcpdump -i eth0`                 | Packet capture    |
| `nc -zv host 22`                  | Port test         |
| `nmap -p- host`                   | Port scan         |
| `openssl s_client -connect h:443` | TLS debug         |

---

## 5. System Monitoring

| Command        | Purpose       |
| -------------- | ------------- |
| `top`          | Live CPU/mem  |
| `htop`         | Enhanced top  |
| `free -h`      | Memory        |
| `uptime`       | Load avg      |
| `vmstat 1`     | CPU/mem stats |
| `iostat -xz 1` | Disk IO       |
| `mpstat`       | Per-CPU       |
| `df -h`        | Disk usage    |

---

## 6. Package Management (APT)

| Command            | Purpose         |
| ------------------ | --------------- |
| `apt update`       | Refresh index   |
| `apt upgrade -y`   | Upgrade system  |
| `apt install pkg`  | Install         |
| `apt remove pkg`   | Remove          |
| `apt purge pkg`    | Remove + config |
| `dpkg -l`          | Installed pkgs  |
| `dpkg -i file.deb` | Install deb     |

---

## 7. Process & Service Management

| Command                 | Purpose        |              |
| ----------------------- | -------------- | ------------ |
| `ps aux`                | Processes      |              |
| `ps -ef                 | grep x`        | Find process |
| `kill -9 pid`           | Force kill     |              |
| `pkill name`            | Kill by name   |              |
| `systemctl status svc`  | Service status |              |
| `systemctl restart svc` | Restart        |              |
| `journalctl -u svc`     | Service logs   |              |
| `journalctl -f`         | Follow logs    |              |

---

## 8. Docker

| Command                  | Purpose        |
| ------------------------ | -------------- |
| `docker ps -a`           | Containers     |
| `docker images`          | Images         |
| `docker run -d img`      | Run container  |
| `docker exec -it c bash` | Shell inside   |
| `docker logs c`          | Container logs |
| `docker stop c`          | Stop           |
| `docker rm c`            | Remove         |
| `docker rmi img`         | Delete image   |
| `docker system df`       | Disk usage     |
| `docker system prune`    | Cleanup        |

---

## 9. Kubernetes / K3s

| Command                    | Purpose        |
| -------------------------- | -------------- |
| `kubectl get nodes`        | Cluster nodes  |
| `kubectl get pods -A`      | All pods       |
| `kubectl describe pod p`   | Pod details    |
| `kubectl logs p`           | Pod logs       |
| `kubectl exec -it p -- sh` | Pod shell      |
| `kubectl apply -f yml`     | Deploy         |
| `kubectl delete -f yml`    | Remove         |
| `kubectl top pod`          | Resource usage |

---

## 10. Git (Deployment Ops)

| Command             | Purpose    |
| ------------------- | ---------- |
| `git status`        | Repo state |
| `git pull`          | Sync code  |
| `git add .`         | Stage      |
| `git commit -m msg` | Commit     |
| `git push`          | Push       |
| `git log --oneline` | History    |

---

**This document is intentionally compact.**

Next expansions (can be appended cleanly):

* Advanced networking (iptables/nftables)
* Storage (LVM, mounts)
* Jenkins CLI
* Rancher / ArgoCD ops
* Prometheus & Grafana CLI
* Failure & incident debugging playbooks
