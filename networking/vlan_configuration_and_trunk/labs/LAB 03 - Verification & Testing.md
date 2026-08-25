For specifications and architecture details, see [[LAB 03 - Network Topology & Overview]].

### Step 1: Verify VLAN Configuration
Confirm that all created VLANs are active and assigned to correct ports.

```cisconetworking
Switch0# show vlan brief
Switch1# show vlan brief
```

* Expected Outcome: Ports `Fa0/2` and `Fa0/3` appear under VLAN 10. On Switch1, `Fa0/4` and `Fa0/5` appear under VLAN 20.

### Step 2: Verify Trunk Link Status
Ensure trunk encapsulation and native VLAN assignments are properly established on `Fa0/1`.

```cisconetworking
Switch0# show interfaces trunk
Switch1# show interfaces trunk
```

* Expected Outcome: `Fa0/1` is listed with Mode `on`, Encapsulation `802.1q`, Trunking Status `trunking`, and Native VLAN `99`.

### Step 3: Connectivity Testing (Ping Execution)

1. **Intra-VLAN Connectivity Test (PC0 to PC2):**
   Open Command Prompt on **PC0** (`192.168.10.4`) and execute:
   ```cmd
   ping 192.168.10.6
   ```
   * Result: **Successful (0% loss)** due to matching VLAN 10 membership across the trunk.

2. **Inter-VLAN Connectivity Test (PC0 to PC4):**
   Open Command Prompt on **PC0** (`192.168.10.4`) and attempt to ping PC4 (`192.168.20.2`):
   ```cmd
   ping 192.168.20.2
   ```
   * Result: **Timed Out (100% loss)**. Layer 2 switches do not route between distinct VLANs without a Layer 3 device (Router-on-a-Stick or L3 Switch).
