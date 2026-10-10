<img width="829" height="423" alt="image" src="https://github.com/user-attachments/assets/c4592624-cb51-43f3-af3d-ac1bdbae3616" />
<img width="1220" height="304" alt="image" src="https://github.com/user-attachments/assets/78de5e07-a89e-4305-9fb2-85f4bbf2bd49" />
# Small Enterprise Network with Complete Cisco IOS Configuration & SSH

## 📌 About the Project

A small enterprise network topology designed and built entirely from a blank canvas in Cisco Packet Tracer. 

Unlike standard pre-configured lab templates, this architecture, physical cabling, and complete device configuration were built from scratch via the CLI (Command Line Interface):
* **Top-down Design:** Self-managed device placement and structured cabling (Router RTA, switches SW1 & SW2, end devices PC-1 & PC-2).
* **Base Security:** Configured encrypted enable passwords (`enable secret`), global password encryption (`service password-encryption`), console line passwords, and VTY line access.
* **System Banners:** Implemented customized MOTD (Message of the Day) security banners (`Authorized access only`).
* **Interfaces & Addressing:** Configured IPv4 addressing, subnet masks, default gateways, and descriptive interface labels (`description`).
* **Remote Management:** Deployed secure **SSH v2** access with RSA keys and a local administrator database.

---

## 🛠️ Router RTA Configuration (`show running-config`)

The actual device configuration captured directly from router RTA:

```ciscoshell
Building configuration...

Current configuration : 888 bytes
!
version 15.1
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname RTA
!
enable secret 5 $1$mERr$Amm/da5NtiazLuZDbgqZ60
!
ip cef
no ipv6 cef
!
license udi pid CISCO1941/K9 sn FTX15247W9B-
!
spanning-tree mode pvst
!
interface GigabitEthernet0/0
 description connected to SW1
 ip address 10.10.10.1 255.255.255.0
 duplex auto
 speed auto
!
interface GigabitEthernet0/1
 description connected to SW2
 ip address 10.10.20.1 255.255.255.0
 duplex auto
 speed auto
!
interface Vlan1
 no ip address
 shutdown
!
ip classless
!
ip flow-export version 9
!
banner motd ^C Authorized access only ^C
!
line con 0
 password 7 0802455D0A16091E1C0E1C057F7E
 login
!
line aux 0
!
line vty 0 4
 password 7 0802455D0A16091E1C0E1C057F7E
 login
line vty 5 15
 login
!
end
```
---
## 🔒 Switch SW1 Configuration & SSH (show running-config)
The actual device configuration captured directly from switch SW1:
```ciscoshell
Building configuration...
Current configuration : 1452 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname SW1
!
enable secret 5 $1$mERr$Amm/da5NtiazLuZDbgqZ60
!
ip ssh version 2
ip domain-name netacad.pka
!
username admin secret 5 $1$mERr$TBVHdOn0.1V2ycb55NNUm0
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface Vlan1
 ip address 10.10.10.2 255.255.255.0
!
ip default-gateway 10.10.10.1
!
line con 0
 password 7 0802455D0A16091E1C0E1C057F7E
 login
!
line vty 0 4
 password 7 0802455D0A16091E1C0E1C057F7E
 login local
 transport input ssh
line vty 5 15
 password 7 0802455D0A16091E1C0E1C057F7E
 login local
 transport input ssh
!
end
```
---
## 🔒 Switch SW2 Configuration & (show running-config)
The actual device configuration captured directly from switch SW2:
```ciscoshell
Building configuration...
Current configuration : 1297 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname SW2
!
enable secret 5 $1$mERr$Amm/da5NtiazLuZDbgqZ60
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface Vlan1
 ip address 10.10.20.2 255.255.255.0
!
ip default-gateway 10.10.20.1
!
line con 0
 password 7 0802455D0A16091E1C0E1C057F7E
 login
!
line vty 0 4
 password 7 0802455D0A16091E1C0E1C057F7E
 login
line vty 5 15
 password 7 0802455D0A16091E1C0E1C057F7E
 login
!
end
```
---
## 💻 End Devices & Verification (PC-1 & PC-2)

End devices are configured with static IP addresses matching their respective subnets and point to their local router interfaces as default gateways:

* **PC-1:** 
  * IP Address: `10.10.10.156`
  * Subnet Mask: `255.255.255.0`
  * Default Gateway: `10.10.10.1`
* **PC-2:** 
  * IP Address: `10.10.20.157`
  * Subnet Mask: `255.255.255.0`
  * Default Gateway: `10.10.20.1`

### 🧪 Connectivity & SSH Testing
To verify the network setup, the following tests were performed successfully:
1. **Inter-VLAN / Gateway Ping:** PCs can successfully reach their default gateways (`RTA`).
2. **Remote Management (SSH):** From `PC-1` (or `PC-2`), a secure SSH v2 session was established to Switch `SW1` using the administrator credentials:
   ```text
   ssh -l admin 10.10.10.2
<img width="1123" height="419" alt="image" src="https://github.com/user-attachments/assets/0ecaaf17-0d36-4334-acba-dd3ea275a32f" />

---
### 🔐 Authentication Summary
> **Security Note:** Configuration parameters and credential databases provided for educational, security hardening, and portfolio demonstration purposes.

* **SSH Administrator Database (SW1):** Username `admin` / Secret authentication
* **Privileged EXEC Mode:** Secured with `enable secret` (MD5/SHA cryptographic hashing)
* **Line Access (Console & VTY):** Protected with password authentication and `service password-encryption`
---
---

## 🧪 Testing Credentials (For Packet Tracer Simulation)

If you download and test the `SSH Management.configurations.pka` file locally, use these lab credentials:

* **Privileged EXEC Password (`enable`):** `Ciscoenpa55`
* **Console / VTY Line Password:** `Ciscolinepa55`
* **SSH Administrator (SW1):** Username `admin` / Password `Ciscoadmin123`
.  


 


