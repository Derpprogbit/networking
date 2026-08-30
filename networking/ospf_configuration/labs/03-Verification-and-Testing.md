# Verification and Testing

## 1. OSPF Neighbor Verification
Run `show ip ospf neighbor` on **R1** to confirm neighbor adjacency with **R2**:

```text
R1# show ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           0   FULL/ -         00:00:36    10.0.0.2        GigabitEthernet0/0/0
```

> **Key Indicator:** State must show `FULL/ -` or `FULL/DR` / `FULL/BDR`. If stuck in `INIT` or `2-WAY`, check for area ID mismatches or subnet mask errors.

## 2. Routing Table Inspection
Run `show ip route ospf` on **R1** to confirm learned dynamic routes:

```text
R1# show ip route ospf
O    192.168.2.0/24 [110/2] via 10.0.0.2, 00:04:22, GigabitEthernet0/0/0
```

- **`O`**: OSPF learned route.
- **`[110/2]`**: Administrative Distance (110) / Cost Metric (2).

## 3. End-to-End Reachability Ping Test
From **PC0 Command Prompt** (`192.168.1.10`), ping **PC1** (`192.168.2.10`):

```text
C:\> ping 192.168.2.10

Pinging 192.168.2.10 with 32 bytes of data:
Reply from 192.168.2.10: bytes=32 time=1ms TTL=126
Reply from 192.168.2.10: bytes=32 time=1ms TTL=126
Reply from 192.168.2.10: bytes=32 time=1ms TTL=126
Reply from 192.168.2.10: bytes=32 time=1ms TTL=126

Ping statistics for 192.168.2.10:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss).
```
