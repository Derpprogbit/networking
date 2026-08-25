For specifications and architecture details, see [[LAB 03 - Network Topology & Overview]].

### Step 1: Create VLANs & Assign Names
Declare VLAN 10, VLAN 20, and Native VLAN 99 on Switch1.

```cisconetworking
Switch1> enable
Switch1# configure terminal
! Create VLAN 10
Switch1(config)# vlan 10
Switch1(config-vlan)# name Users_VLAN10
Switch1(config-vlan)# exit

! Create VLAN 20
Switch1(config)# vlan 20
Switch1(config-vlan)# name Users_VLAN20
Switch1(config-vlan)# exit

! Create Native VLAN 99
Switch1(config)# vlan 99
Switch1(config-vlan)# name Management_Native99
Switch1(config-vlan)# exit
```

### Step 2: Configure Access Ports (PC2, PC3, PC4, PC5)
Assign host interfaces to their respective VLAN access groups.

```cisconetworking
! Assign Fa0/2 (PC2) & Fa0/3 (PC3) to VLAN 10
Switch1(config)# interface range FastEthernet0/2 - 3
Switch1(config-if-range)# switchport mode access
Switch1(config-if-range)# switchport access vlan 10
Switch1(config-if-range)# no shutdown
Switch1(config-if-range)# exit

! Assign Fa0/4 (PC5) & Fa0/5 (PC4) to VLAN 20
Switch1(config)# interface range FastEthernet0/4 - 5
Switch1(config-if-range)# switchport mode access
Switch1(config-if-range)# switchport access vlan 20
Switch1(config-if-range)# no shutdown
Switch1(config-if-range)# exit
```

### Step 3: Configure Trunk Port & Native VLAN on Fa0/1
Set up trunking on interface `Fa0/1` matching Switch0's native VLAN configuration.

```cisconetworking
Switch1(config)# interface FastEthernet0/1
! Enable trunk mode
Switch1(config-if)# switchport mode trunk
! Match native VLAN 99 to eliminate native VLAN mismatch errors
Switch1(config-if)# switchport trunk native vlan 99
Switch1(config-if)# no shutdown
Switch1(config-if)# exit
```

### Step 4: Configure Management SVI Interfaces
Assign SVI management IP addresses for VLAN 10 (`192.168.10.3/24`) and VLAN 20 (`192.168.20.1/24`).

```cisconetworking
! SVI for VLAN 10
Switch1(config)# interface vlan 10
Switch1(config-if)# ip address 192.168.10.3 255.255.255.0
Switch1(config-if)# no shutdown
Switch1(config-if)# exit

! SVI for VLAN 20
Switch1(config)# interface vlan 20
Switch1(config-if)# ip address 192.168.20.1 255.255.255.0
Switch1(config-if)# no shutdown
Switch1(config-if)# exit

Switch1(config)# do copy running-config startup-config
```
