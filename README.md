

# 🚀 **DevOps Linux Assignments – Step by Step Guide**

---

## **✅ LEVEL 1 – Basic (Foundational Skills)**

---

### **1️⃣ Create Users & Groups**

**Steps:**

1. Create a group for your dev team:

```bash
sudo groupadd devteam
```

![alt text](<linux-projects/evidences/Screenshot 2025-12-01 153347.png>)

2. Create users and assign to the group:

```bash
sudo useradd -m -g devteam dev1
sudo useradd -m -g devteam dev2
sudo useradd -m -g devteam qa1
```

3. Set passwords for users:

```bash
sudo passwd dev1
sudo passwd dev2
sudo passwd qa1
```

4. Verify:

```bash
id dev1
groups dev2
```

---

### **2️⃣ Set Permissions for Project Directory**

**Steps:**

1. Create project directory:

```bash
sudo mkdir -p /opt/projectA
```

2. Assign ownership to dev1 and devteam:

```bash
sudo chown -R dev1:devteam /opt/projectA
```

3. Set permissions (rwx for owner & group, none for others):

```bash
sudo chmod -R 770 /opt/projectA
```

4. Verify:

```bash
ls -ld /opt/projectA
```

---

### **3️⃣ Install Required Packages**

**Steps:**

1. Update packages:

```bash
sudo dnf update -y
```

2. Install git, nginx, Java:

```bash
sudo dnf install -y git nginx java-17-openjdk
```

3. Verify:

```bash
git --version
nginx -v
java --version
```

---

### **4️⃣ Check System Info**

**Steps:**

```bash
# CPU info
lscpu

# Memory info
free -h

# Disk usage
lsblk
df -h

# Network info
ip a
```

4. Save system report:

```bash
free -h > /tmp/system_report.txt
df -h >> /tmp/system_report.txt
lscpu >> /tmp/system_report.txt
```

---

## **🟦 LEVEL 2 – Intermediate (Daily DevOps Tasks)**

---

### **5️⃣ Automate Backups with Cron**

**Steps:**

1. Create backup folder:

```bash
sudo mkdir -p /backup
```

2. Open crontab for current user:

```bash
crontab -e
```

3. Add cron job (backup daily at 2 AM):

```bash
0 2 * * * tar -czf /backup/projectA-$(date +\%F).tar.gz /opt/projectA
```

4. Save and exit. Verify cron:

```bash
crontab -l
ls /backup/
```

---

### **6️⃣ Create Shell Scripts**

**a) Log Cleanup Script**

```bash
sudo vi /usr/local/bin/cleanup_logs.sh
```

Paste:

```bash
#!/bin/bash
find /var/log/projectA/ -type f -mtime +7 -delete
```

Make executable:

```bash
sudo chmod +x /usr/local/bin/cleanup_logs.sh
```

**b) Service Restart Script**

```bash
sudo vi /usr/local/bin/restart_nginx.sh
```

Paste:

```bash
#!/bin/bash
systemctl restart nginx
echo "$(date) nginx restarted" >> /var/log/restart.log
```

Make executable:

```bash
sudo chmod +x /usr/local/bin/restart_nginx.sh
```

**c) Health Check Script**

```bash
sudo vi /usr/local/bin/check_nginx.sh
```

Paste:

```bash
#!/bin/bash
if ! curl -s http://localhost > /dev/null; then
  systemctl restart nginx
  echo "$(date) nginx restarted due to failure" >> /var/log/restart.log
fi
```

Make executable:

```bash
sudo chmod +x /usr/local/bin/check_nginx.sh
```

---

### **7️⃣ Manage Logs**

**Steps:**

1. Create log directory:

```bash
sudo mkdir -p /var/log/projectA
```

2. Script to log CPU & Memory usage every minute:

```bash
sudo vi /usr/local/bin/log_usage.sh
```

Paste:

```bash
#!/bin/bash
echo "$(date) CPU: $(top -bn1 | grep "Cpu(s)") MEM: $(free -h | grep Mem)" >> /var/log/projectA/usage.log
```

Make executable:

```bash
sudo chmod +x /usr/local/bin/log_usage.sh
```

3. Schedule in cron:

```bash
* * * * * /usr/local/bin/log_usage.sh
```

---

### **8️⃣ Monitor & Troubleshoot Services**

**Steps:**

```bash
# Check running processes
top

# Memory
free -h

# Disk usage
df -h

# Check nginx status
systemctl status nginx

# View logs
journalctl -u nginx
```

---

## **🟥 LEVEL 3 – Advanced (Production-Ready Linux)**

---

### **9️⃣ Create Custom Systemd Service**

**Steps:**

1. Create service file:

```bash
sudo vi /etc/systemd/system/projectA.service
```

Paste:

```ini
[Unit]
Description=ProjectA App
After=network.target

[Service]
User=dev1
ExecStart=/usr/bin/java -jar /opt/projectA/app.jar
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

2. Reload systemd & enable service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now projectA
```

3. Check status:

```bash
systemctl status projectA
```

---

### **🔟 SSH Hardening**

**Steps:**

1. Edit SSH config:

```bash
sudo vi /etc/ssh/sshd_config
```

2. Change:

```
PermitRootLogin no
PasswordAuthentication no
AllowUsers dev1 dev2
Port 2222
```

3. Restart SSH:

```bash
sudo systemctl restart sshd
```

4. Test login:

```bash
ssh -p 2222 dev1@server-ip
```

---

### **1️⃣1️⃣ LVM Setup**

**Steps:**

1. Initialize new disk:

```bash
sudo pvcreate /dev/xvdb
```

2. Create volume group:

```bash
sudo vgcreate app_vg /dev/xvdb
```

3. Create logical volume:

```bash
sudo lvcreate -L 5G -n data_lv app_vg
```

4. Format & mount:

```bash
sudo mkfs.ext4 /dev/app_vg/data_lv
sudo mkdir -p /mnt/data
sudo mount /dev/app_vg/data_lv /mnt/data
```

5. Add to `/etc/fstab` for auto-mount:

```bash
/dev/app_vg/data_lv /mnt/data ext4 defaults 0 0
```

6. Verify:

```bash
lsblk
df -h
```

---

### **1️⃣2️⃣ Firewall Configuration**

**Steps:**

```bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-port=8080/tcp
sudo firewall-cmd --permanent --add-port=2222/tcp
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

---

### **1️⃣3️⃣ Logrotate for App Logs**

**Steps:**

1. Create logrotate config:

```bash
sudo nano /etc/logrotate.d/projectA
```

Paste:

```
/var/log/projectA/*.log {
    daily
    rotate 10
    compress
    missingok
    notifempty
    postrotate
        systemctl restart projectA
    endscript
}
```

2. Test logrotate:

```bash
sudo logrotate -d /etc/logrotate.d/projectA
```

---

This **step-by-step guide** lets you **perform every assignment successfully**, even if you’re new to Linux.

---

.

Do you want me to this
