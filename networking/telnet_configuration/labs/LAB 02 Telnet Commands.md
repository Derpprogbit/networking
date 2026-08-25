! 1. Complete Hostname & Set Secret Password
Router> enable
Router# configure terminal
Router(config)# hostname R1
R1(config)# enable secret class

! 2. Configure Management Interface
R1(config)# interface GigabitEthernet0/0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

! 3. Configure Local Console Access
R1(config)# line console 0
R1(config-line)# password cisco
R1(config-line)# login
R1(config-line)# exit

! 4. Configure Telnet Access (VTY Lines 0-4)
R1(config)# line vty 0 4
R1(config-line)# password cisco
R1(config-line)# login
R1(config-line)# transport input telnet
R1(config-line)# exit

! 5. Encrypt Passwords & Save Configuration
R1(config)# service password-encryption
R1(config)# end
R1# copy running-config startup-config