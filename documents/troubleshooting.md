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

## 🚨 4. NAT Overwrite Issue

### Problem:
Only one NAT rule was visible in configuration.

### Cause:
Multiple NAT overload commands overwrite each other.

### Solution:
- Used correct NAT configuration per router
- Avoided conflicting rules

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