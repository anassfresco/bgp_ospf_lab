# Network Architecture Overview

This network topology demonstrates the integration of **BGP (Border Gateway Protocol)** and **OSPF (Open Shortest Path First)** routing protocols to achieve a robust and scalable inter-AS and intra-AS routing environment. The design integrates multiple Autonomous Systems (AS) to simulate inter-AS communication, while OSPF is used for internal routing within certain areas.

---

![Network Topology](https://github.com/anassfresco/bgp_ospf_lab/blob/main/pnet_lab.png)

---

## Topology Highlights

1. **Autonomous Systems (AS):**

   - Multiple ASes are deployed: **AS 65001, AS 65002, AS 65003, AS 65004, AS 65005, AS 65006, and AS 65007.**
   - BGP handles communication between these ASes, ensuring inter-AS routing with distinct path preferences.

2. **OSPF Areas:**

   - **OSPF Area 0** (Backbone Area) serves as the core, providing connectivity between other areas.
   - **OSPF Area 1** and **OSPF Area 2** are defined for hierarchical routing and segmentation.
   - A virtual link is configured between Area 0 and Area 2 for improved connectivity.

3. **Firewall Integration:**

   - Each AS incorporates **FortiGate firewalls** for security:
     - Example: **fw09** in AS 65003, **fw08** in AS 65004, **fw02** in AS 65005, etc.
   - These firewalls secure subnets such as `192.168.x.x/24` within their respective AS.

4. **Routing Design:**

   - **BGP Configuration:**
     - eBGP is used between ASes to advertise and exchange routes.
     - iBGP within AS ensures internal consistency for route propagation.
   - **OSPF Configuration:**
     - Used for intra-AS routing, dividing internal networks into logical areas.
     - Networks within the same AS use OSPF to share routes dynamically.

5. **Switch and Router Configuration:**

   - Each AS employs dedicated core routers and switches (e.g., **SW\_DC\_1/1**, **R1**, **R2**) to route traffic internally and between ASes.
   - IP addresses are assigned per subnet for clear segmentation and routing.

6. **Redundancy and Scalability:**

   - Multiple paths are configured between ASes and within OSPF areas to ensure high availability.
   - Cross-links and dual connections enhance fault tolerance.

---

## Key Components and Their Functions

| Component        | Description                                                                 |
| ---------------- | --------------------------------------------------------------------------- |
| **FortiGate**    | Provides security at AS boundaries, filters traffic, and enforces policies. |
| **Core Routers** | Handle both BGP and OSPF configurations for routing traffic.                |
| **Switches**     | Distribute traffic within data centers or ASes.                             |
| **OSPF Areas**   | Enable hierarchical routing and reduce routing overhead.                    |
| **BGP**          | Manages inter-AS routing for scalability and external connectivity.         |

---

## Virtual Link in OSPF

- A virtual link is created between **Area 0 (Backbone)** and **Area 2** to ensure reachability between non-backbone areas. This approach resolves the isolation of Area 2 and maintains OSPF backbone requirements.

---

## Sample Configuration Details

### **BGP Configuration Example (AS 65001):**

```bash
router bgp 65001
  neighbor 10.160.0.2 remote-as 65002
  network 10.160.0.0 mask 255.255.255.0
```

### **OSPF Configuration Example (Area 0):**

```bash
router ospf
  network 192.168.1.0 0.0.0.255 area 0
  network 172.16.0.0 0.0.0.3 area 0
```

---

## Diagram Explanation

The architecture is visualized with clear divisions:

- **AS Boundaries:** Marked by firewalls and connected using BGP.
- **Internal Routing:** Achieved with OSPF across designated areas.
- **Data Centers:** Switches (e.g., SW\_DC\_1/1, SW\_DC\_2/2) ensure internal traffic distribution.
- **Subnets:** Each AS is subdivided into distinct subnets for better organization.

---

This design showcases an efficient hybrid routing model with an emphasis on security, scalability, and fault tolerance.

---

