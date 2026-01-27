# 🔄 DevOps Tools Backup & Recovery Guide

## 📋 Overview
Comprehensive backup and recovery procedures for Jenkins, Harbor, Rancher, and ArgoCD installations.

---

## 🗂️ 1. Jenkins Backup

### **A. Complete Jenkins Backup**
```bash
#!/bin/bash
# Jenkins Complete Backup Script
BACKUP_DIR="/backup/jenkins_$(date +%Y%m%d)"
mkdir -p $BACKUP_DIR

# Backup Jenkins home directory
tar -czf $BACKUP_DIR/jenkins_home.tar.gz /var/lib/jenkins/

# Backup Jenkins configuration
cp -r /etc/jenkins $BACKUP_DIR/
cp /usr/lib/systemd/system/jenkins.service $BACKUP_DIR/ 2>/dev/null

# Backup installed plugins
if [ -d "/var/lib/jenkins/plugins" ]; then
    ls /var/lib/jenkins/plugins/ > $BACKUP_DIR/plugins_list.txt
fi

# Backup job configurations only (lightweight)
find /var/lib/jenkins/jobs -name "config.xml" -exec tar -czf $BACKUP_DIR/jenkins_jobs.tar.gz {} +

echo "✅ Jenkins backup completed: $BACKUP_DIR"
```

### B. Quick Configuration Backup
```bash
# Backup only essential configurations
tar -czf jenkins_essential_backup.tar.gz \
  /var/lib/jenkins/config.xml \
  /var/lib/jenkins/credentials.xml \
  /var/lib/jenkins/users/ \
  /var/lib/jenkins/secrets/ \
  /var/lib/jenkins/jobs/*/config.xml
```

### C. Plugin Backup
```bash
# List and backup installed plugins
java -jar /var/lib/jenkins/war/WEB-INF/jenkins-cli.jar -s http://localhost:8080/ list-plugins > plugins.txt

# Or using Jenkins CLI
curl -s "http://localhost:8080/pluginManager/api/json?depth=1" | \
  jq -r '.plugins[] | "\(.shortName):\(.version)"' > jenkins_plugins.txt
```

---

## 🐳 2. Harbor Backup

### A. Complete Harbor Backup
```bash
#!/bin/bash
# Harbor Complete Backup Script
HARBOR_BACKUP="/backup/harbor_$(date +%Y%m%d)"
mkdir -p $HARBOR_BACKUP

# Stop Harbor services
cd /opt/harbor
docker-compose down

# Backup database
docker exec -t harbor-db pg_dump -U postgres registry > $HARBOR_BACKUP/harbor_database.sql

# Backup configuration files
cp -r /opt/harbor/* $HARBOR_BACKUP/

# Backup data volume
tar -czf $HARBOR_BACKUP/harbor_data.tar.gz /data/registry/

# Backup Docker images list
docker images | grep harbor > $HARBOR_BACKUP/harbor_images.txt

# Start Harbor again
docker-compose up -d

echo "✅ Harbor backup completed: $HARBOR_BACKUP"
```

### B. Image-Level Backup
```bash
# Backup specific Harbor images
docker save -o harbor_core.tar goharbor/harbor-core:latest
docker save -o harbor_portal.tar goharbor/harbor-portal:latest
docker save -o harbor_db.tar goharbor/harbor-db:latest

# Backup all Harbor-related images
docker save $(docker images | grep harbor | awk '{print $1":"$2}') -o all_harbor_images.tar
```

### C. Registry Data Backup
```bash
# Backup registry storage
tar -czf registry_storage.tar.gz /data/registry/docker/registry/v2/

# Alternative: rsync backup
rsync -avz /data/registry/ backup-server:/backup/harbor/registry/
```

---

## 🐄 3. Rancher Backup

### A. Single Node Rancher Backup
```bash
#!/bin/bash
# Rancher Single Node Backup
RANCHER_BACKUP="/backup/rancher_$(date +%Y%m%d)"
mkdir -p $RANCHER_BACKUP

# Create backup using Rancher's built-in tool
docker stop rancher
docker create --volumes-from rancher --name rancher-data-copy rancher/rancher:latest
docker run --volumes-from rancher-data-copy -v $RANCHER_BACKUP:/backup \
  alpine tar zcvf /backup/rancher-data-backup.tar.gz /var/lib/rancher
docker rm -f rancher-data-copy
docker start rancher
```

---

## 4. ArgoCD Backup

### A. ArgoCD Complete Backup
```bash
#!/bin/bash
# ArgoCD Complete Backup
ARGOCD_BACKUP="/backup/argocd_$(date +%Y%m%d)"
mkdir -p $ARGOCD_BACKUP

# Backup ArgoCD configuration
kubectl get all -n argocd -o yaml > $ARGOCD_BACKUP/argocd_resources.yaml
kubectl get applications -n argocd -o yaml > $ARGOCD_BACKUP/applications.yaml
kubectl get projects -n argocd -o yaml > $ARGOCD_BACKUP/projects.yaml

# Backup secrets (including repositories)
kubectl get secrets -n argocd -o yaml > $ARGOCD_BACKUP/secrets.yaml

# Backup ConfigMaps
kubectl get configmaps -n argocd -o yaml > $ARGOCD_BACKUP/configmaps.yaml

# Backup Redis data (if persistent)
if kubectl get pvc -n argocd | grep -q redis; then
  kubectl cp argocd/argocd-redis-0:/data $ARGOCD_BACKUP/redis_data/
fi

echo "✅ ArgoCD backup completed: $ARGOCD_BACKUP"
```

### B. ArgoCD Application Export
```bash
# Export all applications
argocd app list -o json | jq -r '.[].metadata.name' | while read app; do
  argocd app export $app > $app.yaml
done

# Export with resources
argocd app list -o json | jq -r '.[] | "\(.metadata.name) \(.spec.destination.namespace)"' | \
  while read app namespace; do
    kubectl get all -n $namespace -l app.kubernetes.io/instance=$app -o yaml > ${app}_resources.yaml
  done
```

### C. ArgoCD Repository Backup
```bash
# Backup repository configurations
kubectl get secrets -n argocd -l argocd.argoproj.io/secret-type=repository -o yaml > repositories.yaml

# Export repository list
argocd repo list -o json > repo_list.json
```

---

## 5. Rancher Configuration Backup (Additional)
```bash
# Backup Rancher cluster and user configuration
kubectl get clusters.management.cattle.io -n local -o yaml > $RANCHER_BACKUP/clusters.yaml
kubectl get users.management.cattle.io -n local -o yaml > $RANCHER_BACKUP/users.yaml

echo "✅ Rancher backup completed: $RANCHER_BACKUP"
