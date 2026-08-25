For specifications and architecture details, see [[LAB 03 - Network Topology & Overview]].

### Step 1: Create VLANs & Assign Names
Enter global configuration mode to declare VLAN 10 and Management/Native VLAN 99.

```cisconetworking
Switch0> enable
Switch0# configure terminal
! Create VLAN 10 for standard host traffic
Switch0(config)# vlan 10
Switch0(config-vlan)# name Users_VLAN10
Switch0(config-vlan)# exit

! Create VLAN 99 for trunk native traffic
Switch0(config)# vlan 99
Switch0(config-vlan)# name Management_Native99
Switch0(config-vlan)# exit
```

### Step 2: Configure Access Ports (PC0 & PC1)
Assign FastEthernet interfaces `Fa0/2` and `Fa0/3` to VLAN 10.

```cisconetworking
! Configure interface Fa0/2 for PC0
Switch0(config)# interface FastEthernet0/2
Switch0(config-if)# switchport mode access
Switch0(config-if)# switchport access vlan 10
Switch0(config-if)# no shutdown
Switch0(config-if)# exit

! Configure interface Fa0/3 for PC1
Switch0(config)# interface FastEthernet0/3
Switch0(config-if)# switchport mode access
Switch0(config-if)# switchport access vlan 10
Switch0(config-if)# no shutdown
Switch0(config-if)# exit
```

### Step 3: Configure Trunk Port & Native VLAN on Fa0/1
Set up port `Fa0/1` as an 802.1Q trunk connection and set Native VLAN to 99.

```cisconetworking
Switch0(config)# interface FastEthernet0/1
! Set interface mode to trunking
Switch0(config-if)# switchport mode trunk
! Assign Native VLAN 99 to untagged management frames
Switch0(config-if)# switchport trunk native vlan 99
Switch0(config-if)# no shutdown
Switch0(config-if)# exit
```

### Step 4: Configure Management SVI (VLAN 10)
Assign the SVI IP address `192.168.10.2/24` to enable switch management over VLAN 10.

```cisconetworking
Switch0(config)# interface vlan 10
Switch0(config-if)# ip address 192.168.10.2 255.255.255.0
Switch0(config-if)# no shutdown
Switch0(config-if)# exit
Switch0(config)# do copy running-config startup-config
```
