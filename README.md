Absolutely! I can break down your **Level 1 → Level 3 assignments** into **step-by-step, easy-to-follow guidance**. I’ll keep it simple, actionable, and practical.

---

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

![alt text](<evidences/Screenshot 2025-12-01 155832.png>)

![alt text](<evidences/Screenshot 2025-12-01 160332.png>)

---

### **2️⃣ Set Permissions for Project Directory**

**Steps:**

1. Create project directory:

```bash
sudo mkdir -p /opt/projectA
```
![alt text](image.png)

2. Assign ownership to dev1 and devteam:

```bash
sudo chown -R dev1:devteam /opt/projectA
```
![alt text](<evidences/Screenshot 2025-12-01 161113.png>)
3. Set permissions (rwx for owner & group, none for others):

```bash
sudo chmod -R 770 /opt/projectA
```
![alt text](<evidences/Screenshot 2025-12-01 161343.png>)

4. Verify:

```bash
ls -ld /opt/projectA
```
![alt text](<evidences/Screenshot 2025-12-01 161628.png>)
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

sudo dnf install -y java-17-amazon-corretto-devel


```

3. Verify:

```bash
git --version
nginx -v
java --version
```

![alt text](<evidences/Screenshot 2025-12-01 163023.png>)  

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

![alt text](<evidences/Screenshot 2025-12-01 163237.png>)  


![alt text](<evidences/Screenshot 2025-12-01 163537.png>) 


![alt text](<evidences/Screenshot 2025-12-01 163647.png>)  


![alt text](<evidences/Screenshot 2025-12-01 163807.png>)  


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

![alt text](<evidences/Screenshot 2025-12-01 164424.png>)  


![alt text](<evidences/Screenshot 2025-12-01 165013.png>) 

---

### **6️⃣ Create Shell Scripts**

**a) Log Cleanup Script**

```bash
sudo vi /usr/local/bin/cleanup_logs.sh
```
paste :

```bash
#!/bin/bash
find /var/log/projectA/ -type f -mtime +7 -delete
```

Make executable:

```bash
sudo chmod +x /usr/local/bin/cleanup_logs.sh
```

![alt text](<evidences/Screenshot 2025-12-01 171006.png>) 


![alt text](<evidences/Screenshot 2025-12-01 165636.png>)  


![alt text](<evidences/Screenshot 2025-12-01 171300.png>) 


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
sudo nano /usr/local/bin/check_nginx.sh
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


![alt text](<evidences/Screenshot 2025-12-01 172653.png>) 


![alt text](<evidences/Screenshot 2025-12-01 171300.png>) 


![alt text](<evidences/Screenshot 2025-12-01 171659.png>)  


![alt text](<evidences/Screenshot 2025-12-01 172546.png>) 


![alt text](<evidences/Screenshot 2025-12-01 172834.png>)

### **7️⃣ Manage Logs**

**Steps:**

1. Create log directory:

```bash
sudo mkdir -p /var/log/projectA
```

2. Script to log CPU & Memory usage every minute:

```bash
sudo nano /usr/local/bin/log_usage.sh
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

![alt text](<evidences/Screenshot 2025-12-01 173920.png>)  


![alt text](<evidences/Screenshot 2025-12-01 173246.png>) 


![alt text](<evidences/Screenshot 2025-12-01 173624.png>) 



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

![alt text](<evidences/Screenshot 2025-12-01 174608.png>) 


![alt text](<evidences/Screenshot 2025-12-01 174912.png>) 

---

## **🟥 LEVEL 3 – Advanced (Production-Ready Linux)**

---

Here is a clear, step-by-step guide to **create a custom systemd service** on Linux (CentOS, RHEL, Rocky, Ubuntu — same steps).

---

# ✅ **How to Create a Custom systemd Service**

### **Example Goal:**

Create a service named **myapp.service** that runs a script located at:

```
/usr/local/bin/myapp.sh
```

---

# **1️⃣ Create Your Script**

Create the script you want systemd to run.

```sh
sudo vi /usr/local/bin/myapp.sh
```

Add:

```sh
#!/bin/bash
echo "MyApp service is running..." >> /var/log/myapp.log
```

Save & exit.

Make it executable:

```sh
sudo chmod +x /usr/local/bin/myapp.sh
```

---

# **2️⃣ Create the Systemd Service File**

Create the service file:

```sh
sudo vi /etc/systemd/system/myapp.service
```

Paste:

```ini
[Unit]
Description=My Custom Application Service
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/myapp.sh
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Save & exit.

---

# **3️⃣ Reload Systemd Daemon**

Whenever you add/edit a service:

```sh
sudo systemctl daemon-reload
```

---

# **4️⃣ Enable the Service (start on boot)**

```sh
sudo systemctl enable myapp.service
```

---

# **5️⃣ Start the Service**

```sh
sudo systemctl start myapp.service
```

---

# **6️⃣ Check Status**

```sh
systemctl status myapp.service
```

---

# **7️⃣ View Logs (journalctl)**

```sh
sudo journalctl -u myapp.service -f
```

---

# **8️⃣ Stop / Restart the Service**

Stop:

```sh
sudo systemctl stop myapp.service
```

Restart:

```sh
sudo systemctl restart myapp.service
```
![alt text](<evidences/Screenshot 2025-12-01 195438.png>)

![alt text](<evidences/Screenshot 2025-12-01 195904.png>)

![alt text](<evidences/Screenshot 2025-12-01 201050.png>)
---

# ✔ Your custom systemd service is now created, enabled, and running!

---


### ** - SSH Hardening**

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
![alt text](<evidences/Screenshot 2025-12-01 201125.png>)
4. Test login:

```bash
ssh -p 2222 dev1@server-ip
```

---

### **1️⃣1️⃣ LVM Setup**

Sure! I can guide you step by step on setting up **LVM (Logical Volume Management)** on Linux. I’ll provide clear commands and explanations. I assume you are using **Red Hat/CentOS/Ubuntu** type Linux.

---

## **Step 1: Check Existing Disks**

First, check the disks available on your system.

```bash
lsblk
```

or

```bash
fdisk -l
```

Identify the disks you want to use for LVM (e.g., `/dev/sdb`, `/dev/sdc`).

---

## **Step 2: Install LVM Tools (if not installed)**

```bash
# CentOS/RHEL
sudo yum install lvm2 -y

# Ubuntu/Debian
sudo apt install lvm2 -y
```

---
the `lsblk` output, I see:

* You currently have **only one disk**: `nvme0n1` (the root disk) with partitions.
* There are **no extra disks** for LVM yet.

💡 **Important:** LVM requires **unused disks or partitions**. You **cannot use the root disk** (`nvme0n1`) safely for LVM unless you are willing to destroy data.

---

## **Step 1: Attach a New Disk**

Since you are on AWS EC2 (I assume from `nvme0n1`), do this:

1. Go to the **EC2 Console → Volumes → Create Volume**

   * Choose the size (e.g., 10 GB)
   * Use the **same Availability Zone** as your instance
2. Attach it to your instance
- Verify in Linux:

lsblk


You should see something like:

nvme0n1   8G  disk
nvme1n1  10G  disk   <-- New disk


## Then proceed with LVM commands:

sudo pvcreate /dev/nvme1n1
sudo vgcreate my_vg /dev/nvme1n1
sudo lvcreate -L 10G -n my_lv my_vg
sudo mkfs.ext4 /dev/my_vg/my_lv
sudo mkdir /mnt/mydata
sudo mount /dev/my_vg/my_lv /mnt/mydata

---
![alt text](<evidences/Screenshot 2025-12-01 204610.png>)
![alt text](<evidences/Screenshot 2025-12-01 204635.png>)
![alt text](<evidences/Screenshot 2025-12-01 204701.png>)
---

✅ Now your LVM setup is ready, flexible, and can be expanded dynamically.


---

### **1️⃣2️⃣ Firewall Configuration**
Step 1: Install firewalld
sudo yum install -y firewalld


On Amazon Linux 2, you may need:

sudo amazon-linux-extras install epel -y
sudo yum install -y firewalld

Step 2: Start and enable firewalld
sudo systemctl start firewalld
sudo systemctl enable firewalld

Step 3: Open ports

After installing and starting firewalld, you can run your commands:

sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --add-port=8080/tcp --permanent
sudo firewall-cmd --reload

Step 4: Verify
sudo firewall-cmd --list-all
**Steps:**

```bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-port=8080/tcp
sudo firewall-cmd --permanent --add-port=2222/tcp
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```
![alt text](<evidences/Screenshot 2025-12-01 202228.png>)

![alt text](<evidences/Screenshot 2025-12-01 202421.png>)
---

### **1️⃣3️⃣ Logrotate for App Logs**

**Steps:**

1. Create logrotate config:

```bash
sudo vi /etc/logrotate.d/projectA
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

![alt text](<evidences/Screenshot 2025-12-01 202832.png>)
![alt text](<evidences/Screenshot 2025-12-01 203200.png>)

---

This **step-by-step guide** lets you **perform every assignment successfully**, even if you’re new to Linux.

---

If you want, I can make a **single PDF-style checklist with all commands** so you can **execute it directly on a server witking up commands**.

