# Lab 05: EIGRP Configuration & Verification

**CCNA Lab Portfolio | Cisco Packet Tracer Documentation**

| | |
|---|---|
| **Author** | Luigi |
| **Date** | August 30, 2026 |
| **Program** | BS Information Technology |
| **Tool** | Cisco Packet Tracer |
| **Protocol** | EIGRP (Enhanced Interior Gateway Routing Protocol) |
| **Previous Lab** | Lab 04 — Single-Area OSPFv2 Configuration & Verification |

---

## 1. Objectives

By the end of this lab, I should be able to:

- Explain what EIGRP is and how it differs from OSPF (which I configured in Lab 04).
- Enable classic EIGRP on Cisco IOS routers using an Autonomous System (AS) number.
- Advertise networks correctly using the `network` command and wildcard masks.
- Prevent unnecessary EIGRP hellos on LAN-facing interfaces using `passive-interface`.
- Disable automatic summarization for discontiguous networks using `no auto-summary`.
- Verify neighbor adjacencies, the topology table, and the routing table using EIGRP-specific `show` commands.
- Troubleshoot common EIGRP adjacency problems (AS mismatch, K-value mismatch, subnet mismatch).

## 2. What is EIGRP?

EIGRP is a Cisco-proprietary (now partially open, RFC 7868) **advanced distance-vector routing protocol**. Unlike OSPF, which is a link-state protocol that floods full topology information and calculates paths with Dijkstra's SPF algorithm, EIGRP uses the **Diffusing Update Algorithm (DUAL)** to calculate loop-free paths and keeps a Successor, Feasible Successor, and full topology table per network.

Key characteristics:

- **Administrative Distance:** 90 (internal EIGRP), 170 (external EIGRP) — lower (more trusted) than OSPF's 110.
- **Metric:** Composite metric based on Bandwidth and Delay by default (K1 and K3), with Reliability, Load, and MTU available but rarely used.
- **Convergence:** Very fast, because DUAL pre-computes backup (Feasible Successor) routes.
- **Neighbor discovery:** Multicast Hello packets to 224.0.0.10, every 5 seconds on most links.
- **No concept of "areas"** like OSPF — it uses a single Autonomous System (AS) number that just has to match between neighbors.

## 3. Why this lab matters

Lab 04 covered OSPF, which is the industry-standard link-state protocol used in multi-vendor networks. This lab covers EIGRP so I can compare both on the same style of topology and understand *when* an engineer would pick EIGRP (Cisco-only shops, simpler configuration, faster convergence, easy VLSM/unequal-cost load balancing) versus OSPF (multi-vendor environments, more granular area design).

## 4. Lab Files in this Repository

| File | Purpose |
|---|---|
| `00-Overview-and-Author.md` | This file — objectives, background, author info |
| `01-Topology-and-Addressing.md` | Network diagram and IP addressing table |
| `02-CLI-Commands-and-Configuration.md` | Full step-by-step CLI configuration with EIGRP command explanations |
| `03-Verification-and-Testing.md` | Verification commands, expected output, and troubleshooting |
| `export/lab05_eigrp_guide.pdf` | Single-file PDF export of the full lab (all sections combined) |
