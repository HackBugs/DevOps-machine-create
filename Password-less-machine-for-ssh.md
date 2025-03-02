> # steps for setting up a password-less SSH connection and transferring files using `scp`:  

---

# **Setup Password-less SSH & Secure File Transfer (SCP)**  

## **1. Enable SSH on Both Machines**  
Run these commands on both machines to ensure SSH is running:  
```bash
systemctl status sshd
systemctl start sshd
systemctl enable sshd
```

## **2. Generate SSH Key on the Source Machine**  
Run the following command on the machine that will initiate SSH connections (192.168.1.125):  
```bash
ssh-keygen -t rsa
```

## **3. Verify Key Generation**  
Check if the SSH key files are created in the `.ssh` directory:  
```bash
cd ~/.ssh  
ll -a  
cat id_rsa.pub  # View the public key  
```

## **4. Copy the SSH Key to the Destination Machine**  
Send the generated key to the target machine (192.168.1.124):  
```bash
ssh-copy-id root@192.168.1.124
```

## **5. Confirm Network Connectivity**  
Check the IP routes to verify network connectivity:  
```bash
ip r
```

## **6. Transfer Files via SCP**  
Copy files from the source machine (`192.168.1.125`) to the destination (`192.168.1.124`):  
```bash
scp /home/alam/.ssh/ssh-transfar-check/* root@192.168.1.124:/root/ssh-transfar-check-2
```

---

Now, you should be able to SSH into `192.168.1.124` from `192.168.1.125` without a password and transfer files securely using SCP. 🚀

--- 

## **7. Transfer Files From this location To that location**
```
From - /home/alam/.ssh/ssh-transfar-check/
To - /root/ssh-transfar-check-2
```

---

> # 1. Capture transfar scpt Output data Script / working 

```
#!/bin/bash

# Log file path
LOG_FILE="/home/alam/.ssh/ssh-transfar-check/transfar-scp-log.txt"

# Current Date and Time
cur_time=$(date '+%d-%m-%Y %H:%M')

# Append Date to Log File
echo -e "\n===== Transfer Log: $cur_time =====" >> "$LOG_FILE"

# Run SCP command and capture output
script -q -c "scp /home/alam/.ssh/ssh-transfar-check/* root@192.168.1.124:/root/ssh-transfar-check-2" >> "$LOG_FILE" 2>&1
```

---

> # 2.  Capture transfar scpt Output data Script / not wroking properly

```
#!/bin/bash

# Log file path
LOG_FILE="/home/alam/.ssh/ssh-transfar-check/transfar-scp-log.txt"

# Current Date and Time
cur_time=$(date '+%d-%m-%Y %H:%M')

# Append Date to Log File
echo -e "\n===== Transfer Log: $cur_time =====" >> "$LOG_FILE"

# Run SCP command with verbose mode and capture last 3 lines
scp -v /home/alam/.ssh/ssh-transfar-check/* root@192.168.1.124:/root/ssh-transfar-check-2 2>&1 | tail -3 > a.txt

# Append the last 3 lines to log file
cat a.txt >> "$LOG_FILE"

# Remove temporary file
rm -f a.txt
```
