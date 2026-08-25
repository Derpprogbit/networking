### Network Topology & Overview

![[vlan_trunking_topology.png]]

This topology demonstrates multi-VLAN segmentation across two switches (`Switch0` and `Switch1`). Inter-switch communication is enabled via an 802.1Q trunk link configured on FastEthernet 0/1 (`Fa0/1`) on both switches, utilizing **VLAN 99** as the native VLAN.

---

### IP Addressing Scheme & Port Assignments

| Device / Host | Interface / Port | IP Address | Subnet Mask | VLAN | Role / Description |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Switch0** | SVI (Vlan 10) | `192.168.10.2` | `255.255.255.0` | 10 | Switch Management |
| **Switch0** | Fa0/1 | N/A | N/A | 99 (Native) | Trunk Link to Switch1 |
| **PC0** | Fa0 | `192.168.10.4` | `255.255.255.0` | 10 | VLAN 10 Host |
| **PC1** | Fa0 | `192.168.10.5` | `255.255.255.0` | 10 | VLAN 10 Host |
| **Switch1** | SVI (Vlan 10) | `192.168.10.3` | `255.255.255.0` | 10 | Switch Management (VLAN 10) |
| **Switch1** | SVI (Vlan 20) | `192.168.20.1` | `255.255.255.0` | 20 | Switch Management (VLAN 20) |
| **Switch1** | Fa0/1 | N/A | N/A | 99 (Native) | Trunk Link to Switch0 |
| **PC2** | Fa0 | `192.168.10.6` | `255.255.255.0` | 10 | VLAN 10 Host |
| **PC3** | Fa0 | `192.168.10.7` | `255.255.255.0` | 10 | VLAN 10 Host |
| **PC4** | Fa0 | `192.168.20.2` | `255.255.255.0` | 20 | VLAN 20 Host |
| **PC5** | Fa0 | `192.168.20.3` | `255.255.255.0` | 20 | VLAN 20 Host |
