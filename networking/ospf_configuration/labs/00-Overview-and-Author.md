---
author: BSIT Student
date: 2026-08-30
lab_id: LAB-04
topic: Single-Area OSPFv2 Configuration and Verification
environment: Cisco Packet Tracer v8.x
---
[[!INFO]]
1.[[01-Topology-and-Addressing]]
2.[[02-CLI-Commands-and-Configuration]]
3.[[03-Verification-and-Testing]]
# Lab 04: Single-Area OSPFv2 Configuration and Verification

## 1. Overview
This lab demonstrates the fundamental setup and verification of Open Shortest Path First version 2 (OSPFv2) in a single-area (Area 0) enterprise topology. The scenario connects two branch routers over a point-to-point `/30` WAN network and routes traffic between two local area networks (LANs).

## 2. Lab Objectives
- Configure point-to-point IP addressing on router serial/Ethernet interfaces.
- Calculate and apply wildcard masks for `/30` and `/24` subnets.
- Configure manual OSPF Router IDs (`1.1.1.1` and `2.2.2.2`).
- Establish OSPF neighbor adjacencies across Area 0 (Backbone).
- Configure passive interfaces on LAN ports for security best practices.
- Verify dynamic routing table updates and end-to-end ICMP reachability.

## 3. High-Level Topology
```
[ PC0: 192.168.1.10/24 ]
           |
   (Gi0/0/1: 192.168.1.1)
        [ R1 ] (Router ID: 1.1.1.1)
   (Gi0/0/0: 10.0.0.1/30)
           |
       [ Area 0 ] (10.0.0.0/30 WAN Link)
           |
   (Gi0/0/0: 10.0.0.2/30)
        [ R2 ] (Router ID: 2.2.2.2)
   (Gi0/0/1: 192.168.2.1)
           |
[ PC1: 192.168.2.10/24 ]
```
