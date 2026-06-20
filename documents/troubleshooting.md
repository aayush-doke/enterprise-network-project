# 🔧 Troubleshooting & Issues Faced

## 🚨 1. NAT Not Working

### Problem:
Traffic reached ISP but packets were not translated (private IP visible).

### Cause:
Missing `ip nat inside` on router interface.

### Solution:
- Configured correct inside/outside interfaces
- Verified using `show ip nat translations`

---

## 🚨 2. Internet Not Reachable (TTL Expiry)

### Problem:
Traceroute stopped at distribution switch.

### Cause:
Default route missing or incorrect routing path.

### Solution:
- Fixed OSPF default route propagation
- Used `redistribute static` on router

---

## 🚨 3. Default Route Not Advertised in OSPF

### Problem:
Other devices were not receiving default route.

### Cause:
Router had static default route but it was not redistributed.

### Solution:
- Enabled:
router ospf 1
redistribute static subnets

---

## 🚨 4. NAT Overwrite Bug (Dual-WAN Limitation)

### Problem:
- Attempting to apply multiple NAT overload commands bound to separate outbound interfaces caused the commands to overwrite each other in the Cisco IOS running configuration.

### Cause:
- Cisco IOS treats mapping the exact same standard Access Control List (ACL) to two different physical outbound interfaces as a direct command conflict, overwriting the previous entry.

### Solution:
- Implemented the Access List Split Trick to bypass the syntax limitation.
- Created two identical, standalone access lists (access-list 10 and access-list 20) containing the enterprise private network footprints.
- Separately bound each list to its respective primary and backup interface, allowing simultaneous NAT rules to coexist without conflict:
- `ip nat inside source list 10 interface Serial0/2/0 overload`
- `ip nat inside source list 20 interface Serial0/2/1 overload`

---

## 🚨 5. Routing Loop / TTL Expiry

### Problem:
Packets looped between devices and TTL expired.

### Cause:
Mismatch between HSRP active gateway and OSPF routing path.

### Solution:
- Tuned OSPF cost
- Ensured:
- DSW1 → R1
- DSW2 → R2

---

## 🚨 6. Traceroute vs Ping Behavior

### Observation:
Simple PDU worked but ping failed.

### Explanation:
- Simulation mode may show success
- Real ping requires correct return path

---

## 🎯 Key Learning

- Small misconfigurations (like missing NAT inside) can break entire network
- Alignment of HSRP + OSPF + NAT is critical
- Troubleshooting is as important as configuration
- Real-world networks require step-by-step isolation of issues

---
