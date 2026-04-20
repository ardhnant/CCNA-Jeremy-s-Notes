## OSPF Cost

- OSPF's metric is called cost.
- It is automatically calculated based on the bandwidth (speed) of the interface.
- It is calculated by dividing a reference bandwidth value by the interface's bandwidth.
- The default reference bandwidth in 100 mbps.
	**Reference:** 100 mbps/Interference: 10 mbps = cost of 10
	**Reference:** 100 mbps/Interference: 100 mbps = cost of 1
	**Reference:** 100 mbps/Interference: 1000 mbps = cost of 1??
	**Reference:** 100 mbps/Interference: 10000 mbps = cost of 1??
- All values less than 1 will be converted of 1.
- Therefore FastEthernet, Gigabit Ethernet, 10Gig Ethernet, etc. are equal and all have cost of 1 by default.

![[Pasted image 20260309231100.png]]

![[Pasted image 20260309231117.png]]

- You can (and should!) change the reference bandwidth with his command: 
	`R1(config-router)#auto-cost reference-bandwidth <megabits-per-second>` 

![[Pasted image 20260309231253.png]]

- The command is entered in megabits per second (default is 100)

- 100000/100 = cost of 1000 for FastEthernet
- 100000/1000 = cost of 100 for Gig Ethernet

- You should configure a reference bandwidth greater than the fastest links in your network (to allow for future upgrades)

- You should configure the same reference bandwidth on all OSPF routers in the network.

- The OSPF cost to a destination is the total cost of the 'outgoing/exit interface'

#### Example

For example, **R1’s cost to reach 192.168.4.0/24 is:**

```
100 (R1 G0/0) + 100 (R2 G1/0) + 100 (R4 G1/0) = 300
```


#### Loopback Interface Cost

Loopback interfaces have a **cost of 1**.

![[Pasted image 20260309231807.png]]

![[Pasted image 20260309232115.png]]

![[Pasted image 20260309232130.png]]


--- 

- One more option to change the OSPF cost of an interface is to change the bandwidth of the interface with the bandwidth command.
- The formula to calculate OSPF cost is **reference bandwidth/interface bandwidth**
- Although the bandwidth matches the interface speed by default, changing the interface bandwidth *doesn't actually change the speed at which the interface operates.*
- The bandwidth is just a value that is used to calculate OSPF cost, EIGRP metric, etc.
- To change the speed at which the interace operates, use the **speed** command.
- Because the bandwidth value is used in other calculations, it is not recommended to change this value to alter the interface's OSPF cost.
- It is recommended that you change the reference bandwidth, and then use the ip ospf cost command to change the cost of individual interfaces if you want.

![[Pasted image 20260309232741.png]]

### Command Overview 

- Three ways to modify the OSPF cost:

	1. Change the reference bandwidth:
	`R1(config-router)#auto-cost reference-bandwitdh <bandwidth in Mbps>`
	2. Manual Configuration 
	`R1(config-router)#ip ospf cost <cost>`
	3. Change the interference bandwidth 
	`R1(config-router)#bandwidth <Kbps>

![[Pasted image 20260309233311.png]]

## OSPF Neighbors 

- Making sure that routers successfully become OSPF neighbors is the main task in configuring and troubleshooting OSPF.
- Once routers become neighbors, they automatically do the work of sharing network information, calculating routes, etc.
- When OSPF is activated on an interface, the router starts sending OSPF **hello** messages out of the interface at regular interfaces at regular intervals (determined by the **hello timer**). These are used to introduce the router to potential OSPF neighbors.
- The default hello timer is 10 seconds on an Ethernet connection.
- Hello messages are multicast to **224.0.0.5** (multicast address for all OSPF routers)

- OSPF messages are encapsulated in an IP header, with a value of 89 in the protocol field.

## States of OSPF Neighbors

**Down State**
- OSPF is activated of R1's G0/0 interface.
- It sends an OSPF hello message to 224.0.0.5.
- It doesn't know about any OSPF neighbors yet, so the current neighbor state is **Down**.

![[Pasted image 20260309235048.png]]

**Init State**
- When R2 receives the Hello packet, it will add an entry for R1 to its OSPF neighbor table.
- In R2's neighbor table, the relationship with R1 is now the **Init** state.

![[Pasted image 20260309235255.png]]

- Init state = Hello packet received, but own router ID is not in the Hello packet

**2-way state**
- R2 will send a Hello packet containing the RID of both routers.
- R1 will insert R2 into its OSPF neighbors table in the 2-way state.

- R1 will send another Hello message, this time containing R2's RID.
 Now both routers are in the 2-way state.

![[Pasted image 20260309235637.png]]

- The 2-way state means the router has received a Hello with its own RID in it.
- If both routers reach the 2-way state, it means that all of the conditions have been met for them to become OSPF neighbors. They are now ready to share LSAs to build a common LSDB.
- In some network types, a DR (Designated Routers) and BDR (Backup Designated Router) will be elected at this point.
(I will talk about OSPF network types and DR/BDR election in Day 28)

**Exstart State**
- The two routers will now prepare to exchange information about their LSDB.
- Before that, they have to choose which one will start the exchange.
- They do this in the Exstart state.
- The router with higher RID will become the Master and initiate the exchange. The router with the lower RID will become the Slave.
- To decide the Master and Slave, they exchange DBD (Database Description) packets.

![[Pasted image 20260310001659.png]]

**Exchange State** 
- In the Exchange state, the routers exchange DBDs which contain a list of the LSAa in their LSDB.
- These DBDs do not include detailed information about the LSAs, just basic information.
- They routers compare the information in the DBD they received to the information in their own LSDB to determine which LSAs they must receive from their neighbor.

![[Pasted image 20260310002008.png]]

**Loading State** 
- In the Loading state, routers send Link State Request (LSR) messages to request that their neighbors send them any LSAs they don't have.
- LSAs are sent in Link State Update (LSU) messages.
- The routers send LSAck messages to acknowledge that they received the LSAs.

![[Pasted image 20260310002336.png]]

**Full State** 
- In the Full state, the routers have a full OSPF  adjacency and identical LSDBs.
- They continue to send and listen for Hello packets (every 10 seconds by default) to maintain the neighbor adjacency.
- Every time a Hello packet is received, the 'Dead' timer (40 seconds by default) is reset.
- If the Dead timer counts down to 0 and no Hello message is received, the neighbor is removed.
- The routers will continue to share LSAs as the network changes to make sure each router has a complete and accurate map of the network (LSDB).

![[Pasted image 20260310002931.png]]


## OSPF Neighbors 

- In OSPF, there are three main steps in the process of sharing LSAs and determining the best route to each destination in the network.
1. **Become neighbors** with other routers connected to the same segment.
2. **Exchange LSAs** with neighbor routers.
3. **Calculate the best routes** to each destination, and insert them into the routing table.

![[Pasted image 20260310003404.png]]

![[Pasted image 20260310003426.png]]

## OSPF Configuration

![[Pasted image 20260310003524.png]]

- You can activate OSPF directly on an interface with this command:
 `R1(config-if)#ip ospf <process-id> area <area>`

![[Pasted image 20260310003648.png]]

![[Pasted image 20260310003856.png]]

- Configure ALL interface as OSPF passive interfaces: 
`R1(config-router)#passive-interface default`

- Then configure specific interfaces as active:
`R1(config-router)#no passive-interface <int-id>`

![[Pasted image 20260310004133.png]]

- Activate OSPF directly on an interface:
`R1(config-if)#ip ospf <process-id> area <area-id>`
- Configure all interfaces as passive interfaces by default:
`R1(config-if)#passive-interface default`


## Quiz

1.
![[Pasted image 20260310004615.png]]

>Reference bandwidth /interface bandwidth = cost (values less than 1 are converted to 1) Default reference bandwidth = 100 mbps


2. ![[Pasted image 20260310004944.png]]

3. ![[Pasted image 20260310005004.png]]
4. ![[Pasted image 20260310005024.png]]
































