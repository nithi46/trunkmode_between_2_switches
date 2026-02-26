# trunkmode_between_2_switches
A two switches by activating trunk-mode four VLAN's with one MGMT-PC VLAN ;Configuring ssh on the switch

=================================
TWO SWITCHES CONFIGURATION IN CLI
=================================


Switch(config-vlan)#name MGMT-PC
Switch(config-vlan)#exit
Switch(config)#interface range fa0/4
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switch access vlan 99
Switch(config-if-range)#exit

	
Switch(config)#interface vlan 99
Switch(config-if)#
%LINK-5-CHANGED: Interface Vlan99, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan99, changed state to up

Switch(config-if)#ip address 192.168.99.1 255.255.255.0
Switch(config-if)#no sh
Switch(config-if)#exit
Switch(config)#interface vlan 10
Switch(config-if)#
%LINK-5-CHANGED: Interface Vlan10, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan10, changed state to up
Switch(config)#hostname Switch1
Switch1(config)#ip domain-name demo.local
Switch1(config)#username admin password admin@12
Switch1(config)#enable password admin12
Switch1(config)#service password-encryption
Switch1(config)#exit
Switch1#
%SYS-5-CONFIG_I: Configured from console by console

Switch1#exit

Switch1 con0 is now available

Press RETURN to get started.


Switch1>enable
Password: 
Switch1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch1(config)#ip domain-name demo.local	
Switch1(config)#crypto key generate rsa
The name for the keys will be: Switch1.demo.local
Choose the size of the key modulus in the range of 360 to 2048 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]

Switch1(config)#ip ssh version 2
*Mar 1 0:49:17.549:  %SSH-5-ENABLED: SSH 1.99 has been enabled 
Switch1(config)#line vty 0 4
Switch1(config-line)#transport input ssh
Switch1(config-line)#login local
Switch1(config-line)#exit
Switch1(config)#exit
Switch1#
%SYS-5-CONFIG_I: Configured from console by console

Switch1#write
Building configuration...
[OK]

Switch1#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23, Fa0/24
                                                Gig0/1, Gig0/2
10   HR                               active    Fa0/1, Fa0/2
20   SALES                            active    Fa0/3
30   IT                               active    
99   MGMT-PC                          active    Fa0/4
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
Switch1#show ip ssh
SSH Enabled - version 2.0
Authentication timeout: 120 secs; Authentication retries: 3
Switch1#


Switch1#show running-config
Building configuration...

Current configuration : 1558 bytes
!
version 12.2
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname Switch1
!
enable password 7 0820484300175445
!
!
!
ip ssh version 2
ip domain-name demo.local
!
username admin privilege 1 password 7 0820484300175445
!
!
spanning-tree mode pvst

 --More-- 

Switch1>
Switch1>en
Password: 
Switch1#
Switch1#show interface trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/24      on           802.1q         trunking      99
Gig0/1      on           802.1q         trunking      1

Port        Vlans allowed on trunk
Fa0/24      1-1005
Gig0/1      1-1005

Port        Vlans allowed and active in management domain
Fa0/24      1,99
Gig0/1      1,99

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/24      1,99
Gig0/1      1,99
Switch1#


-----

Switch>
Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname Switch0
Switch0(config)#username admin12 password admin@12
Switch0(config)#enable password admin@12
Switch0(config)#service password-encryption
Switch0(config)#exit

Switch0>en
Password: 
Switch0#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch0(config)#vlan 10 
Switch0(config-vlan)#name HR
Switch0(config-vlan)#end
Switch0#
%SYS-5-CONFIG_I: Configured from console by console

Switch0#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch0(config)#vlan 20 
Switch0(config-vlan)#name SALES
Switch0(config-vlan)#exit
Switch0(config)#vlan 30
Switch0(config-vlan)#name IT
Switch0(config-vlan)#exit
Switch0(config)#interface fa0/1
Switch0(config-if)#switchport mode access
Switch0(config-if)#switchport access vlan 10
Switch0(config-if)#exit
Switch0(config)#interface fa0/2
Switch0(config-if)#switchport mode access
Switch0(config-if)#switchport access vlan 10
Switch0(config-if)#exit
Switch0(config)#interface fa0/3
Switch0(config-if)#switchport mode access
Switch0(config-if)#switchport access vlan 20
Switch0(config-if)#exit
Switch0#show running-config
Building configuration...

Current configuration : 1625 bytes
!
version 12.2
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname Switch0
!
enable password 7 082048430017254640
!
!
!
ip ssh version 2
ip domain-name demo.local
!
username admin12 privilege 1 password 7 082048430017254640
!
!
spanning-tree mode pvst
!
interface FastEthernet0/1
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/2
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/3
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/4
 switchport access vlan 30
 switchport mode access
!
interface FastEthernet0/5

:
:

interface FastEthernet0/24
 shutdown
!
interface GigabitEthernet0/1
 switchport mode trunk
!
interface GigabitEthernet0/2
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan10
 ip address 192.168.10.11 255.255.255.0
!
interface Vlan20
 ip address 192.168.20.10 255.255.255.0
!
interface Vlan30
 ip address 192.168.30.10 255.255.255.0
!
line con 0
!
line vty 0 4
 login local
 transport input ssh
line vty 5 15
 login
!
!
end

Switch0#show interface trunk
Port        Mode         Encapsulation  Status        Native vlan
Gig0/1      on           802.1q         trunking      1

Port        Vlans allowed on trunk
Gig0/1      1-1005

Port        Vlans allowed and active in management domain
Gig0/1      1,10,20,30

Port        Vlans in spanning tree forwarding state and not pruned
Gig0/1      1,10,20,30
Switch0#


---------------

VERIFICATIONS(SWITCH-CLI):
#show ip interface brief
#show ip ssh
#show running-config
#show vlan brief

==================
PC CONFIGURATION
==================

configure each PCs with IP Addr. , Subnet mask, Default gateway 

or ipconfig <IP_address> <Subnet_Mask> <Default_Gateway>

and verify it by running [ipconfig] in CLI 

==============
SSH TO SWITCH
==============
from PC-4 (MGMT-PC) desktop-> CLI

ssh -l admin 192.168.99.1
password:

PC>
