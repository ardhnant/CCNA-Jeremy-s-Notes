
## Basic commands

`enable` - privileged exec mode
`configure terminal` - global config
`exit` - step back
`copy running-config startup-config` or `write` or `write memory` - saving your progress in startup config
`hostname <hostname>` - to configure a hostname 
`show version` - to know about the router in detail 
`show running-config` - show the running config
`show running-config interface g0/1` - show running config just for interface g0/1
`show running-config | section interface g0/1` - show running config and pipe it into the next command which tell it to show only the section under interface g0/1
`show running-config | include interface g0/1` - show running config and pipe it into the next command which tell it to show only the commands which include the key word interface g0/1
`ping <ip address>` - send ICMP packets to a interface via IP address 
`interface <interface name>` - to enter interface configuration mode (gc)
`interface range <range (f0/5-12)>` - configure several interfaces at ones 

### Security 

`enable password <pwd>` - store password in text without encryption
`service password-encryption` - changes the display (encrypt it) not the actual password
`enable secret <pwd>` - store the password in MD5 (type -5) encryption 

### Undoing-doing commands

`no` - prefix is used to remove previously configured command 
	example - `no service password-encryption`
`do` - execute any command from global config mode which was meant to be executed from privilege config mode
	example - `do show ip interface`

## Basic switches command

`speed <speed>` - set the speed to data transmission 
`duplex <full/auto/half` - determines if sending and receiving can occur simultaneously

## MAC Address Commands 

`show arp` - show arp table 
`show mac address-table` - show mac address-table
`clear mac address-table dynamic` - clear all dynamic entries in mac table 
`clear mac address-table dynamic address <mac>` - to clear a specific mac address table
`clear mac address-table dynamic interface <int>` - clear all the entries learned from the specific interface

## IP related commands

`ip address A.B.C.D MASK` - assign IP address (igc)
`shutdown` and `no shutdown` - to disable and enable an interface res. (igc) 
`show ip interface brief` - quick interface status and IP address check (p)
`show ip interface status` - check the status of the interface (up/down)
`show interfaces` - detailed information about a specific interface 
`show interface <interface name>` - detailed information about a specific interface
`show interface description` - show description of all the interfaces
`description <description>` - add interface description 

## Routing Commands

`show ip route` - display the routing table

### Static Routing

`ip route <destination network> MASK <next hop ip addr>` - configuring a static route 
`ip route <destination network> MASK <exit int>` - configuring static route via next hop as a interface 
`ip route <destination network> MASK <exit int> <next hop ip addr>` - configuring static route via next hop as a interface 
`ip route 0.0.0.0 0.0.0.0 <next hop ip>` - configuring default ip addr

### Floating Static Route

`ip route <destination network> MASK <next hop ip addr> <cost>` - configuring floating static routes

### Dynamic Routing

`interface loopback0` or `int l0` - enter in the loopback interface configuring mode
`show ip protocols` - show which routing protocol you are using and other information related to it
`no auto-summary` - remove the summarising of the network mask
`network <ip addr> {wildcard mask}` - IP addr you wanna include 
`default-information orignate` - advertise the default route / static routes you configured
`passive-interface <int>` - router does not advertise on passive interface 
`router id <ip addr>` - configure the router ID manually

#### RIP 

`router rip` - enter into routing configuring mode of RIP 
`version 2` - tell the router to use RIPv2

#### EIGRP

`router eigrp <AS number>` - enter into routing configuring mode of EIGRP 

#### OSPF 

`router ospf <process ID>` - enter into routing configuring mode of OSPF
`network <network name> <wildcard mask> area <area no.>` - IP address you wanna include with the area you wanna put them in
`router id <ip addr>` - to set router ID
`clear ip ospf process` - to clear OSPF process

`show ip ospf interface <int>` - show more OSPF related information about the interface 
`show ip interface brief` - give a brief information about all interface about OSPF protocol
`show ip ospf neightbor` - to see the neighbor relationship wrt OSPF for all interfaces
`show ip ospf database` - see the database containing all the IP's 

`auto-cost reference-bandwidth <megabits-per-second>` - change the bandwidth of the router 
`bandwidth <kb-ps>` - change the bandwidth on the interface config mode
`ip ospf cost <cost>` - manual configuration of cost
`ip ospf <process id> area <area>` - configuring OSPF from interface config mode
`passive-interface default` - configure all interfaces as passive interfaces by default
`ip ospf priority <priority>` - configure the priority of an interface
`ip ospf network {broadcast | point-to-point | non-broadcast}` - configure the OSPF network type

`ip ospf hello-interval <time in seconds>` - configure the hello time interval
`ip ospf dead-interval <time in seconds>` - configure the dead time interval manually

`ip ospf authentication-key <pwd>` - to set a password for ospf authentication on an interface
`ip ospf authentication` - to enable the authentication on the interface

`ip mtu <size in bytes>` - configure the mtu size manually


##### Serial Interfaces

`clock rate <in bits>` - set the speed on DCE side
`encapsulation {hdlc|ppp}` - for encapsulation 
`show controller serial <whatever int>` - 

## VLAN

`show vlan brief` - display existing vlans and assigned interfaces
`vlan <vid>` - to enter vlan configuring mode
`name <name>` - to name the vlan (vgc)

### Access port command

`switchport mode access` - to change the switchport mode to access (igc) 
`switchport access vlan <vid>` - create a vlan with the given vid on the interface operating on with switchport mode access

### Trunk port command

`switchport trunk encapsulation dot1q` - change encapsulation trunk protocol to 802.1Q
`switchport mode trunk` - to change the switchport mode to trunk (igc)
`show interface trunk` - show the trunk interfaces, mode, encapsulation type etc
`switchport truck allowed vlan 10,30` - add specific vlan
`switchport truck allowed vlan add 20` - add vlan 20
`switchport trunk allowed vlan remove 20` - remove vlan 20
`switchport trunk allowed vlan all` - allow all vlan
`switchport truck allowed vlan except 1-5,10` - allow all except specific vlans mentioned
`switchport trunk allowed vlan none` - allow none vlan

`switchport trunk native vlan 1001` - configure the native vlan 

### ROAS Configuration 

#### **Steps**

1. Enable physical interface

```
no shutdown
```

2. Create subinterface

```
interface g0/0.10
```

3. Assign VLAN tag

```
encapsulation dot1q 10
```

4. Assign IP address

```
ip address X.X.X.X MASK
```

5. Make a native VLAN

```
encapsulation dot1q {vlan-id} native
```


### Removing ROAS

`no interface <sub interface>` - remove the sub interface
`default interface g0/0` - defaulting the g0/0 interface to default (how it was before configuring the ROAS)

### SVI 

`ip routing` - enables layer 3 routing on this switch 
`no switchport` - configures the switch from switchport to routed port
`ip route 0.0.0.0 0.0.0.0 <next hop ip>` - default route on SVI (gc)
`interface vlan 1` then `ip add <whatever>` - it does not need no switchport command to configure svi here
`ip default-gateway <ip add>` - to configure default gateway on the switch (switch own traffic will follow this gateway not the pc like ping, snmp etc)

### DTP 

`switchport mode dynamic {auto | desirable}` - configure the interface to use DTP. (igc)
`switchport nonegotiate` - to disable the DTP 

### VTP

`vtp mode {server | client | transparent}` - to configure the switch into desired VTP mode (gc)
`show vtp status` - show the vtp status

## STP 

`show spanning-tree` - provide information about the spanning tree protocol
`show spanning-tree vlan <vid>` - show information about the spanning tree protocol for a specific vlan
`show spanning-tree detail` - show information about the spanning tree protocol for all the port in the switch
`show spanning-tree summary` - provide summary about all the interfaces in the switch
`spanning-tree mode {pvst | mst | rapid-pvst}` - configure the spanning tree 
`spanning-tree link-type {point-to-point | shared}` - configure the link type for a collision domain

`spanning-tree vlan <vid> root primary` - set the following switch as root switch for the specified VLAN
`spanning-tree vlan <vid> root secondary` - set the following switch as secondary root switch for the specified VLAN

`spanning-tree vlan <vid> priority 24576` - manually configure priority for a switch for a vlan
`spanning-tree vlan <vid> cost 100` - manually configure the cost of the switch for the specific vlan

#### Portfast, BPDU Guard, BPDU Filter

`spanning-tree portfast` - configure the port as portfast
`spanning-tree portfast default` - enable portfast on all the existing access port
`spanning-tree portfast disable` - to disable portfast on a specific access port

`spanning-tree bpduguard enabled` - enable bpdu guard in a specific interface (igc)
`spanning-tree portfast bpduguard default` -  enable BPDU Guard on all the portfast-enabled interfaces (gc)
`spanning-tree bpduguard disable` - disable BPDU Guard on a specific interface (igc)
`errdisable recovery cause <cause>` - enable the errdisable for the specific cause
`errdisable recovery interval <in seconds>` - configure the interval it will recover after errdisable in seconds

`spanning-tree bpdufilter enable` -  enable bpdufilter in a specific interface (igc)
`spanning-tree portfast bpdufilter default` -  enable BPDU Filter on all the portfast-enabled interfaces (gc)
`spanning-tree bpdufilter disable` - disable BPDU Filter on a specific interface (igc)

## Etherchannel

`show etherchannel summary` - 
`show ether-channel load-balance` - display the configuration of the load-balance 
`port-channel load-balance method` - to change the method of load balancing 

`channel-group <channel no.> mode {auto | desirable}` - configure etherchannel via PAgP
`channel-group <channel no.> mode {active | passive}` - configure etherchannel via LACP
`channel-group <channle no.> mode {on | off}` - configuring static etherchannel 

## IPv6 

`ipv6 unicast-routing` - enable ipv6 routing
`ipv6 address <ip add> <CIDR notation>` - manually configure ipv6 addr
`ipv6 address <ip add> <CIDR notation> anycast` - manually configure anycast ipv6 addr
`ipv6 address <first 64 bit of ip addr> eui-65` - configuring ipv6 IP address via EUI-64 protocol
`ipv6 address autoconfig` - 

`show ipv6 interface brief` - show IP addr and the status of the interface (up/down) Secure Shell
`show ipv6 neighbor` - shows neighboring interfaces along with their IP address

## NAT

`ip nat inside` - define the 'inside' interface(s) connected to the internal network.
`ip nat outside` - define the outside interface(s) connected to the external network.

`ip nat inside source static 10.0.0.1 192.168.0.1` - configure the one-to-one IP address

`show ip nat translation` - show all the configured NAT.
`show ip nat statistics` - detailed information about nat and other stats.

`access-list 1 permit 192.168.0.0 0.0.0.0 0.0.0.255` - an access list which you can apply on nat as a security measure.
`ip nat pool POOL 10.0.0.0 10.0.0.255 prefix-length 8` - define a pool of inside global IP address.
`ip nat inside source list 1 pool POOL` - configure dynamic nat by mapping ACL to the pool
`ip nat inside source list 1 pool POOL overlord` - configure PAT by mapping ACL to the pool
`ip nat inside source list 1 interface g0/0 overlord` - configuring PAT to a interface

## Port Security 

`switchport port-security` - enable port security on a interface config mode
`switchport port-security mac-address <mac add>` - to preconfigure mac address
`switchport port-security violation {shutdown|restrict|Protect}` - configure the port-security violation
`switchport port-security aging type {absolute|inactivity}` - configure the aging type
`switchport port-security maximum <no.>` - configure how many mac address to allow 
`switchport port-security aging time <time in min>` - configure time 
`switchport port-security mac-address sticky` - to configure stick in port security

`show port-security interface g0/1` - to see port security stats
`errdisable recovery cause psecure-violation` - to enable errdisable for port security
`show mac address-table secure` - to see all the sticky and static mac addresses

## console

`line console 0` - there is only one console line so no. is gonna be 0 all the time.
`password <passwd>` - after entering console configuring mode configure the passwd
`login` - tell the device to require user to enter the configured password to access the cli via the console port

`username jeremy secret ccna` - create a user with name jeremy and pass ccna
`login local` - inside console configuration mode to use one of the user which is locally configured


## telnet

`line vty 0 15` - Telnet/SSH access is configured on the VTY lines. There are 16 lines available, so up to 16 users can be connected at once.
`transport input {telnet|ssh|telnet ssh|all|none}` - pretty self explanatory 
`exec-timeout 5 20` - logout after 5min 20seconds of inactivity
`access-class 1 in` - apply the access list 1 
`telnet ip` - to enter via telnet

## ssh

`ip domain name lanel.com` - configure domain name on global config mode
`crypto key generate rsa` - generate rsa key which helps in the encryption of the packet
`ip ssh version 2` - set the version to 2
`ssh -l <username> <ip add>` or `ssh username@ip` - to enter via ssh