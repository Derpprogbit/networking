# Topology and Addressing Table

## 1. Topology Diagram
![Network Topology](attachments/topology.png)

## 2. IP Addressing Scheme
| Device | Interface | IP Address | Subnet Mask | Wildcard Mask | OSPF Area / Zone |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **R1** | Gi0/0/0 | `10.0.0.1` | `255.255.255.252` (`/30`) | `0.0.0.3` | Area 0 (Backbone) |
| **R1** | Gi0/0/1 | `192.168.1.1` | `255.255.255.0` (`/24`) | `0.0.0.255` | Area 0 (Passive) |
| **R2** | Gi0/0/0 | `10.0.0.2` | `255.255.255.252` (`/30`) | `0.0.0.3` | Area 0 (Backbone) |
| **R2** | Gi0/0/1 | `192.168.2.1` | `255.255.255.0` (`/24`) | `0.0.0.255` | Area 0 (Passive) |
| **PC0** | FastEthernet0 | `192.168.1.10` | `255.255.255.0` (`/24`) | N/A | Default Gateway: `192.168.1.1` |
| **PC1** | FastEthernet0 | `192.168.2.10` | `255.255.255.0` (`/24`) | N/A | Default Gateway: `192.168.2.1` |

## 3. Subnetting & Wildcard Mask Calculations

### WAN Subnet (/30)
- **Subnet Mask:** `255.255.255.252`
- **Wildcard Formula:** `255.255.255.255 - 255.255.255.252`
- **Calculated Wildcard Mask:** `0.0.0.3`
- **Network ID:** `10.0.0.0`
- **Broadcast Address:** `10.0.0.3`
- **Usable Host Addresses:** `10.0.0.1` and `10.0.0.2`

### LAN Subnet (/24)
- **Subnet Mask:** `255.255.255.0`
- **Wildcard Formula:** `255.255.255.255 - 255.255.255.0`
- **Calculated Wildcard Mask:** `0.0.0.255`
- **Usable Host Range:** `.1` through `.254`
