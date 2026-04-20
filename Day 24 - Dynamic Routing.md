## Overview

![[Pasted image 20260307114226.png]]

- Router advertise their metric and destination to their neighboring router.
- Best path is determined by the metric (cost) used to get to that path and then are added to the routing table.
 > Only one path to the destination is added to the routing table at a time.

- Router can use dynamic routing protocols to advertise information about the routes they know to other routers.
- They form 'adjacencies' / 'neighbor relationship' / 'neighborships' with adjacent routers to exchange this information.
- If multiple routes to a destination are learned the router determines which route is superior and adds it to routing table. It uses the 'metric' of the router to decide which is superior (lower metric = superior).

## Types of Dynamic Routing Protocols

- Dynamic routing protocols can be divided into two main categories:
	1. IGP (Interior Gateway Protocol)
	2. EGP (Exterior Gateway Protocol)

- IGP is used to share routes within a single autonomous system (AS), which is a single organization (ie. a company).
- EGP is used to share routes between different autonomous systems.

- **IGP**
	1. Distance Vector
		- Routing Information Protocol (RIP)
		- Enhanced Interior Gateway Routing Protocol (EIGRP)
	2. Link State
		- Open Shortest Path First (OSPF).
		- Intermediate System to Intermediate system (IS-IS)

- **EGP**
	1. Path Vector 
		- Border Gateway Protocol (BGP)

> Distance Vector, Link State and Path Vector are algorithm type.


### Distance Vector Routing Protocols

- Distance vector protocols were invented before link state protocols.
- Early examples are RIPv1 and Cisco's proprietary protocol IGRP (which was updated to EIGRP).
- Distance vector protocols operate by sending the following to their directly connected neighbors:
	- their known destination networks
	- their metric to reach their known destination networks
- This method of sharing route information is often called 'routing by rumor'
- This is because the router doesn't know about the network beyond its neighbors. It only knows the information that its neighbors tell it.
- Called 'distance vector' because the routers only learn the 'distance (metric)' and 'vector' (direction, the next-hop router) of each route.

### Link state routing protocols

- When using a **link state** routing protocol, every router creates a ‘connectivity map’ of the network.

- To allow this, each router advertises information about its interfaces (connected networks) to its neighbors. These advertisements are passed along to other routers, until all routers in the network develop the same map of the network.

- Each router independently uses this map to calculate the best routes to each destination.

- Link state protocols use more resources (CPU) on the router, because more information is shared.

- However, link state protocols tend to be faster in reacting to changes in the network than distance vector protocols.


### Dynamic Routing Protocol Metrics

- A router’s route table contains the best route to each destination network it knows about.

- If a router using a dynamic routing protocol learns two different routes to the same destination, how does it determine which is ‘best’?

- It uses the **metric** value of the routes to determine which is best. A lower metric = better.

- Each routing protocol uses a different metric to determine which route is the best.


![[Pasted image 20260307120641.png]]


What if this was also a gigabit Ethernet connection?  
Both routes would have the same metric, so which route would be added to the route table?

> If a router learns two (or more) routes via the **same routing protocol** to the **same destination** (same network address, same subnet mask) with the **same metric**, both will be added to the routing table. Traffic will be load-balanced over both routes.

> ECMP (Equal Cost Multi-Path)

![[Pasted image 20260307120941.png]]

![[Pasted image 20260307121039.png]]

- In this question if we are using RIP R1 will use both routes (via R2 and R3) to go R4.
- If we use OSPF bandwidth will determine the path, in this case gigabit ethernet will less compared to fast ethernet hence R2 will be added to the routing table.

## Administrative Distance

 - In most cases a company will only use a single IGP – usually OSPF or EIGRP.

- However, in some rare cases they might use two. For example, if two companies connect their networks to share information, two different routing protocols might be in use.

- Metric is used to compare routes learned via the same routing protocol.

- Different routing protocols use totally different metrics, so they cannot be compared.

- For example, an OSPF route to 192.168.4.0/24 might have a metric of 30, while an EIGRP route to the same destination might have a metric of 33280. Which route is better? Which route should the router put in the route table?

- The administrative distance (AD) is used to determine which routing protocol is preferred.

- A lower AD is preferred, and indicates that the routing protocol is considered more ‘trustworthy’ (more likely to select good routes).


![[Pasted image 20260307121600.png]]

![[Pasted image 20260307121621.png]]

> If the administrative distance is 255, the router does not believe the source of that route and odes not install the route in the routing table.



- The following routes to the destination network 10.1.1.0/24 are learned:  
    → next hop 192.168.1.1, learned via RIP, metric 5  
    → next hop 192.168.2.1, learned via RIP, metric 3  
    → next hop 192.168.3.1, learned via OSPF, metric 10

Which route to 10.1.1.0/24 will be added to the route table?

- Metric is used to compare routes learned from the same routing protocol.
- However, before comparing metrics, AD is used to select the best route.
- The OSPF route will always take precedence over the RIP routes, because it has a lower AD.

> You change the AD of routing protocol.

> You can also change the AD of a static route

![[Pasted image 20260307122014.png]]

![[Pasted image 20260307122031.png]]


## Floating Static Routes 

- By changing the AD of a static route, you can make it less preferred than routes learned by a dynamic routing protocol to the same destination (make sure the AD is higher than the routing protocol’s AD!).
- This is called a ‘floating static route’.
- The route will be inactive (not in the routing table) unless the route learned by the dynamic routing protocol is removed (for example, the remote router stops advertising it for some reason, or an interface failure causes an adjacency with a neighbor to be lost).


## Quiz

![[Pasted image 20260307122217.png]]

![[Pasted image 20260307122230.png]]

![[Pasted image 20260307122245.png]]


![[Pasted image 20260307122309.png]]

