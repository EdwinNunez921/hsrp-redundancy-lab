# 🔁 HSRP (First Hop Redundancy) Lab – Layer 3 Failover

## Overview
This project demonstrates the implementation of **HSRP (Hot Standby Router Protocol)**, a Cisco proprietary First Hop Redundancy Protocol (FHRP), to provide **Layer 3 gateway redundancy** and ensure continuous network availability during router failure.

The topology consists of:
- 3 Routers (R1, R2, R3)
- 2 Switches
- 2 End Hosts (PC1, PC2)

Router 3 simulates an external network (internet) using a loopback interface (8.8.8.8).

---

## Initial Configuration
- Configured IP addressing on all devices
- Configured default gateways on end hosts:
  - PC1 → R1
  - PC2 → R2
- Enabled **OSPF** on R1 and R2 to dynamically learn routes to the simulated external network (R3)
- Configured routing so both PCs could successfully ping **8.8.8.8**

---

## HSRP Configuration
- Implemented **HSRP Version 2** on R1 and R2
- Placed both routers in the same standby group
- Configured a **virtual IP address: 10.0.1.254**

### Router Roles:
- **R1 (Active Router):**
  - Priority set to **200**
  - **Preemption enabled** to allow it to reclaim active role after recovery

- **R2 (Standby Router):**
  - Priority set to **50**

---

## Default Gateway Update
- Updated both PC1 and PC2 to use the **virtual IP address (10.0.1.254)** as their default gateway

---

## Verification
- Verified both PCs could successfully ping **8.8.8.8** using the virtual IP
- Used `tracert` to confirm traffic was initially flowing through **R1 (Active Router)**
- Verified HSRP status using CLI commands (e.g., `show standby`)

---

## Failover Testing
1. **Simulated Failure:**
   - Shut down R1
   - R2 transitioned from **Standby → Active**
   - Verified both PCs could still ping **8.8.8.8**
   - Confirmed traffic now flowed through R2

2. **ARP Verification:**
   - Checked ARP tables on end hosts
   - Confirmed presence of **HSRP virtual MAC address**
   - Verified correct Layer 2 to Layer 3 mapping during failover

3. **Recovery Test:**
   - Re-enabled R1 interface
   - Due to **preemption**, R1 reclaimed its role as **Active Router**
   - R2 returned to **Standby state**

---

## Final Validation
- Confirmed continuous connectivity to **8.8.8.8** during:
  - Normal operation (R1 active)
  - Failover scenario (R2 active)
  - Recovery (R1 resumes active role)

This validates successful implementation of **Layer 3 failover with zero loss of network availability**.

---

## Key Takeaways
- HSRP provides seamless **default gateway redundancy**
- Preemption ensures the preferred router regains control after recovery
- Virtual IP allows hosts to maintain a **consistent gateway**
- Network remains operational even during router failure
- Demonstrates real-world **high availability network design**
