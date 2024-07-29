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

- Virtual box Machine Setting network - attached to - bridged adapeter choose name as your network
- cmd - hostname -i
- cmd - connection show
- 
- cmd - nmcli connection up upp0s3

### Now you clone and write name jenkins what ever you want to run
