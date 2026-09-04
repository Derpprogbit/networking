# 03 — Verification and Testing

## 1. Verify Neighbor Adjacencies

```
R1# show ip eigrp neighbors
```

Expected output (on R1, after all three routers are configured):

```
EIGRP-IPv4 Neighbors for AS(100)
H   Address         Interface   Hold Uptime   SRTT   RTO  Q  Seq
                                 (sec)         (ms)       Cnt Num
1   10.0.13.2       Gi0/0/1       14 00:02:10   40   200  0  12
0   10.0.12.2       Gi0/0/0       12 00:03:45   35   200  0  15
```

- Two neighbors expected on R1 (R2 and R3).
- If this list is **empty**, EIGRP is not forming adjacencies — go to the troubleshooting table below.

## 2. Verify the Topology Table

```
R1# show ip eigrp topology
```

Look for:
- **P** (Passive) state next to each route — this is the *healthy* state, meaning DUAL is done computing and has a stable best path. **A** (Active) means DUAL is currently recalculating — fine briefly during convergence, a problem if it stays that way.
- A line beginning `via` for the **Successor** (installed route), and optionally a second `via` line for a **Feasible Successor** (backup, not in the routing table but ready).

## 3. Verify the Routing Table

```
R1# show ip route eigrp
```

Expected: R1 should show `D` routes for R2's and R3's loopback networks (192.168.2.0/24 and 192.168.3.0/24), plus the transit link it isn't directly connected to (R2–R3's 10.0.23.0/30).

## 4. Verify Protocol Settings

```
R1# show ip protocols
```

Confirm:
- `Routing Protocol is "eigrp 100"`
- Correct `network` statements listed
- `Automatic network summarization is not in effect` (confirms `no auto-summary` took effect)

## 5. End-to-End Connectivity Test

From R1:

```
R1# ping 192.168.2.1
R1# ping 192.168.3.1
```

Both should succeed once EIGRP has converged. Also test from a PC attached to one router's LAN side (if added to the topology) to a PC on another router's LAN, to prove routing works end-to-end, not just router-to-router.

## 6. Common Problems & Troubleshooting

| Symptom | Likely Cause | Fix |
|---|---|---|
| `show ip eigrp neighbors` is empty between two directly connected routers | AS number mismatch | Confirm `router eigrp <AS>` uses the **same** number on both routers |
| Neighbors won't form, interfaces are `up/up` | Interface subnet mismatch (routers not actually on the same subnet) | Recheck IPs and masks against the addressing table in file `01` |
| Neighbors won't form despite matching AS and correct subnet | K-value (metric weight) mismatch | Run `show ip protocols` on both sides and compare the K1–K5 values; they must match exactly |
| Neighbor forms then immediately flaps (up/down repeatedly) | Hello/Hold timer mismatch, or a flapping physical link | Check `show interfaces` for errors/resets; check timers with `show ip eigrp interfaces detail` |
| Route missing from `show ip route eigrp` | Forgot a `network` statement, or wildcard mask is wrong | Re-check the `network` line's wildcard mask — a mask that's too narrow silently excludes the interface |
| Fewer routes than expected, or routes look "summarized" (e.g., only 192.168.0.0/16 shown) | `auto-summary` still enabled | Run `no auto-summary` under `router eigrp 100` on every router |
| Loopback interfaces are forming EIGRP adjacencies with themselves / unexpected neighbors on LAN side | Forgot `passive-interface` on Loopback0 | Add `passive-interface Loopback0` under the EIGRP process |

## 7. Lab Completion Checklist

- [ ] All three routers show 2 neighbors each in `show ip eigrp neighbors`
- [ ] All six subnets (3 loopbacks + 3 point-to-point links) appear in every router's `show ip route eigrp`
- [ ] `show ip protocols` confirms `no auto-summary` is active on all routers
- [ ] End-to-end pings between all three loopback addresses succeed
- [ ] Screenshot of `show ip eigrp neighbors` and `show ip route eigrp` saved for the GitHub write-up
