> # **Windows Server 2022 Datacenter (Desktop Experience)** setting for **network configuration issue** 

```
1. **Windows Server 2022 Standard** (Core)
2. **Windows Server 2022 Standard (Desktop Experience)**
3. **Windows Server 2022 Datacenter** (Core)
4. **Windows Server 2022 Datacenter (Desktop Experience)**
5. **Azure Stack HCI (Server Core Installation)**
```

Chaliye step by step check karte hain:

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






