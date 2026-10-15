# VLAN and Trunking Configuration with Management SVI

📌 **About the Project**

Virtual Local Area Networks (VLANs) are essential in modern network design to logically segment a single physical network into multiple distinct broadcast domains. By isolating traffic, VLANs reduce unnecessary broadcast storms, improve overall network performance, enhance security by restricting inter-departmental communication at Layer 2, and simplify network management.

This project is based on an official CCNA lab environment implemented in Cisco Packet Tracer.

**Initial Lab Scope:**
- **Create and Name VLANs:** Configured operational VLANs (10, 20, 30, 99, 150) with appropriate naming conventions across the topology.
- **Assign VLANs to Ports:** Associated end-user access interfaces with their respective VLANs.
- **Voice VLAN Configuration:** Assigned the VOICE VLAN (VLAN 150) to interface `FastEthernet 0/11` on switch **S3**.
- **Connectivity Testing:** Verified basic Layer 2 connectivity across end devices in the topology.

**My Implementations & Additions:**
- **Trunking & DTP Protocol:** Configured 802.1Q trunk links between switches using Dynamic Trunking Protocol (DTP).
- **Native VLAN Migration:** Changed the default native VLAN from VLAN 1 to a dedicated Management & Native VLAN (VLAN 99) for security hardening.
- **Management SVI Deployment:** Implemented and verified IP addressing on Switch Virtual Interfaces (`Vlan99`) for centralized switch management.

---
---

## 📊 Addressing Table

| Device | Interface | IP Address | Subnet Mask | VLAN |
| :--- | :--- | :--- | :--- | :---: |
| **PC1** | NIC | `172.17.10.21` | `255.255.255.0` | 10 |
| **PC2** | NIC | `172.17.20.22` | `255.255.255.0` | 20 |
| **PC3** | NIC | `172.17.30.23` | `255.255.255.0` | 30 |
| **PC4** | NIC | `172.17.10.24` | `255.255.255.0` | 10 |
| **PC5** | NIC | `172.17.20.25` | `255.255.255.0` | 20 |
| **PC6** | NIC | `172.17.30.26` | `255.255.255.0` | 30 |
| **S1** | Vlan99 | `172.17.99.11` | `255.255.255.0` | 99 |
| **S2** | Vlan99 | `172.17.99.12` | `255.255.255.0` | 99 |
| **S3** | Vlan99 | `172.17.99.13` | `255.255.255.0` | 99 |
<img width="1148" height="596" alt="image" src="https://github.com/user-attachments/assets/51e04b1a-ca56-4c8d-b611-07c3fc1924be" />

---

## 🛠️ Step-by-Step Implementation & Verification

### 1. Create and Name VLANs
The initial phase required creating operational VLANs (10, 20, 30, 99, 150) across the switches with standardized naming conventions to isolate broadcast domains and structure traffic according to departmental roles.

Verification using `show vlan brief` on **S3** confirms that all specified VLANs are successfully configured and active:

<img width="592" height="287" alt="image" src="https://github.com/user-attachments/assets/9985d1e5-6899-4b24-9f57-be4b71b7e96a" />

---


### 2. Assign VLANs to Ports
After creating the operational VLANs, the access interfaces connected to end devices (PCs) were configured in access mode and assigned to their respective VLANs:
- **FastEthernet 0/6** assigned to **VLAN 30** (Guest)
- **FastEthernet 0/11** assigned to **VLAN 10** (Faculty/Staff)
- **FastEthernet 0/18** assigned to **VLAN 20** (Students)

Below is the CLI configuration applied directly on switch **S3**:


<img width="403" height="262" alt="image" src="https://github.com/user-attachments/assets/a45ecd16-db2c-402f-b932-2a53e773092f" />

---
---

### 3. Voice VLAN Configuration
To support IP Telephony alongside data traffic, interface `FastEthernet 0/11` on switch **S3** was configured with a dedicated Voice VLAN:
- **Data Traffic:** Assigned to **VLAN 10** (Faculty/Staff).
- **Voice Traffic:** Tagged and routed through **VLAN 150** (VOICE).
- **QoS:** Configured `mls qos trust cos` to prioritize voice traffic frames.

Verification via `show interfaces fastEthernet 0/11 switchport` confirms both VLAN 10 (data) and VLAN 150 (voice) are active on the interface:

<img width="457" height="531" alt="image" src="https://github.com/user-attachments/assets/f2e03c7a-f609-4e39-a3e9-36174b1559cb" />

---
---

### 4. Connectivity Testing
To verify that the configuration operates as intended, basic end-to-end connectivity testing was performed between devices assigned to the same VLAN:
- **Intra-VLAN Communication:** Devices residing in the same broadcast domain (e.g., PC1 and PC4 configured in **VLAN 10**) successfully communicated with each other.
- **Verification:** Successful ICMP echo requests (ping) confirm proper port assignment and Layer 2 frame forwarding within the configured VLAN.

<img width="406" height="225" alt="image" src="https://github.com/user-attachments/assets/b612c436-a159-4d39-b9be-0ddde581a30c" />

