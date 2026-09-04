# 02 — CLI Commands and Configuration

Since I already know `enable`, `configure terminal`, interface configuration, and basic `show` commands from previous labs, this file focuses only on what's **new for EIGRP**.

## 1. EIGRP-Specific Commands Explained

| Command | What it does |
|---|---|
| `router eigrp <AS-number>` | Enters EIGRP configuration mode and starts the process under Autonomous System `<AS-number>`. This number must be identical on every router that should form an adjacency — it is not tied to a real internet ASN, it's just a local tag. |
| `network <address> <wildcard-mask>` | Tells EIGRP which locally connected interfaces to advertise. Uses a **wildcard mask** (inverse of a subnet mask) instead of a regular subnet mask — this trips up a lot of people coming from OSPF's similar-but-different `network ... area` syntax. |
| `no auto-summary` | Disables automatic summarization of routes at classful network boundaries. Classic EIGRP summarizes by default, which can hide subnets and break routing across discontiguous networks. Almost always disabled in modern designs. |
| `passive-interface <interface>` | Stops EIGRP Hello packets from being sent out an interface while still advertising that interface's network. Used on LAN/access interfaces where no EIGRP neighbor should ever exist (security + reduces unnecessary traffic). |
| `eigrp router-id <id>` | Manually sets the router ID used in EIGRP (otherwise it's auto-derived from the highest loopback IP, or highest active interface IP if no loopback exists). Good practice to set this explicitly. |
| `bandwidth <kbps>` (interface mode) | Sets the interface's bandwidth value used in EIGRP's metric calculation. Does **not** change the physical link speed — it's a label EIGRP (and OSPF cost, and QoS) reads. |
| `delay <tens of microseconds>` (interface mode) | Sets the interface delay value, the other half of EIGRP's default composite metric. |
| `show ip eigrp neighbors` | Lists routers this router has formed a full EIGRP adjacency with. This is the very first thing to check when EIGRP "isn't working." |
| `show ip eigrp topology` | Shows DUAL's topology table: the **Successor** (best path, installed in the routing table) and any **Feasible Successor** (backup path, ready instantly if the successor fails) for every learned network. |
| `show ip protocols` | Shows which routing protocols are running, the AS number, timers, and which networks are being advertised — useful to sanity-check the whole config in one screen. |
| `show ip route eigrp` | Filters the routing table to show only routes learned via EIGRP (marked with a `D` — "Diffusing" — in the full routing table). |

> [!warning] Wildcard mask, not subnet mask
> `network 10.0.12.0 0.0.0.3` is correct. `network 10.0.12.0 255.255.255.252` will be silently rejected/misread by classic EIGRP. Wildcard = subnet mask flipped bit-for-bit (0 = must match, 1 = don't care).

## 2. Full Configuration — R1

```
enable
configure terminal
hostname R1

interface GigabitEthernet0/0/0
 description Link-to-R2
 ip address 10.0.12.1 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet0/0/1
 description Link-to-R3
 ip address 10.0.13.1 255.255.255.252
 no shutdown
 exit

interface Loopback0
 description R1-LAN-simulated
 ip address 192.168.1.1 255.255.255.0
 exit

router eigrp 100
 eigrp router-id 1.1.1.1
 network 10.0.12.0 0.0.0.3
 network 10.0.13.0 0.0.0.3
 network 192.168.1.0 0.0.0.255
 passive-interface Loopback0
 no auto-summary
 exit

end
write memory
```

## 3. Full Configuration — R2

```
enable
configure terminal
hostname R2

interface GigabitEthernet0/0/0
 description Link-to-R1
 ip address 10.0.12.2 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet0/0/1
 description Link-to-R3
 ip address 10.0.23.1 255.255.255.252
 no shutdown
 exit

interface Loopback0
 description R2-LAN-simulated
 ip address 192.168.2.1 255.255.255.0
 exit

router eigrp 100
 eigrp router-id 2.2.2.2
 network 10.0.12.0 0.0.0.3
 network 10.0.23.0 0.0.0.3
 network 192.168.2.0 0.0.0.255
 passive-interface Loopback0
 no auto-summary
 exit

end
write memory
```

## 4. Full Configuration — R3

```
enable
configure terminal
hostname R3

interface GigabitEthernet0/0/0
 description Link-to-R1
 ip address 10.0.13.2 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet0/0/1
 description Link-to-R2
 ip address 10.0.23.2 255.255.255.252
 no shutdown
 exit

interface Loopback0
 description R3-LAN-simulated
 ip address 192.168.3.1 255.255.255.0
 exit

router eigrp 100
 eigrp router-id 3.3.3.3
 network 10.0.13.0 0.0.0.3
 network 10.0.23.0 0.0.0.3
 network 192.168.3.0 0.0.0.255
 passive-interface Loopback0
 no auto-summary
 exit

end
write memory
```

## 5. Step-by-Step Activity Flow

1. Cable the three routers into the triangle topology described in file `01`.
2. Configure hostnames and interface IPs on R1, R2, and R3 (no EIGRP yet) — bring every interface `no shutdown` first.
3. Confirm with `show ip interface brief` on each router that all interfaces show `up / up` before touching EIGRP — routing problems are much easier to debug when you know Layer 1/2 is already fine.
4. Enable EIGRP AS 100 on R1 only, add its `network` statements, then move to R2.
5. Enable EIGRP AS 100 on R2, add its `network` statements. At this point R1 and R2 should form a neighbor adjacency — check with `show ip eigrp neighbors` on either router before moving on.
6. Repeat for R3. Once R3 is configured, all three routers should see two neighbors each (a full mesh).
7. Add `no auto-summary` and the `passive-interface Loopback0` line on all three if not already typed in during step 4–6.
8. Move to file `03` and run the full verification + troubleshooting checklist.
