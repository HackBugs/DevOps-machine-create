# DevOps-machine-create

### Download Centos - CentOS Server 9 (64bit).vdi
 - Webiste - https://www.osboxes.org/centos/ - Download Server not Workstation
 - install Oracle VM virtual Box
 - Create machine new dont't select image write name and path where you want to save than
   Go hardware - section choose memory as per per your requrement now Go harddisk - "choose an existing virtual harddisk file

- Open terminal enter username login credintial from webiste where you donwload there meniton in info secion
```sh
Username: osboxes
Password: osboxes.org
Root Account Password: osboxes.org
Guest Tools: Open-VM-Tools Installed
Keyboard Layout: US (Qwerty)
VMware Compatibility: Version 10+
```

- **After Login change Password and add user**
```sh
Passwd root - change whatever you want
useradd HackBugs
Passwd @#123 - Enter password whatever you want
```

- Virtual box Machine Setting network - attached to - bridged adapeter choose name as your network
```sh
- For ubuntu
- cmd - snap install network-manager
- cmd - apt install network-manager 
```  
- cmd - ```apt-get update```
- cmd - ```hostname -i```
- cmd - ```connection show ```
- cmd - ```nmcli connection up enp0s3 ```

### Connect with SSH with putty as root fist change this
- ```vi /etc/ssh/sshd_config```
- edit - ```PermitRootLogin Yes```
- ```systemctl restart sshd```

### Ip Change DHCP
- ```vi /etc/sysconfig/network-scripts/ifcfg-enp0s3```
```sh
DEVICE="enp0s3"
ONBOOT=yes
NETBOOT=yes
BOOTPROTO=dhcp
TYPE=Ethernet
NAME="enp0s3"
```
```sh
reboot
```
```sh
poweroff
```
__________________________________________________________________________

### Clone of machine
 - Click on Cline and choose this options
 - Nmae - Jenkins
 - Path - you can leave defualt
 - MAC address policy - Generate new mac address for all network adaptars
 - Change hostname
```sh
sudo hostnamectl set-hostname minikube
```
__________________________________________________________________________

### This command will provide detailed information about the system, similar to the `systeminfo` command on Windows.

- On Centos
1. **Install**
   ```bash
   sudo yum install epel-release
   sudo yum install inxi
   ```

2. **Run `inxi` to get system information:**
   ```bash
   inxi -Fxz
   ```
  
- On Ubuntu
1. **Install `inxi`:**
   ```bash
   sudo apt update
   sudo apt install inxi
   ```

2. **Run `inxi` to get system information:**
   ```bash
   inxi -Fxz
   ```

 ### Download file from internet use this cmd
 `wget`: Best for downloading files and recursively fetching directories or websites.
 `curl`: Best for interacting with APIs, customizing requests, and handling a variety of protocols.
