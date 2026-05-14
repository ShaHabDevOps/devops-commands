#!/bin/bash

# Server Audit Script - Collects critical info for DevOps review
# Usage: ./server-audit.sh

OUTPUT_FILE="/tmp/server-audit-$(hostname).txt"
DIVIDER="================================================================"

# Clear previous report
> "$OUTPUT_FILE"

echo "$DIVIDER" | tee -a "$OUTPUT_FILE"
echo "SERVER AUDIT REPORT - $(date '+%Y-%m-%d %H:%M:%S %Z')" | tee -a "$OUTPUT_FILE"
echo "Hostname: $(hostname)" | tee -a "$OUTPUT_FILE"
echo "$DIVIDER" | tee -a "$OUTPUT_FILE"

# 1. OS & Kernel
echo -e "\n[OS & KERNEL]" | tee -a "$OUTPUT_FILE"
cat /etc/os-release | grep -E "PRETTY_NAME|VERSION_ID" | tee -a "$OUTPUT_FILE"
uname -r | sed 's/^/Kernel: /' | tee -a "$OUTPUT_FILE"

# 2. Hardware Specs
echo -e "\n[HARDWARE SPECS]" | tee -a "$OUTPUT_FILE"
echo "CPU: $(nproc) cores" | tee -a "$OUTPUT_FILE"
grep "model name" /proc/cpuinfo | head -1 | sed 's/model name.*: /CPU Model: /' | tee -a "$OUTPUT_FILE"
free -h | grep -E "Mem:|Swap:" | tee -a "$OUTPUT_FILE"

# 3. Disk Usage
echo -e "\n[DISK USAGE]" | tee -a "$OUTPUT_FILE"
df -h / /var /tmp 2>/dev/null | grep -v "tmpfs" | tee -a "$OUTPUT_FILE"

# 4. Uptime & Load
echo -e "\n[UPTIME & LOAD]" | tee -a "$OUTPUT_FILE"
uptime | tee -a "$OUTPUT_FILE"

# 5. Network Interfaces & IPs
echo -e "\n[NETWORK]" | tee -a "$OUTPUT_FILE"
ip -4 -br addr show | grep -v "127.0.0.1" | tee -a "$OUTPUT_FILE"

# 6. Listening Ports (Critical services only)
echo -e "\n[LISTENING PORTS]" | tee -a "$OUTPUT_FILE"
ss -tunlp | grep LISTEN | awk '{print $5, $7}' | column -t | sort -u | tee -a "$OUTPUT_FILE"

# 7. Critical Systemd Services
echo -e "\n[CRITICAL SYSTEMD SERVICES]" | tee -a "$OUTPUT_FILE"
systemctl list-units --type=service --state=running | \
  grep -E "docker|containerd|kubelet|k3s|jenkins|postgresql|mysql|redis|nginx|harbor|consul|grafana|prometheus|argocd" | \
  awk '{print $1, $3, $4}' | column -t | tee -a "$OUTPUT_FILE"

# 8. Docker/Containerd Containers (if available)
if command -v docker &> /dev/null; then
  echo -e "\n[DOCKER CONTAINERS]" | tee -a "$OUTPUT_FILE"
  docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}" 2>/dev/null | tee -a "$OUTPUT_FILE"
fi

# 9. Kubernetes Info (if kubectl available)
if command -v kubectl &> /dev/null; then
  echo -e "\n[KUBERNETES CLUSTER]" | tee -a "$OUTPUT_FILE"
  kubectl get nodes -o wide 2>/dev/null | tee -a "$OUTPUT_FILE"
  
  echo -e "\n[KUBERNETES NAMESPACES]" | tee -a "$OUTPUT_FILE"
  kubectl get ns 2>/dev/null | tee -a "$OUTPUT_FILE"
  
  echo -e "\n[KUBERNETES PODS (Critical)]" | tee -a "$OUTPUT_FILE"
  kubectl get pods -A --field-selector=status.phase!=Succeeded | \
    grep -vE "Completed|Evicted" | head -30 2>/dev/null | tee -a "$OUTPUT_FILE"
  
  echo -e "\n[KUBERNETES SERVICES]" | tee -a "$OUTPUT_FILE"
  kubectl get svc -A 2>/dev/null | head -20 | tee -a "$OUTPUT_FILE"
fi

# 10. Critical Processes (Java, K8s, Databases, Monitoring)
echo -e "\n[CRITICAL PROCESSES]" | tee -a "$OUTPUT_FILE"
ps aux --sort=-%mem | head -1 | tee -a "$OUTPUT_FILE"
ps aux --sort=-%mem | \
  grep -E "java|k3s|containerd|dockerd|postgres|mysql|redis|jenkins|harbor|consul|grafana|prometheus|argocd|traefik|coredns|loki|promtail" | \
  grep -v grep | \
  awk '{printf "%-10s %6s %6s %s\n", $1, $3"%", $4"%", $11}' | head -20 | tee -a "$OUTPUT_FILE"

# 11. Top Memory Consumers
echo -e "\n[TOP 10 MEMORY CONSUMERS]" | tee -a "$OUTPUT_FILE"
ps aux --sort=-%mem | head -11 | tail -10 | awk '{printf "%-10s %6s %6s %s\n", $1, $3"%", $4"%", $11}' | tee -a "$OUTPUT_FILE"

# 12. Database Services (if running)
if pgrep -x postgres > /dev/null 2>&1; then
  echo -e "\n[POSTGRESQL INFO]" | tee -a "$OUTPUT_FILE"
  echo "PostgreSQL is running (PID: $(pgrep -x postgres | head -1))" | tee -a "$OUTPUT_FILE"
  ss -tunlp | grep postgres | awk '{print $5}' | sed 's/.*:/Port: /' | tee -a "$OUTPUT_FILE"
fi

if pgrep -x mysqld > /dev/null 2>&1; then
  echo -e "\n[MYSQL INFO]" | tee -a "$OUTPUT_FILE"
  echo "MySQL is running (PID: $(pgrep -x mysqld | head -1))" | tee -a "$OUTPUT_FILE"
  ss -tunlp | grep mysql | awk '{print $5}' | sed 's/.*:/Port: /' | tee -a "$OUTPUT_FILE"
fi

# 13. Security - Failed Login Attempts (last 24h)
echo -e "\n[SECURITY - FAILED LOGINS (last 24h)]" | tee -a "$OUTPUT_FILE"
grep "Failed password" /var/log/auth.log 2>/dev/null | tail -5 | tee -a "$OUTPUT_FILE" || echo "No auth.log access or no failed logins" | tee -a "$OUTPUT_FILE"

echo -e "\n$DIVIDER" | tee -a "$OUTPUT_FILE"
echo "Report saved to: $OUTPUT_FILE" | tee -a "$OUTPUT_FILE"
echo "$DIVIDER" | tee -a "$OUTPUT_FILE"

# Display file location
echo ""
echo "✓ Audit complete!"
echo "✓ Report saved: $OUTPUT_FILE"
echo ""
echo "To view: cat $OUTPUT_FILE"
echo "To copy: scp $(whoami)@$(hostname -I | awk '{print $1}'):$OUTPUT_FILE ."
