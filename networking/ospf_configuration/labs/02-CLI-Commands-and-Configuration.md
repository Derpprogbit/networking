# CLI Commands and Configuration

## 1. Router 1 (R1) Configuration

```ciscolike
! --- Interface Configuration ---
enable
configure terminal
hostname R1

interface GigabitEthernet0/0/0
 description WAN Connection to R2
 ip address 10.0.0.1 255.255.255.252
 no shutdown
exit

interface GigabitEthernet0/0/1
 description LAN Connection to PC0 Network
 ip address 192.168.1.1 255.255.255.0
 no shutdown
exit

! --- OSPF Dynamic Routing Configuration ---
router ospf 1
 router-id 1.1.1.1
 network 10.0.0.0 0.0.0.3 area 0
 network 192.168.1.0 0.0.0.255 area 0
 passive-interface GigabitEthernet0/0/1
end

write memory
```

## 2. Router 2 (R2) Configuration

```ciscolike
! --- Interface Configuration ---
enable
configure terminal
hostname R2

interface GigabitEthernet0/0/0
 description WAN Connection to R1
 ip address 10.0.0.2 255.255.255.252
 no shutdown
exit

interface GigabitEthernet0/0/1
 description LAN Connection to PC1 Network
 ip address 192.168.2.1 255.255.255.0
 no shutdown
exit

! --- OSPF Dynamic Routing Configuration ---
router ospf 1
 router-id 2.2.2.2
 network 10.0.0.0 0.0.0.3 area 0
 network 192.168.2.0 0.0.0.255 area 0
 passive-interface GigabitEthernet0/0/1
end

write memory
```

## 3. Alternative Modern Interface-Level Method
Instead of calculating wildcard masks using the `network` command under `router ospf`, you can enable OSPF directly on the interface level:

```ciscolike
R1(config)# interface GigabitEthernet0/0/0
R1(config-if)# ip ospf 1 area 0
```
