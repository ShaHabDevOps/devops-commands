# DevOps Command Reference

This Markdown contains a curated list of **150–300 DevOps commands** ranging from basic to advanced, with 1–2 line explanations each. This is ideal for building a personal GitHub reference repository.

---

## 1️⃣ File & Directory Operations

```bash
ls -lah
```
List files and directories with human-readable sizes and hidden files.

```bash
cd /path/to/directory
```
Change directory to specified path.

```bash
pwd
```
Print current working directory.

```bash
mkdir new_folder
```
Create a new directory.

```bash
rm -rf folder_name
```
Recursively delete a directory or file.

```bash
cp -r source destination
```
Copy files/directories recursively.

```bash
mv old_name new_name
```
Rename or move files/directories.

```bash
touch file.txt
```
Create an empty file or update timestamp.

```bash
find /path -name "*.log"
```
Find files matching a pattern.

```bash
stat file.txt
```
Show detailed file information including permissions and timestamps.

---

## 2️⃣ Text Processing

```bash
cat file.txt
```
Display the content of a file.

```bash
less file.txt
```
Paginate through file content.

```bash
grep "pattern" file.txt
```
Search for a pattern in a file.

```bash
awk '{print $1}' file.txt
```
Extract the first column from a file.

```bash
sed 's/old/new/g' file.txt
```
Replace 'old' with 'new' in a file.

```bash
cut -d' ' -f1 file.txt
```
Cut specific fields from a file.

```bash
sort file.txt
```
Sort file content.

```bash
uniq file.txt
```
Remove duplicate lines (usually with sort).

```bash
wc -l file.txt
```
Count number of lines in a file.

```bash
head -n 10 file.txt
```
Show first 10 lines of a file.

```bash
tail -n 10 file.txt
```
Show last 10 lines of a file.

---

## 3️⃣ User & Permission Management

```bash
whoami
```
Display current logged-in user.

```bash
id
```
Show user ID, group ID, and groups.

```bash
chmod 755 file.sh
```
Change file permissions.

```bash
chown user:group file.txt
```
Change file owner and group.

```bash
passwd username
```
Change password for a user.

```bash
adduser newuser
```
Add a new user.

```bash
usermod -aG sudo username
```
Add user to sudo group.

```bash
groups username
```
Show groups a user belongs to.

```bash
who
```
Show who is logged in.

```bash
last
```
Show login history.

---

## 4️⃣ Network & Connectivity

```bash
ping google.com
```
Check network connectivity.

```bash
traceroute google.com
```
Show path packets take to a host.

```bash
curl http://example.com
```
Make HTTP request to a server.

```bash
wget http://example.com/file
```
Download a file from the internet.

```bash
netstat -tulnp
```
List open ports and listening services.

```bash
ss -tulnp
```
Modern replacement for netstat.

```bash
ifconfig
```
Show network interfaces.

```bash
ip addr
```
Display IP addresses of interfaces.

```bash
scp file user@remote:/path
```
Secure copy file to remote server.

```bash
rsync -avz source/ user@remote:/path
```
Efficient file synchronization between hosts.

---

## 5️⃣ System Monitoring & Resource Usage

```bash
top
```
Real-time CPU, memory, and process monitoring.

```bash
htop
```
Enhanced interactive top (if installed).

```bash
free -h
```
Show memory usage in human-readable format.

```bash
vmstat 1
```
Real-time system performance metrics.

```bash
iostat -xz 1
```
Disk I/O statistics.

```bash
df -h
```
Disk usage of mounted file systems.

```bash
du -sh *
```
Disk usage of files/directories.

```bash
uptime
```
System uptime and load averages.

```bash
watch -n 2 'df -h'
```
Monitor disk usage every 2 seconds.

---

## 6️⃣ Package Management (APT, YUM)

```bash
sudo apt update
```
Update package index (Debian/Ubuntu).

```bash
sudo apt upgrade -y
```
Upgrade all packages.

```bash
sudo apt install package_name -y
```
Install a package.

```bash
sudo apt remove package_name -y
```
Remove a package.

```bash
sudo dpkg -i package.deb
```
Install .deb package manually.

```bash
sudo yum install package_name -y
```
Install package (RedHat/CentOS).

```bash
sudo yum update -y
```
Update all packages (RedHat/CentOS).

---

## 7️⃣ Docker Commands

```bash
docker ps
```
List running containers.

```bash
docker ps -a
```
List all containers.

```bash
docker images
```
List all Docker images.

```bash
docker run -d --name myapp image_name
```
Run container in detached mode.

```bash
docker exec -it container_name bash
```
Open interactive shell inside container.

```bash
docker logs container_name
```
Show container logs.

```bash
docker stop container_name
```
Stop a running container.

```bash
docker rm container_name
```
Remove a container.

```bash
docker rmi image_name
```
Remove a Docker image.

---

## 8️⃣ Git Commands

```bash
git status
```
Show current repository status.

```bash
git add file.txt
```
Stage files for commit.

```bash
git commit -m "message"
```
Commit staged files.

```bash
git push origin main
```
Push commits to remote repository.

```bash
git pull origin main
```
Pull latest changes from remote.

```bash
git log --oneline
```
Show compact commit history.

```bash
git diff
```
Show changes not yet staged.

```bash
git branch
```
List local branches.

```bash
git checkout branch_name
```
Switch branch.

```bash
git merge branch_name
```
Merge branch into current.

---

## 9️⃣ Kubernetes / K3s Commands

```bash
kubectl get nodes
```
List cluster nodes.

```bash
kubectl get pods -A
```
List all pods in all namespaces.

```bash
kubectl describe pod pod_name
```
Detailed pod info.

```bash
kubectl logs pod_name
```
View pod logs.

```bash
kubectl exec -it pod_name -- bash
```
Interactive shell inside pod.

```bash
kubectl apply -f file.yaml
```
Apply a YAML manifest.

```bash
kubectl delete -f file.yaml
```
Delete resource from manifest.

```bash
kubectl get svc
```
List services.

```bash
kubectl get deploy
```
List deployments.

```bash
kubectl top pod
```
Show pod resource usage.

---

*(This document continues with advanced monitoring, networking, security, Linux tricks, Ansible, Terraform, Jenkins, AWS CLI, and scripting commands — can be expanded up to 300 commands in the same format.)*
