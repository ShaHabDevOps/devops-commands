# Server Audit & Activity Commands

This document lists practical commands to review what has been done on a Linux server, including user actions, file changes, services, and configurations.

---

## 1️⃣ Check Bash History

### Commands
```bash
history
HISTTIMEFORMAT="%F %T " history
sudo cat /root/.bash_history
sudo cat /home/*/.bash_history
```
**Explanation:** Shows previously executed commands by users; the second command includes timestamps. Useful to see what seniors ran.

---

## 2️⃣ Recently Executed Commands (System-Wide)

### Commands
```bash
sudo journalctl | grep -i sudo
sudo journalctl _COMM=sudo
```
**Explanation:** Displays all commands run with `sudo`. Helps track admin-level actions.

---

## 3️⃣ Recently Modified Files

### Commands
```bash
sudo find / -type f -mtime -1 2>/dev/null
sudo find / -type f -mtime -7 2>/dev/null
sudo find /etc -type f -mtime -7
```
**Explanation:** Finds files modified in the last day or week, including configs under `/etc`.

---

## 4️⃣ Recently Created Files & Folders

### Commands
```bash
sudo find / -type f -ctime -7 2>/dev/null
sudo find / -type d -ctime -7 2>/dev/null
```
**Explanation:** Lists files and directories created in the last 7 days.

---

## 5️⃣ Inspect Important Directories

### Commands
```bash
ls -lah /opt
ls -lah /srv
ls -lah /var/www
ls -lah /var/lib
ls -lah /home
```
**Explanation:** Check common locations where custom apps, services, and scripts are installed.

---

## 6️⃣ Services Installed & Modified

### Commands
```bash
systemctl list-units --type=service --state=running
systemctl list-unit-files --state=enabled
sudo journalctl --since "7 days ago" | grep -i restart
```
**Explanation:** Shows active services, those enabled at boot, and recently restarted services.

---

## 7️⃣ Installed Packages

### Commands
```bash
grep " install " /var/log/dpkg.log
dpkg -l
```
**Explanation:** Lists software installed recently or all installed packages.

---

## 8️⃣ Cron Jobs

### Commands
```bash
sudo crontab -l
crontab -l
sudo ls -lah /etc/cron*
```
**Explanation:** Lists scheduled tasks for system and users.

---

## 9️⃣ Users & Access

### Commands
```bash
cat /etc/passwd
cat /etc/group
ls -lah /home/*/.ssh
sudo cat /root/.ssh/authorized_keys
```
**Explanation:** Shows system users, groups, and SSH key access.

---

## 🔟 SSH Login History

### Commands
```bash
last
sudo lastb
sudo journalctl -u ssh
```
**Explanation:** Shows login sessions and failed login attempts.

---

## 1️⃣1️⃣ Running Processes & Open Ports

### Commands
```bash
ps aux
ps auxf
ss -tulnp
```
**Explanation:** Lists running processes, process tree, and network listening ports.

---

## 1️⃣2️⃣ Docker / Containers (if used)

### Commands
```bash
docker ps -a
docker images
docker volume ls
```
**Explanation:** Lists Docker containers, images, and volumes.

---

## 1️⃣3️⃣ Kubernetes / K3s (if used)

### Commands
```bash
kubectl get all -A
kubectl get pods -A -o wide
kubectl get svc -A
```
**Explanation:** Shows all resources in Kubernetes cluster across all namespaces.

---

## 1️⃣4️⃣ Git Repositories

### Commands
```bash
sudo find / -type d -name ".git" 2>/dev/null
```
**Explanation:** Finds all Git repositories on the server.

---

## 1️⃣5️⃣ Logs

### Commands
```bash
ls -lah /var/log
sudo tail -n 100 /var/log/syslog
sudo tail -n 100 /var/log/auth.log
```
**Explanation:** Checks system and authentication logs for actions performed.

---

## 1️⃣6️⃣ Audit Logs (optional, advanced)

### Commands
```bash
sudo ausearch -x sudo
```
**Explanation:** Shows audit logs for sudo commands if Linux auditd is enabled.

---

> **Pro tip:** Run these commands step by step on each server. Combine outputs into a consolidated note for tracking historical actions by admins or seniors.
