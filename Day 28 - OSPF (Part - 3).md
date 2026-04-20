
## Loopback interfaces

- A loopback interface is virtual interface in the router.
- It is always up/up (unless you manually shut it down) 
- It is not dependent on a physical interface.
- So, it provides a consistent IP address that can be used to reach/identify the router.

![[Pasted image 20260310005549.png]]


## OSPF Networks Types 

- The OSPF 'network type' refers to the type of connection between OSPF neighbors (Ethernet, etch)
- There are three main OSPF network types:
	- Broadcast
		- enabled by default on Ethernet and FDDI (Fiber Distributed Data Interfaces) interfaces
	- Point-to-point
		- enabled by default on Frame Relay and X.25 interfaces


### OSPF Broadcast Network Type 

![[Pasted image 20260310010008.png]]

- Enabled on Ethernet and FDDI interfaces by default.
- Routers dynamically discover neighbors by sending/listening for OSPF Hello messages using multicast address 224.0.0.5
- A DR (designated router) and BDR (backup designated router) must be elected on each subnet (only DR if there are no OSPF neighbors, ie R1's G1/0 interface)
- Routers which aren't the DR or BDR become a DROther.

- The DR/BDR election order of priority:
	1. Highest OSPF interface priority 
	2. Highest OSPF Router ID

- 'First place' become the DR for the subnet, 'second place' becomes the BDR
- The default OSFP interface priority is 1 on all interfaces

![[Pasted image 20260310011114.png]]

![[Pasted image 20260310011330.png]]

![[Pasted image 20260310011403.png]]

> If you set the OSFP interface priority to 0, the router CANNOT be the DR/BDR for the subnet.

![[Pasted image 20260310011904.png]]

- The DR/BDR election is 'non-preemptive'. Once the DR/BDR are selected they will they will keep their role until OSPF is reset, the interface fails/is shut down, etc.

![[Pasted image 20260310013629.png]]

- R4 become the DR, not R2 became the BDR.
	- When the DR goes down, the BDR becomes the new DR. Then an election is held for the next BDR.
- R3 is a DROther, and is stable in the 2-way state.
	- DROthers (R3 and R5 in this subnet) will only move to the FULL state with the DR and BDR. The neighbor state with other DROther will be 2-way.

- In the broadcast network type, routers will only form a full OSPF adjacency with the DR and BDR of the segment.
- Therefore, routers only exchange LSAs with the DR and BDR. DROthers will not exchange LSAs with each other.
- All routers will still have the same LSDB, but this reduces the amount of LSAs flooding the network.

> Messages to the DR/BDR are multicast using address 224.0.0.6

![[Pasted image 20260310014654.png]]

![[Pasted image 20260310014814.png]]

![[Pasted image 20260310014844.png]]

## OSPF Point-to-point Network Type

![[Pasted image 20260310015040.png]]

- Enabled on serial interfaces using the PPP or HDLC encapsulations by default.
- Routers dynamically discover neighbors by sending/listening for OSPF Hello messages using multicast address 224.0.0.5.
- A DR and BDR are not elected.
- These encapsulations are used for ‘point-to-point’ connections.
- Therefore there is no point in electing a DR and BDR.
- The two routers will form a Full adjacency with each other.

## Serial Interfaces

![[Pasted image 20260310015328.png]]

- One side of a serial connection function as DCE (Data Communication Equipments)
- The other side functions as DTE (Data Terminal Equipment)
- The DCE side needs to specify the clock rate (speed) of the connection.

> Ethernet interfaces use the speed command to configure the interface's operating speed. Serial interfaces use the clock rate command.


![[Pasted image 20260310020051.png]]

- The default encapsulation on a serial interface is HDLC. actually cHDLC (Cisco HDLC)

- If you change the encapsulation, it must match on both ends or the interface will go down.

![[Pasted image 20260310020323.png]]

![[Pasted image 20260310020354.png]]

![[Pasted image 20260310021628.png]]

## Configure the OSPF Network Type 

![[Pasted image 20260310021732.png]]

- You can configure the OSPF network type on an interface with `ip ospf network <type>`.
- For example, if two routers are directly connected with an Ethernet link, there is no need for a DR/BDR. You can configure the point-to-point network type in this case.
- NOTE: Not all network types work on all link types (for example, a serial link cannot use the broadcast network type).

![[Pasted image 20260310021912.png]]

> Non-broadcast network type default timers = Hello 30, Dead 120

## OSPF Neighbor Requirements

1. Area number must match
![[Pasted image 20260310022137.png]]

2. Interfaces must be in the same subnet
![[Pasted image 20260310022242.png]]

3. OSPF process Requirements 
![[Pasted image 20260310022447.png]]

4. OSPF process must not be shutdown
![[Pasted image 20260310022655.png]]

5. Hello and Dead timers must match
![[Pasted image 20260310022751.png]]

6. Authentication settings must match
![[Pasted image 20260310023122.png]]

7. ![[Pasted image 20260310023232.png]]

![[Pasted image 20260310023305.png]]

![[Pasted image 20260310023418.png]]

8. OSPF Network Type must match

![[Pasted image 20260310023626.png]]

![[Pasted image 20260310023715.png]]

## OSPF LSA Types 

![[Pasted image 20260310023810.png]]

- The OSPF LSDB is made up of LSAs.
- There are 11 types of LSA, but there are only 3 you should be aware of for the CCNA:
	Type 1 (Router LSA)
	Type 2 (Network LSA)
	Type 5 (AS External LSA)


## OSPF LSA Types 

- Type 1 (Router LSA)
	- Every OSPF router generates this type of LSA.
	- It identifies the router using its router ID.
	- It also lists network attached to the router's OSPF-activated interfaces.
- Type 2 (Network LSA)
	- Generated by the DR of each 'multi-access' networks (ie. the broadcast network type).
	- Lists the routers which are attached to the multi-access network. 
- Type 5 (AS-External LSA)
	 - Generated by ASBRs to describe routes to destination outside of the AS (OSPF domain).

![[Pasted image 20260310024903.png]]


## Quiz

![[Pasted image 20260310024945.png]]

![[Pasted image 20260310024959.png]]

![[Pasted image 20260310025014.png]]

![[Pasted image 20260310025028.png]]

![[Pasted image 20260310025100.png]]

![[Pasted image 20260310025120.png]]