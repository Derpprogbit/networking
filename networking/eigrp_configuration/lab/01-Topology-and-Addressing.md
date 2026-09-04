# 01 — Topology and Addressing

## 1. Topology Diagram

Three routers in a triangle so I can see EIGRP form adjacencies over more than one link, plus a loopback on each router to simulate a LAN behind that router (so there's something worth routing besides the point-to-point links).

```
                         Lo0: 192.168.1.1/24
                                |
                             [ R1 ]
                            /        \
                 10.0.12.0/30      10.0.13.0/30
                          /            \
                     [ R2 ]------------[ R3 ]
                        |    10.0.23.0/30   |
              Lo0: 192.168.2.1/24    Lo0: 192.168.3.1/24
```

- **R1 — R2** link: `Gig0/0/0` on both ends
- **R2 — R3** link: `Gig0/0/1` on both ends
- **R1 — R3** link: `Gig0/0/1` on R1, `Gig0/0/0` on R3
- Each router's **Loopback0** represents a stub LAN (e.g., a floor's user subnet)

> [!tip] Why a triangle instead of a straight line?
> A line topology (R1–R2–R3) only proves EIGRP works over one hop at a time. A triangle lets me confirm DUAL is actually picking the best path and lets me test **Feasible Successors** later by shutting down a link.

## 2. IP Addressing Table

| Device | Interface | IP Address | Subnet Mask | Wildcard Mask |
|---|---|---|---|---|
| R1 | Gig0/0/0 (to R2) | 10.0.12.1 | 255.255.255.252 | 0.0.0.3 |
| R1 | Gig0/0/1 (to R3) | 10.0.13.1 | 255.255.255.252 | 0.0.0.3 |
| R1 | Loopback0 | 192.168.1.1 | 255.255.255.0 | 0.0.0.255 |
| R2 | Gig0/0/0 (to R1) | 10.0.12.2 | 255.255.255.252 | 0.0.0.3 |
| R2 | Gig0/0/1 (to R3) | 10.0.23.1 | 255.255.255.252 | 0.0.0.3 |
| R2 | Loopback0 | 192.168.2.1 | 255.255.255.0 | 0.0.0.255 |
| R3 | Gig0/0/0 (to R1) | 10.0.13.2 | 255.255.255.252 | 0.0.0.3 |
| R3 | Gig0/0/1 (to R2) | 10.0.23.2 | 255.255.255.252 | 0.0.0.3 |
| R3 | Loopback0 | 192.168.3.1 | 255.255.255.0 | 0.0.0.255 |

## 3. Design Notes

- All point-to-point links use `/30` subnets — only 2 usable hosts needed per link.
- Loopbacks use `/24` on purpose (not `/32`) so I can practice EIGRP advertising a "normal" LAN-sized subnet, not just a router ID.
- EIGRP Autonomous System number chosen for this lab: **AS 100**. This number just has to match on every router in the same EIGRP domain — it does **not** need to match any real-world ASN, it's a locally significant tag.
- Because I'm using non-contiguous /24 loopback networks off a 10.x.x.x backbone, `no auto-summary` is required — otherwise classic EIGRP will summarize at the classful boundary and break reachability. This is covered in file 02.

## 4. Packet Tracer Setup Checklist

- [ ] Place 3x Router (1941 or ISR4331, depending on what's available in your PT version)
- [ ] Connect R1↔R2 and R2↔R3 and R1↔R3 with copper straight-through cables (PT auto-detects and treats as crossover where needed)
- [ ] Add a Loopback0 interface on each router (`interface loopback 0` — this is a virtual interface, no cable needed)
- [ ] Label each router in Packet Tracer to match R1 / R2 / R3 for clarity
