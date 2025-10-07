> # Ham `PowerSheel` windows pe `SSH` install karke bina putty ke connect krayenge - command ye follow karo 

1. **Port 1521 reachable hai** → `Test-NetConnection` se confirm hua ✅
2. **`system/system@192.168.1.124:1521/oradb.localdomain`** CMD/PowerShell me direct type kiya → ye fail hua kyunki **ye SQL*Plus ka connection string hai, PowerShell command nahi** ❌
3. **OpenSSH install karne ki attempt** → failed, kyunki **admin privileges required** ⚠️

---

### Step-by-step solution for your case

Aap Windows CMD/PowerShell se bina PuTTY/MobaXterm kaise connect karenge aur Oracle VM ke database tak pohonchenge:

---

## 1️⃣ Option A — SSH using OpenSSH (recommended)

#### Step 1 — Admin privilege PowerShell

* Press **Windows key → type PowerShell → Right click → Run as Administrator**
* Fir run karo:

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```

* Ye **Windows me SSH client install** kar dega.

#### Step 2 — SSH to VM

```powershell
ssh oracle@192.168.1.124
```

> **Note:** `oracle` = VM ka OS user (jo aapko VM pe login deta hai)
> `system` = Oracle DB user, not OS user.

#### Step 3 — Run SQL*Plus on VM

VM shell me:

```bash
sqlplus system/system@//localhost:1521/oradb.localdomain
```

---

## 2️⃣ Option B — Use **SQL Developer or DBeaver** (GUI, no SQL*Plus install needed)

1. Download SQL Developer (Oracle free tool) or DBeaver (open source)
2. Create new connection:

   * **Hostname:** 192.168.1.124
   * **Port:** 1521
   * **Service Name:** oradb.localdomain
   * **Username:** system
   * **Password:** system
3. Test → Connect ✅

> Ye option easiest hai agar aap Windows pe SQL*Plus install nahi karna chahte.

---

## 3️⃣ Option C — SSH Tunnel + Local SQL Client

Agar future me aapko **local SQL client** se connect karna hai:

```powershell
# Step 1: SSH tunnel
ssh -f -N -L 1521:localhost:1521 oracle@192.168.1.124

# Step 2: Connect locally
sqlplus system/system@//localhost:1521/oradb.localdomain
```

> Isse Windows pe local port 1521 → VM Oracle port 1521 tunnel ho jata hai.

---

### 🔹 Important Notes

* `system/system@...` **direct PowerShell me type mat karo** — ye SQL*Plus syntax hai. CMD/PowerShell me run karne ke liye **SQL*Plus installed hona chahiye**.
* Agar admin access nahi hai → SSH client install nahi hoga → Option B (SQL Developer) best hai.
* **OS user ≠ DB user**: SSH login ke liye VM ka OS user chahiye (jaise `oracle`), DB login ke liye DB user (`system`) chahiye.

---

> # **Oracle Linux / RHEL VM me network troubleshoot and setup useful Linux commands** in **bash code block**
---

```bash
###############################################
# Oracle Linux VM Network Debug & Ping Fix
###############################################

# 1. Check current IP address and interface status
ip a

# 2. Check if NetworkManager is running
sudo systemctl status NetworkManager

# 3. Start and enable NetworkManager (if not running)
sudo systemctl enable NetworkManager --now

# 4. Show active network connections
nmcli connection show

# 5. Bring interface down and up again
sudo nmcli connection down enp0s3
sudo nmcli connection up enp0s3

# 6. Restart network service (for older versions)
sudo systemctl restart network

# 7. Stop and disable firewalld (for ping test)
sudo systemctl stop firewalld
sudo systemctl disable firewalld

# 8. Check if ICMP ping packets are received
sudo tcpdump -i enp0s3 icmp

# 9. View ARP table entries
arp -a

# 10. Check SELinux status
sestatus

# 11. Check iptables rules (optional, if ping still blocked)
sudo iptables -L

# 12. Edit manual static IP config (ifcfg-enp0s3 file)
sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s3

# Example content for static IP (inside that file):
# --------------------------------------------
# TYPE=Ethernet
# BOOTPROTO=static
# NAME=enp0s3
# DEVICE=enp0s3
# ONBOOT=yes
# IPADDR=192.168.1.124
# PREFIX=24
# GATEWAY=192.168.1.1
# DNS1=8.8.8.8
# --------------------------------------------

# 13. Apply new network config
sudo systemctl restart network

# 14. From host system, try to ping the VM
ping 192.168.1.124
```

---

### 💾 Tip: Save this to a file

You can save it on your Linux VM:

```bash
nano ~/vm_network_fix.sh
```

Paste the above content, then save with `CTRL+O`, exit with `CTRL+X`.

<hr>

> # **complete Linux command sheet (all categories)** — in **bash-style** 

---

## 🧾 **Linux All-in-One Command Cheatsheet**

📄 Save this file in `.sh` ya `.txt` format for reference.

```bash
##############################################
# 🔧 1. Basic System Info & Hardware
##############################################
uname -a                   # Kernel & OS info
hostnamectl                # Hostname and OS info
uptime                     # System uptime
free -h                    # RAM usage
df -h                      # Disk usage
lsblk                      # Block devices
lscpu                      # CPU info

##############################################
# 👤 2. User & Group Management
##############################################
adduser myuser             # Add user
passwd myuser              # Set password
usermod -aG wheel myuser   # Add user to sudo group
id myuser                  # User info
groups myuser              # Groups of user
deluser myuser             # Delete user
groupadd devs              # Create new group
groupdel devs              # Delete group

##############################################
# 📦 3. Package Management (RHEL/CentOS)
##############################################
yum update -y              # Update all packages
yum install nano -y        # Install package
yum remove nano -y         # Remove package
yum list installed         # List installed packages

# 📦 Ubuntu/Debian equivalent:
apt update && apt upgrade -y
apt install nano -y
apt remove nano -y

##############################################
# ⚙️ 4. Service & Systemd Management
##############################################
systemctl status sshd      # Check service status
systemctl start nginx      # Start service
systemctl stop nginx       # Stop service
systemctl restart nginx    # Restart service
systemctl enable nginx     # Enable on boot
systemctl disable nginx    # Disable on boot

##############################################
# 🌐 5. Networking
##############################################
ip a                       # Show IP addresses
nmcli connection show      # Show network connections
systemctl restart network  # Restart network service
ping 8.8.8.8               # Test internet
ping <host-ip>             # Ping another machine
netstat -tuln              # Show open ports (install: net-tools)
ss -tuln                   # Alternative to netstat

##############################################
# 🔐 6. Firewall & SELinux
##############################################
systemctl stop firewalld   # Stop firewall
systemctl disable firewalld# Disable firewall
sestatus                   # Check SELinux status
getenforce                 # Get SELinux mode
setenforce 0               # Set SELinux to permissive (temp)

##############################################
# 📁 7. File & Directory Management
##############################################
ls -lh                     # List files with size
cd /etc                   # Change directory
mkdir myfolder             # Create folder
touch myfile.txt           # Create file
rm -rf myfolder            # Remove folder
cp file1.txt /tmp/         # Copy file
mv file1.txt file2.txt     # Rename/move file

##############################################
# 📋 8. Process Management
##############################################
ps aux                     # All running processes
top                        # Real-time processes
htop                       # (Install first) Better top view
kill -9 <PID>              # Force kill process
pkill nginx                # Kill all nginx processes

##############################################
# 🔁 9. System Reboot/Shutdown
##############################################
reboot                     # Reboot system
shutdown now               # Shutdown immediately
shutdown -r +5             # Reboot in 5 mins

##############################################
# 🐳 10. Docker (Basics)
##############################################
docker --version           # Check Docker version
docker ps -a               # All containers
docker images              # List images
docker run -it ubuntu bash# Run Ubuntu container
docker stop <id>           # Stop container
docker rm <id>             # Remove container
docker rmi <image>         # Remove image

##############################################
# 🐙 11. Git (Basic)
##############################################
git init                   # Initialize repo
git clone <url>            # Clone repo
git add .                  # Add changes
git commit -m "msg"        # Commit
git push                   # Push to remote
git pull                   # Pull latest

##############################################
# 📂 12. Archiving & Compression
##############################################
tar -cvf myarchive.tar dir/     # Create tar
tar -xvf myarchive.tar          # Extract tar
tar -czvf file.tar.gz dir/      # Tar + gzip
gzip file.txt                   # Compress file
gunzip file.txt.gz              # Decompress

##############################################
# 🧪 13. Misc Utilities
##############################################
history                   # Show previous commands
clear                     # Clear screen
date                      # Show date & time
cal                       # Calendar
uptime                    # How long system is up
whoami                    # Current user

##############################################
# 🔐 14. SSH & SCP
##############################################
ssh user@remote-ip               # SSH login
scp file.txt user@remote:/path/ # Copy file to remote
scp user@remote:/file.txt ./    # Copy file from remote

##############################################
# 🧠 15. Monitoring & Logging
##############################################
journalctl -xe                # System logs
tail -f /var/log/messages     # Live logs
dmesg                         # Boot/kernel messages

##############################################
# 🌍 16. Networking Tools
##############################################
curl ifconfig.me              # Show public IP
traceroute google.com         # Route tracing
dig google.com                # DNS info (install: bind-utils)
```

---

## 💡 How to Save This File:

1. On your VM or host system:

   ```bash
   nano linux_cheatsheet.sh
   ```

2. Paste the full content, then:

   * Press `Ctrl + O` → Save
   * Press `Ctrl + X` → Exit

You can even keep it in GitHub, Notion, or local Notepad++ for future use ✅

---

