#!/bin/bash
# ============================================================
#   Server Health Check — DevOps Quick Audit
#   Usage: bash server_health_check.sh
#   Output: Printed to terminal + saved to /tmp/server_audit.log
# ============================================================

OUTPUT_FILE="/tmp/server_audit_$(hostname)_$(date +%Y%m%d_%H%M%S).log"
DIVIDER="============================================================"

log() {
    echo -e "$1" | tee -a "$OUTPUT_FILE"
}

log "\n$DIVIDER"
log " SERVER HEALTH CHECK — $(date)"
log " Host: $(hostname) | User: $(whoami)"
log "$DIVIDER"

# ------------------------------------------------------------
# 1. OS & KERNEL
# ------------------------------------------------------------
log "\n[1] OS & KERNEL"
cat /etc/os-release | grep -E "PRETTY_NAME|VERSION_ID" | tee -a "$OUTPUT_FILE"
uname -r | sed 's/^/Kernel: /' | tee -a "$OUTPUT_FILE"

# ------------------------------------------------------------
# 2. CPU & LOAD
# ------------------------------------------------------------
log "\n[2] CPU & LOAD"
uptime | tee -a "$OUTPUT_FILE"
echo "" | tee -a "$OUTPUT_FILE"
top -bn1 | head -20 | tee -a "$OUTPUT_FILE"
echo "" | tee -a "$OUTPUT_FILE"
log "Top CPU processes:"
ps aux --sort=-%cpu | head -10 | tee -a "$OUTPUT_FILE"

# ------------------------------------------------------------
# 3. MEMORY
# ------------------------------------------------------------
log "\n[3] MEMORY"
free -h | tee -a "$OUTPUT_FILE"

# ------------------------------------------------------------
# 4. DISK USAGE
# ------------------------------------------------------------
log "\n[4] DISK USAGE"
df -h / /var /tmp 2>/dev/null | grep -v "tmpfs" | tee -a "$OUTPUT_FILE"
echo "" | tee -a "$OUTPUT_FILE"
log "Top 10 largest directories:"
du -sh /* 2>/dev/null | sort -rh | head -10 | tee -a "$OUTPUT_FILE"

# ------------------------------------------------------------
# 5. NETWORK INTERFACES & IPs
# ------------------------------------------------------------
log "\n[5] NETWORK INTERFACES & IPs"
ip -4 -br addr show | grep -v "127.0.0.1" | tee -a "$OUTPUT_FILE"
echo "" | tee -a "$OUTPUT_FILE"
log "Routing Table:"
ip route show | tee -a "$OUTPUT_FILE"

# ------------------------------------------------------------
# 6. LISTENING PORTS
# ------------------------------------------------------------
log "\n[6] LISTENING PORTS"
ss -tunlp | grep LISTEN | awk '{print $5, $7}' | column -t | sort -u | tee -a "$OUTPUT_FILE"

# ------------------------------------------------------------
# 7. ACTIVE CONNECTIONS
# ------------------------------------------------------------
log "\n[7] ACTIVE CONNECTIONS"
ss -s | tee -a "$OUTPUT_FILE"
echo "" | tee -a "$OUTPUT_FILE"
log "Established TCP connections:"
ss -tnp state established | tee -a "$OUTPUT_FILE"

# ------------------------------------------------------------
# 8. SERVICES & FAILED UNITS
# ------------------------------------------------------------
log "\n[8] FAILED SERVICES"
systemctl list-units --failed | tee -a "$OUTPUT_FILE"

# ------------------------------------------------------------
# 9. RECENT ERROR LOGS
# ------------------------------------------------------------
log "\n[9] RECENT SYSTEM ERRORS (last 50)"
journalctl -p err -n 50 --no-pager | tee -a "$OUTPUT_FILE"
echo "" | tee -a "$OUTPUT_FILE"
log "Kernel messages (last 20):"
dmesg | tail -20 | tee -a "$OUTPUT_FILE"

# ------------------------------------------------------------
# 10. FIREWALL
# ------------------------------------------------------------
log "\n[10] FIREWALL (UFW)"
ufw status verbose 2>/dev/null | tee -a "$OUTPUT_FILE"

# ------------------------------------------------------------
# 11. EXTERNAL CONNECTIVITY
# ------------------------------------------------------------
log "\n[11] EXTERNAL CONNECTIVITY"
log "Ping 8.8.8.8:"
ping -c 3 8.8.8.8 | tee -a "$OUTPUT_FILE"
echo "" | tee -a "$OUTPUT_FILE"
log "HTTP check (google.com):"
curl -sI https://google.com | head -5 | tee -a "$OUTPUT_FILE"

# ------------------------------------------------------------
# 12. DNS
# ------------------------------------------------------------
log "\n[12] DNS"
log "Configured nameservers:"
cat /etc/resolv.conf | tee -a "$OUTPUT_FILE"
echo "" | tee -a "$OUTPUT_FILE"
log "DNS resolution test:"
dig google.com +short | tee -a "$OUTPUT_FILE"

# ------------------------------------------------------------
# 13. SECURITY / LOGINS
# ------------------------------------------------------------
log "\n[13] RECENT LOGINS"
last -n 10 | tee -a "$OUTPUT_FILE"
echo "" | tee -a "$OUTPUT_FILE"
log "Currently logged in:"
who | tee -a "$OUTPUT_FILE"

# ------------------------------------------------------------
# 14. KUBERNETES / DOCKER (if available)
# ------------------------------------------------------------
log "\n[14] KUBERNETES / DOCKER"

if command -v kubectl &>/dev/null; then
    log "kubectl found — running checks:"
    kubectl get nodes | tee -a "$OUTPUT_FILE"
    echo "" | tee -a "$OUTPUT_FILE"
    kubectl get pods -A | grep -v Running | tee -a "$OUTPUT_FILE"
    echo "" | tee -a "$OUTPUT_FILE"
    kubectl top nodes 2>/dev/null | tee -a "$OUTPUT_FILE"
else
    log "kubectl not found — skipping."
fi

if command -v docker &>/dev/null; then
    log "docker found — stopped containers:"
    docker ps -a | grep -v "Up" | tee -a "$OUTPUT_FILE"
else
    log "docker not found — skipping."
fi

# ------------------------------------------------------------
# DONE
# ------------------------------------------------------------
log "\n$DIVIDER"
log " AUDIT COMPLETE"
log " Log saved to: $OUTPUT_FILE"
log "$DIVIDER\n"
