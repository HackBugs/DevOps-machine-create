> # 1. **Windows Server 2022 Datacenter (Desktop Experience)** setting for **network configuration issue** 
[Link](https://github.com/massgravel/Microsoft-Activation-Scripts?tab=readme-ov-file)
```
1. **Windows Server 2022 Standard** (Core)
2. **Windows Server 2022 Standard (Desktop Experience)**
3. **Windows Server 2022 Datacenter** (Core)
4. **Windows Server 2022 Datacenter (Desktop Experience)**
5. **Azure Stack HCI (Server Core Installation)**
```
```
Hostname - WIN-GQCRQ1SBBFA
Username - Administrator
Password - Admin123
```

> # 2. Installation Windows Server 2022 - Chaliye step by step check karte hain:

---

## 🔎 Step 1: VM Network Settings Check karo

Aap kaunsa virtualization use kar rahe ho?

* **VMware / VirtualBox / Hyper-V** me network adapter ka **mode** decide karta hai ki bahar se ping hoga ya nahi.

  * **NAT (default hota hai):** VM internet use kar sakta hai, lekin bahar se direct ping nahi hota.
  * **Bridged Adapter:** VM directly aapke LAN network pe ek alag device ban jaata hai → bahar se ping ho jaayega.
  * **Host-only Adapter:** Sirf host aur VM ke beech network.

👉 Solution:
Network Adapter ko **Bridged Mode** pe set karo, taaki VM ko aapke physical network se direct IP mile.

---

## 🔎 Step 2: Windows Firewall me ICMP (Ping) allow karo

By default **Windows Server firewall ping block kar deta hai**.

1. Open: **Windows Defender Firewall with Advanced Security**
2. Left → **Inbound Rules**
3. Find rule: **File and Printer Sharing (Echo Request - ICMPv4-In)**
4. Enable karo (Allow).

👉 Ab bahar se ping kaam karega.

---

## 🔎 Step 3: IP Address confirm karo

* Server me `ipconfig` run karke IP check karo.
* Phir host ya network ke dusre PC se `ping <server-ip>` try karo.

---

## 🔎 Step 4 (Optional): Routing / NAT check karo

Agar aapko **sirf host machine se ping karna hai** to NAT + Port Forwarding bhi configure kar sakte ho (VirtualBox/VMware NAT mode me).

---

### ✅ Final Recommended Setup (aapke case me):

* **Bridged Adapter** select karo
* **Firewall me ICMP allow** karo
---

> # 3. Aapko **Remote Desktop Connection**  to Host to Windows server 2022

---

## ✅ Step 1: Remote Desktop Enable karo

1. Apne server VM ke andar login karo.
2. Right-click **This PC** → **Properties**.
3. Left side pe **Remote Desktop** option milega → click karo.
4. **Enable Remote Desktop** select karo.
5. Firewall confirmation aayega → allow kar do.

---

## ✅ Step 2: Firewall me RDP Allow karo

1. Start menu → search **Windows Defender Firewall with Advanced Security**.
2. Left side: **Inbound Rules**.
3. List me **Remote Desktop (TCP-In)** dhoondo.
4. Make sure status **Enabled** hai. Agar disabled ho to enable karo.

---

## ✅ Step 3: Network Setting Check karo

* Server ka **IP address** check karo:

  ```cmd
  ipconfig
  ```
* Host machine se ye IP ping karo. Agar ping nahi ho raha to pehle ICMP allow karna padega (maine pehle steps bataye the).

---

## ✅ Step 4: User Permission

* Sirf **Administrators group** ya explicitly allowed users RDP se login kar sakte hain.
* Check:

  1. Remote Desktop settings me **Select users** option hai.
  2. Waha aap apna user add karo (agar admin nahi ho).

---

## ✅ Step 5: Connect karna

* Apne host machine se open karo:

  ```
  mstsc
  ```
* Enter server ka IP.
* Username likho:

  ```
  SERVERNAME\Administrator
  ```

  (ya jo bhi user aapne banaya hai)

---

> # 4. **VirtualBox VM me Windows Server 2022 install karke Remote Desktop enable** kar diya hai aur ab **host PC se connect ho ja rahe ho**. Ab aap chahte ho ki **dusre place se (public internet)** bhi connect kar sako.

Ye 3 step me possible hai 👇

---

## 🔹 Step 1: VirtualBox Network Setting

By default VirtualBox VM **NAT** pe hota hai, jo sirf host ko access deta hai. Aapke paas do option hain:

### Option A → **Bridged Adapter** (easy way)

* VirtualBox me VM select karo → **Settings → Network**.
* "Attached to" dropdown me **Bridged Adapter** select karo.
* Phir VM ko restart karo.
* Ab VM ko **aapke broadband router se direct ek IP milega (192.168.x.x)** jaise normal PC ko milta hai.

👉 Is IP ko use karke same LAN me koi bhi connect kar sakta hai.

### Option B → **NAT + Port Forwarding**

Agar aap NAT use karna chahte ho:

* VirtualBox → VM Settings → Network → Advanced → Port Forwarding.
* Add rule:

  * Protocol: TCP
  * Host Port: `50000` (ya koi bhi custom)
  * Guest IP: `192.168.x.x` (VM ka local IP)
  * Guest Port: `3389` (RDP port)

👉 Ab aap **host PC ke public IP:50000** se connect kar sakte ho.

---

## 🔹 Step 2: Router me Port Forwarding

Abhi tak sirf **local network** se connect ho paoge. Agar aapko **duniya ke kahin se bhi RDP chahiye**, to:

1. Apne **broadband router** ke web panel me login karo (192.168.1.1 jaisa hota hai).
2. **Port Forwarding / Virtual Server** option dhoondo.
3. Rule banao:

   * Public Port: `50000` (jo aapne VirtualBox NAT me set kiya)
   * Forward to: `192.168.x.x:3389` (aapke VM ka internal IP)

Ab jo bhi aapka **Public IP hai (ISP ka)**, uspe connect karke RDP chal jaayega.

Example:

```
mstsc → 203.91.xx.xx:50000
```

---

## 🔹 Step 3: Security Precautions ⚠️

Public RDP open karna risky hota hai. Attackers brute-force karte rehte hain. Isliye:
✅ Strong password rakho.
✅ Agar possible ho to RDP port `3389` change karke koi aur rakho.
✅ Firewall me rule lagao ki sirf specific IPs se RDP allow ho.
✅ Best & secure way: **VPN setup karo (OpenVPN, WireGuard)** aur phir sirf VPN ke andar RDP allow karo.

---


