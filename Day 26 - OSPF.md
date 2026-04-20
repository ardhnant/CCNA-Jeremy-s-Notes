## Link State Routing Protocols

- When using a link state routing protocol, every router creates a 'connectivity map' of the network.
- To allow this, each router advertises information about its interfaces (connected networks) to its neighbors. These advertisements are passed along to other routers, until all routers in the network develop the same map of the network.
- Each router independently uses this map to calculate the best routes to each destination.
- Link state protocols use more resources (CPU) on the router, because more information is shared.
- However, link state protocols tend to be faster in reacting to changes in the network than distance vector protocols.

## OSPF 

- Stands for Open Shortest Path First 
- Uses the Shortest Path First (SPF) algorithm of Dutch computer scientist Edsger Dijkstra.
- Three versions:
	- OSPFv1 (1989): OLD, not in use anymore
	- OSPFv2 (1998): Used for IPv4 
	- OSPFv3 (2008): Used for IPv6 (can also be used for IPv4, but usually v2 is used)
- Routers store information about the network in LSAs (link state advertisement), which are organised in a structure called the LSDB (link state database).
- Routers will flood LSAs until all routers in the OSPF area develop the same map of the network (LSDB).

## LSA Flooding

![[Pasted image 20260308222525.png]]

- OSPF is enabled on R4's G1/0 interface.
- R4 creates an LSA to tell its neighbors about the network on G1/0.
- The LSA is flooded throughout the network until all routers have received it.
- This results in all routers sharing the same LSDB.
- Each router then uses the SPF(shortest path first) algorithm to calculate its best route to 192.168.4.0/24

![[Pasted image 20260308222839.png]]

> Each LSA has an aging timer (30 min by default). The LSA will be flooded again after the timer exires.


## OSPF 

- In OSPF, there are three main steps in the process of sharing LSAs and determining the best route to each destination in the network.
- Become neighbors with other routers connected to the same segment.
- Exchange LSAs with neighbor routers.
- Calculate the best routes to each destination, and insert them into the routing table.


## OSPF Areas

- OSPF uses areas to divide up the network.
- Small network can be single-area without any negative effects on performance.

![[Pasted image 20260308223429.png]]


- In larger networks can be single-area design can have negative effects:
	- the SPF algorithm takes more time to calculate routes
	- the SPF algorithm requires exponentially more processing power on the routers 
	- the larger LSDB takes up more memory on the routers 
	- any small change in the network causes every router to flood LSAs and run the SPF algorithm again.

- By dividing a large OSPF network into several smaller areas, you can avoid the above negative effects.

SPF = Shortest Path Forward

## OSPF Areas

- An area is a set of routers and links that share the same LSDB.

![[Pasted image 20260308224212.png]]

- The backbone area (area 0) is an area that all other areas must connect to and the router connected to this area are called backbone router.

![[Pasted image 20260308224343.png]]
In this case area 1 is not allowed in OSPF. wrong pic

- Routers with all interfaces in the same area are called internal routers.
![[Pasted image 20260308224513.png]]

- Routers with interfaces in multiple areas are called area border routers (ABRs).

![[Pasted image 20260308224627.png]]


> ABRs maintain a separate LSDB for each area they are connected to. It is recommend that you connect an ABR to a maximum of 2 areas. Connecting an ABR to 3+ areas can overburden the router.

- Routers connected to the backbone area (area 0) are called backbone routers.

![[Pasted image 20260308225059.png]]

- An intra-area route is a route to a destination inside the same OSPF area.
- An inter area router is a route to destination in a different OSPF area. 

- OSPF areas should be contiguous.

![[Pasted image 20260308225421.png]]

this is bad coz area 1 is scattered and all the routers of area 1 are not connected.. 

- All OSPF areas must have at least one ABR connected to the backbone area.
- The 'wrong pic' is not having any interface connected with Area 0 so technically area 1 is part of area 2.

- OSPF interfaces in the same subnet must be in the same area.

![[Pasted image 20260308230412.png]]

so every interfaces which is connected to a particular area.. 

## Basic OSPF Configuration 

![[Pasted image 20260308230808.png]]


- The OSPF process ID is locally significant. Routers with different process IDs can become OSPF neighbors.
- The OSPF network command requires you to specify the area.
- For the CCNA, you only need to configure single-area OSPF (area 0)

![[Pasted image 20260308231043.png]]

> The network command tells OSPF to look for any interfaces with an IP address contained in the range specified in the network command. Activate OSPF on the interface in the specified area. The router will then try to become OSPF neighbors with other OSPF-activated neighbor routers.


## The `passive-interface` command

`passive-interface g2/0`

- You already know this command from RIP and EIGRP.
- The passive-interface command tells the router to stop sending OSPF 'hello' messages out of the interface.
- However, the router will continue to send LSAs informing it's neighbors about the subnet configured on the interface.
- You should always use this command on interfaces which don't have any OSPF neighbors.


## Advertise a default 

![[Pasted image 20260308231629.png]]

`ip route 0.0.0.0 0.0.0.0 203.0.113.2`

- You can advertise this route with the command `default-information originate` to other connected routers.

![[Pasted image 20260308231824.png]]

## `show ip protocols`

- Router ID order of priority:
	- Manual configuration 
	- Highest IP address on a loopback interface
	- Highest IP address on a physical interface

![[Pasted image 20260308232024.png]]

to set router id `router id <ip addr you wanna put>`
- if no ID is configured, loopback IP will become router ID.

to clear OSPF process `clear ip ospf process`

![[Pasted image 20260308232247.png]]

- An autonomous system boundary router (ASBR) is an OSPF router that connects the OSPF network to an external network.
- R1 is connected to the internet. By using the default-information originate command, R1 becomes an ASBR.

## Unequal-cost Load Balancing

Unequal-cost load balancing is a feature of EIGRP that allows a router to distribute traffic across multiple routes to the same destination even if the routes have different metrics, as long as they satisfy the feasibility condition and fall within the configured variance value.

`variance 2`

#### How It Works

Variance sets a multiplier for the best metric.

Formula:

Allowed metric ≤ (Best metric × Variance)

Any route whose metric falls within this range can be used for load balancing (if it also satisfies the feasibility condition).

---

##### Example

|Path|Metric|
|---|---|
|Best path|1000|
|Second path|2000|

Configuration:

variance 2

Calculation:

1000 × 2 = 2000

Since 2000 ≤ 2000, both paths can be used.

---


## Quiz

1. ![[Pasted image 20260308232632.png]]

2. ![[Pasted image 20260308234401.png]]
https://chatgpt.com/share/69adbc4f-7b9c-800f-bb6b-9959d9642a71

3. ![[Pasted image 20260308235457.png]]

4. ![[Pasted image 20260308235535.png]]

5. 
   ![[Pasted image 20260308235600.png]]




















