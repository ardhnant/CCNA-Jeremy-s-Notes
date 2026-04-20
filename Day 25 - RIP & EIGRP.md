## RIP

- **Routing Information Protocol** (industry standard)
- Distance vector IGP (uses routing-by-rumor logic to learn/share routes)
- The maximum hop count is 15 (anything more than that is considered unreachable)
- Has three versions:
	- RIPv1 and RIPv2, used for IPv4
	- RIPng (RIP Next Generation), used for IPv6
- Uses two messages types:
	- Request: To ask RIP-enabled neighbor routers to send their routing table.
	- Response: To send the local router's routing table to neighboring routers
- By default, RIP-enabled routers will share their routing table every 30 seconds

### RIPv1


- only advertises **classful** addresses (Class A, Class B, Class C)
- doesn’t support VLSM, CIDR
- doesn’t include subnet mask information in advertisements (Response messages)
	- 10.1.1.0/24 will be become 10.0.0.0 (Class A address, so assumed to be /8)
	- 172.16.192.0/18 will become 172.16.0.0 (Class B address, so assumed to be /16)
	- 192.168.1.4/30 will become 192.168.1.0 (Class C address, so assumed to be /24)
- messages are broadcast to 255.255.255.255


### RIPv2
    
- supports VLSM, CIDR
- includes subnet mask information in advertisements
- messages are **multicast** to 224.0.0.9



> Broadcast messages are delivered to **all devices** on the local network.


> Multicast messages are delivered **only to devices that have joined that specific multicast group.**


### RIP Configuration

![[Pasted image 20260308155002.png]]

- The RIP 'network' command is classful, it will automatically convert to classful networks.
- For example, even if you enter the command network 10.0.12.0, it will be converted to network 10.0.0.0 (a A class network)
- There is no need to enter the network mask.

![[Pasted image 20260308155243.png]]

```cisco
//All the commands are right here
R1(config)#router rip
R1(config-router)#version 2
R1(config-router)#no auto-summary
R1(config-router)#network <network you wanna include>
```


## The network command

![[Pasted image 20260308161456.png]]

- The network command tells the router to:
	- look for interfaces with an IP address that is in the specified range.
	- active IP on the interfaces that fall in the range.
	- form adjacency with connected RIP neighbors.
	- advertise the network prefix of the interface (NOT the prefix in the network command)

- The OSPF and EIGRP network commands operate in the same way.

- Because the network command is classful, 10.0.0.0 is assumed to be 10.0.0.0/8
- R1 will look for any interfaces with an IP address that matches 10.0.0.0/8 (because it is /8 it only needs to match the first 8 bits)
- 10.0.12.1 and 10.0.13.1 both match, so RIP is activated on G0/0 and G1/0
- R1 forms adjacency with its neighbors R2 and R3.
- R1 advertise 10.0.12.0/30 and 10.0.13.0/30 (NOT 10.0.0.0/8).

> The network command doesn't tell the router which networks to advertise. It tells the router which interfaces to activate RIP on, and then the router will advertise the network prefix of those interfaces.



`network 172.16.0.0`

- Because the **network command is classful**, 172.16.0.0 is assumed to be 172.16.0.0/16
- R1 will look for any interfaces with an IP address that matches 172.16.0.0/16
- 172.16.1.14 matches, so R1 will activate RIP on G2/0
- There are no RIP neighbors connected to G2/0, so no new adjacencies are formed
- R1 advertises 172.16.1.0/28 (NOT 172.16.0.0/16) to its RIP neighbors

>Although there are no RIP neighbors connected to G2/0, R1 will continuously send RIP advertisements out of G2/0. This is unnecessary traffic, so G2/0 should be configured as a **passive interface**.

### The passive-interface command# Day 25 Lab | CCNA 20

- The passive-interface command tells the router to stop sending RIP advertisements out of the specified interface (G2/0)
- However, the router will continue to advertise the network prefix of the interface (172.16.1.0/28) to its RIP neighbors (R2, R3)
- You should always use this command on interfaces which don't have any RIP neighbors.
- EIGRP AND OSPF both have the same passive interface functionality, using the same command.

![[Pasted image 20260308161503.png]]

![[Pasted image 20260308161517.png]]

- To advertise default route to other routers we need to execute the command `default-information ofiginate` 

![[Pasted image 20260308161703.png]]

- To see the IP Protocols you can execute the command called `show ip protocols` 

![[Pasted image 20260308161837.png]]

- Maximum Path means how many path of equal cost can routing table hold at a time.

- Distance of a Protocol can be changed from the command `distance <distance you want>` in (config-router mode)

## EIGRP 

- Enhanced Interior Gateway Routing Protocol
- Was Cisco proprietary, but Cisco has now published it openly so other vendors can implement it on their equipment.
- Consider an 'advanced'/'hybrid' distance vector routing protocol.
- Much faster than RIP in reacting to changes in the network.
- Does not have 15 'hop-count' limit of RIP.
- Sends messages using mulicast address 224.0.0.10
- Is the only IGP that  can perform unequal-cost load-balancing (by default it performs ECMP load-balancing over 4 path like RIP).

## EIGRP Configuration 

![[Pasted image 20260308185217.png]]

![[Pasted image 20260308185240.png]]

- The AS (Autonomous System) number must match between routers, or they will not form an adjacency and share route information. In the command `router eigrp 1` '1' is the AS number.
- Auto-summary might be enabled or disabled by default, depending on the router/IOS version. If it's enabled, disable it.
- The network command will assume a classful address if you don't specify the mask.
- EIGRP uses a **wildcard mask** instead of a regular subnet mask.

### Wildcard Mask

- Wildcard mask is basically an 'inverted subnet mask'.
- All 1s in the subent mask are 0 in the equivalent wildcard mask. All 0s in the subnet mask are 1 in the equivalent wildcard mask.

1 . ![[Pasted image 20260308185839.png]]

- Before the network mask was 255.255.255.0 but in wildcard mask 0 become 1 and 1 become 0 so new mask is 0.0.0.255

2 . ![[Pasted image 20260308190110.png]]

- In here same as above so new mask becomes 0.0.0.15 which is equivalent to /28.

- A shortcut is to subtract each octet of the subnet mask from 255.

![[Pasted image 20260308190239.png]]

> '0' in the wildcard mask means that bits must match while '1' means that it doesn't have to match.

> For more information go watch RIP & EIGRP video of jeremy's it lab at 27:18 for more information.


### show ip protocols for EIGRP

![[Pasted image 20260308191719.png]]

Router ID order of priority:
1. Manual configuration 
2. Highest IP address on a loopback interface
3. Highest IP address on a physical interface

![[Pasted image 20260308191933.png]]

- Router ID is nothing but just a name. It does not mean anything to anything.
![[Pasted image 20260308192159.png]]


**To configure loopback ip** you can use the following command:
`interface loopback0 or int l0` 
`ip address 1.1.1.1 255.255.255.255`

## Quiz

1. ![[Pasted image 20260308192633.png]]

2. ![[Pasted image 20260308192705.png]]

3. ![[Pasted image 20260308192732.png]]

4. ![[Pasted image 20260308192820.png]]
