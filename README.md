# DevOps-machine-create

## Commands
```
arp -n
nmcli con down enp0s3 && nmcli con up enp0s3
ifdown enp0s3 && ifup enp0s3
systemctl restart NetworkManager

nmcli connection down enp0s3
nmcli connection up enp0s3

ip link set enp0s3 down
ip link set enp0s3 up

ip neigh flush all
arp -d 192.168.1.1

nmcli con mod enp0s3 ipv4.method manual ipv4.addresses 192.168.1.70/24 ipv4.gateway 192.168.1.1 ipv4.dns 8.8.8.8
nmcli con up enp0s3

systemctl restart NetworkManager
ip route show

ip route add default via 192.168.1.1 dev enp0s3

iptables -L -n
firewall-cmd --list-all
getenforce

setenforce 0

------------------------------------

nmcli con mod enp0s3 ipv4.method manual ipv4.addresses 192.168.1.100/24 ipv4.gateway 192.168.1.1 ipv4.dns 8.8.8.8
nmcli con up enp0s3

ip neigh flush all
systemctl restart NetworkManager

echo "nameserver 8.8.8.8" > /etc/resolv.conf
ping google.com

systemctl stop firewalld
systemctl disable firewalld

firewall-cmd --add-service=dns --permanent
firewall-cmd --add-masquerade --permanent
firewall-cmd --reload

setenforce 0
ping google.com

sed -i 's/^SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config
reboot

ip route show
arp -n
ping -c 4 192.168.1.1
ping -c 4 8.8.8.8
ping -c 4 google.com
```

> ### configure your network interface to use DHCP (Dynamic Host Configuration Protocol) by setting the "BOOTPROTO" parameter to "dhcp" within the network interface configuration file, 
typically located at `/etc/sysconfig/network-scripts/ifcfg-<interface>` where <interface> is the name of your network interface (like "eth0") and then restarting the network service. 

```
systemctl restart NetworkManager
nmcli connection reload
nmcli device status
nmcli connection down enp0s3 && nmcli connection up enp0s3

ip link show
ip a
ifconfig -a

nmcli connection show enp0s3
nmcli connection show
```

```
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=eui64
NAME=ens160
UUID=2e686545-0bae-462b-a8c5-a73694d3fa9b
DEVICE=ens160
ONBOOT=yes
```
```
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s3
UUID=f8ad76da-186d-4662-958e-1469bd5675d0
DEVICE=enp0s3
ONBOOT=yes
IPADDR=192.168.1.124
PREFIX=24
GATEWAY=192.168.1.1

```

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
Passwd @#1234 - Enter password whatever you want
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

### For CentOS Ip Change DHCP
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

### For Ubuntu Ip Change DHCP 
```sh
sudo vi /etc/netplan/01-netcfg.yaml
```
```sh
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    enp0s3:
      dhcp4: true
```
```sh
sudo netplan apply
```
```sh
sudo systemctl status NetworkManager
sudo systemctl restart NetworkManager
```
```sh
ip a show enp0s3
ps aux | grep dhclient
```
```sh
cat /etc/netplan/*.yaml
```

### For Ubuntu cmd
```sh
ip a
sudo apt install net-tools
ifconfig
cat /etc/dhcp/dhclient.conf
nmcli dev status
nmcli dev show enp0s3
ip link show
ip a | grep -A 2 enp0s3
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

<hr>

> ## RedHat RHEL configuration DHCP

```
nmcli device
nmcli device status
nmcli device connect eth0
```
```
nmcli connection show
nmcli connection up eth0
ip addr show
ip link show
```
```
systemctl restart NetworkManager
Net-tools
dhcclient
```
```
ls /etc/sysconfig/network-scripts/ifcfg-*
```
### Script

```
DEVICE="enp0s3"
ONBOOT=yes
NETBOOT=yes
BOOTPROTO=dhcp
TYPE=Ethernet
NAME="enp0s3"
```
