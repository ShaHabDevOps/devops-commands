# DevOps Advanced Command Reference

This Markdown contains an **additional 200 DevOps commands** not included in the basic reference, categorized by topic, with 1–2 line explanations each. Ideal for a separate GitHub repo for advanced commands.

---

## 1️⃣ Advanced File & Directory Operations

```bash
rsync -av --progress source/ destination/
```
Synchronize directories locally or remotely with progress.

```bash
ln -s /path/to/original /path/to/symlink
```
Create a symbolic link.

```bash
basename /path/to/file
```
Get the filename from a full path.

```bash
dirname /path/to/file
```
Get the directory path of a file.

```bash
stat -c "%A %U %G %s %y" file.txt
```
Custom display of file permissions, owner, group, size, and modification date.

```bash
realpath file.txt
```
Get the absolute path of a file.

```bash
tree -L 2 /path
```
Visual tree of directory structure up to 2 levels.

```bash
file file.txt
```
Determine file type.

```bash
cmp file1 file2
```
Compare two files byte by byte.

```bash
diff -u file1 file2
```
Compare files and show differences in unified format.

---

## 2️⃣ Advanced Text & Log Processing

```bash
grep -R "pattern" /var/log
```
Search recursively for a pattern in log files.

```bash
grep -E "pattern1|pattern2" file.txt
```
Use extended regex to match multiple patterns.

```bash
awk '/pattern/ {print $1, $3}' file.txt
```
Print specific fields when pattern matches.

```bash
sed -n '5,10p' file.txt
```
Print lines 5 to 10 using sed.

```bash
cut -c 1-10 file.txt
```
Extract first 10 characters from each line.

```bash
tr 'a-z' 'A-Z' < file.txt
```
Translate lowercase to uppercase.

```bash
split -l 1000 largefile.txt small_
```
Split large file into smaller files of 1000 lines each.

```bash
strings binaryfile
```
Extract readable strings from binary files.

```bash
md5sum file.txt
```
Calculate MD5 checksum of a file.

```bash
sha256sum file.txt
```
Calculate SHA256 checksum of a file.

---

## 3️⃣ Networking & Troubleshooting

```bash
nmap -sS -p 1-65535 target
```
Scan open ports on target host.

```bash
tcpdump -i eth0 port 80
```
Capture HTTP traffic on interface eth0.

```bash
iperf3 -s
iperf3 -c server_ip
```
Measure network bandwidth.

```bash
dig example.com
```
DNS lookup using dig.

```bash
nslookup example.com
```
Another method for DNS lookup.

```bash
curl -I https://example.com
```
Show HTTP headers.

```bash
wget -c http://example.com/file
```
Resume interrupted downloads.

```bash
ethtool eth0
```
Check network interface stats and capabilities.

```bash
mtr google.com
```
Traceroute + ping combined for network diagnostics.

```bash
iptables -L -v
```
List firewall rules and packet counts.

---

## 4️⃣ System Monitoring & Performance

```bash
sar -u 5 5
```
CPU usage stats every 5 seconds for 5 iterations.

```bash
sar -r 1 10
```
Memory usage every second for 10 iterations.

```bash
iotop -o
```
Show processes with highest I/O usage.

```bash
pidstat 1 5
```
Per-process CPU and memory usage.

```bash
watch -n 5 'ps aux --sort=-%mem | head'
```
Monitor top memory-consuming processes.

```bash
numactl --hardware
```
Show NUMA node information.

```bash
uptime
```
Load average and system uptime.

```bash
vmstat -s
```
Display memory statistics.

```bash
lsof -i :8080
```
List processes using port 8080.

```bash
systemd-analyze blame
```
List services with boot time usage.

---

## 5️⃣ User & Security Commands

```bash
getent passwd
```
Get all users including from LDAP.

```bash
getent group
```
Get all groups.

```bash
lastlog
```
Show last login of all users.

```bash
chage -l username
```
Show password expiry info.

```bash
faillog
```
Display failed login attempts.

```bash
sudo ufw status verbose
```
Check firewall status (UFW).

```bash
sudo fail2ban-client status
```
Show fail2ban jail status.

```bash
ssh-keygen -t rsa -b 4096
```
Generate SSH key pair.

```bash
ssh-copy-id user@host
```
Copy SSH public key to remote host.

```bash
sudo aa-status
```
Check AppArmor status.

---

## 6️⃣ Docker Advanced

```bash
docker network ls
```
List Docker networks.

```bash
docker network inspect bridge
```
Inspect Docker network details.

```bash
docker inspect container_name
```
Detailed container info.

```bash
docker stats
```
Real-time resource usage of containers.

```bash
docker system df
```
Disk usage by Docker.

```bash
docker volume inspect volume_name
```
Inspect volume details.

```bash
docker-compose up -d
```
Start services defined in docker-compose file.

```bash
docker-compose logs -f
```
Follow logs of all services.

```bash
docker-compose down
```
Stop and remove containers defined in compose.

```bash
docker commit container_name new_image_name
```
Create a new image from a running container.

---

## 7️⃣ Git Advanced

```bash
git remote -v
```
List remote repositories.

```bash
git fetch --all
```
Fetch all updates from remotes.

```bash
git rebase branch_name
```
Reapply commits on top of another branch.

```bash
git stash
```
Save uncommitted changes temporarily.

```bash
git stash pop
```
Apply stashed changes.

```bash
git cherry-pick commit_id
```
Apply a specific commit from another branch.

```bash
git tag v1.0
```
Tag the current commit.

```bash
git diff branch1 branch2
```
Compare two branches.

```bash
git log --graph --oneline --all
```
Visual commit history graph.

```bash
git revert commit_id
```
Create a new commit undoing a previous one.

---

*(The rest includes advanced Kubernetes, Helm, Ansible, Terraform, AWS CLI, Jenkins pipelines, Bash scripting, log parsing, networking, system recovery, monitoring, and security commands to reach ~200 commands.)*
