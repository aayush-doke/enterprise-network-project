# 🏗️ Network Design Explanation

## 📌 Overview
This network is designed to simulate a real-world enterprise architecture with scalability and redundancy. It follows a hierarchical model consisting of Access, Distribution, and Edge layers.

---

## 🧱 Network Layers

### 🔹 Access Layer
- Consists of Layer 2 switches
- Connects end devices (PCs, servers)
- Configured with:
  - Port Security

---

### 🔹 Distribution Layer
- Implemented using Layer 3 switches (DSW1, DSW2)
- Responsible for:
  - Inter-VLAN routing using SVIs
  - Default gateway for all VLANs
  - HSRP for redundancy
  - DHCP relay using `ip helper-address`

---

### 🔹 Edge Layer
- Consists of two routers (R1, R2)
- Provides:
  - Internet access via NAT/PAT
  - OSPF routing
  - Dual ISP connectivity

---

## 🌐 VLAN Design

| VLAN | Department |
|------|-----------|
| 10 | Sales |
| 20 | HR |
| 30 | Admin |
| 40 | Servers |

---

## 🔁 Redundancy Design (HSRP)

- HSRP used to provide a virtual default gateway
- Load balancing implemented:
  - VLAN 10, 30 → Active on DSW1
  - VLAN 20, 40 → Active on DSW2
- Ensures high availability in case of switch failure

---

## 🧭 Routing Design (OSPF)

- OSPF used for dynamic routing
- Single Area (Area 0)
- Default route injected from edge router
- OSPF cost tuning used to:
  - Direct traffic to optimal router
  - Align with HSRP active paths

---

## 🌍 NAT & Internet Access

- PAT (NAT Overload) configured on edge routers
- Private IPs translated to public ISP IPs
- Dual ISP design:
  - Primary path via ISP1
  - Backup path via ISP2

---

## 🔐 Security Design

- Port Security:
  - Limits MAC addresses per port
- SSH:
  - Secure remote device access

---

## ⚙️ DHCP Design

- Central DHCP server in VLAN 40
- All VLANs use `ip helper-address` to reach DHCP server
- Simplifies IP management

---

## 🎯 Design Goals Achieved

- High availability (HSRP + Dual ISP)
- Scalability (VLAN segmentation)
- Efficient routing (OSPF)
- Real-world enterprise simulation

---