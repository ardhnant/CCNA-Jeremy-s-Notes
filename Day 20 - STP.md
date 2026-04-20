## **Why Redundancy Exists (And Why It’s Dangerous Without Control)**

## **The Goal of Redundancy**

Modern networks must be:

- Always available (24/7/365)
    
- Resilient to hardware failure
    
- Able to reroute traffic automatically
    

Redundancy means:

- Multiple physical paths
    
- Backup devices
    
- Alternate routes
    

If one link or switch fails → traffic should still flow.

---

## **The Hidden Problem: Layer 2 Loops**

When you add redundant Layer 2 links between switches:

- Switches flood **broadcast frames**
    
- Switches flood **unknown unicast frames**
    

Without a loop-prevention mechanism:

- Frames circulate forever
    
- There is no TTL field in Ethernet
    
- Broadcast traffic multiplies infinitely

> Most PCs only have a single network interface card (NIC), so they can only be plugged into a single switch. However, important servers typically have multiple NICs, so they can be plugged into multiple switches for redundancy.

---

## **Broadcast Storm**

A broadcast storm happens when:

- Broadcast frames loop endlessly
    
- Network bandwidth is consumed
    
- Legitimate traffic cannot pass
    

Result:

- Network congestion
    
- Complete LAN failure
- This is known as **broadcast storm**.

![[Pasted image 20260228150700.png]]

> Ethernet header doesn't have a TTL field.

Network congestion isn't the only problem. Each time a frame arrives on a switchport, the switch uses the source MAC address table. When frames with the same source MAC address repeatedly arrive on the different interfaces, the switch is continuously updating the interface in its MAC address table. This is known as **MAC Address Flapping**.

---

### **Spanning Tree Protocol (STP)**

- 'Classic Spanning Tree Protocol' is IEEE 802.1D
- Switches from ALL vendors run STP by default.
- STP prevents Layer 2 loops by placing redundant ports in a blocking state, essentially disabling the interface.
- These interfaces ast as backups that can enter a forwarding state if an active (=currently forwarding) interface fails.
- Interfaces in a forwarding state behave normally. They send and receive all normal traffic.
- Interfaces in a blocking state only send or receive STP messages (called BPDUs = Bridge Protocol Units).


#### **Bridges**

Bridge are old devices which used to act as a middle man between Hub and Switches which are not used anymore. Though STP still uses the term 'bridge'. However when we use the term bridge we really mean 'switch'.

![[Pasted image 20260228153139.png]]

- In this topology SW3 is in a blocking stage, it won't forward any traffic to SW2. Hence the loop won't occur.

![[Pasted image 20260228152941.png]]

- If at some point interface of SW2 connecting with SW1 fails then the blocked node will start forwarding the traffic. Switches will handle this automatically.

![[Pasted image 20260228153439.png]]

![[Pasted image 20260228153554.png]]

- By selecting which ports are forwarding and which ports are blocking, STP create a single path to/from each point in the network. This prevents Layer 2 loops.
- There is a set process that STP uses to determine which ports should be forwarding and which should be blocking.
- STP-enabled switches send/receive Hello BPDUs out of all interfaces, the default timer is 2 seconds (the switch will send a Hello BPDU out of every interface, once every 2 seconds).
- If a switch receives a Hello BPDU on an interace, it knows that interface is connected to another switch (router, PCs, etc. do not use STP, so they do not send Hello BPDUs).

### **What are BPDUs used for ?**

- Switches use one field in the STP BPDU, the Bridge ID field, to elect a root bridge for the network.
- The switch with the lowest Bridge ID becomes the **root bridge**.
- ALL ports on the root bridge are put in a forwarding state, and other switches in the topology must have a path to reach the root bridge.

> The default bridge priority is 32768 on all switches, so by default the MAC address is used as the tie-breaker (lowest MAC address becomes the root bridge).

>*** The bridge priority is compared first. If they tie, the MAC address is then compared ***


### **Anatomy of BPDU**


![[Pasted image 20260228161202.png]]

> Cisco switches use a version of STP called PVST (Per-VLAN Spanning Tree). PVST runs a separate STP 'instance' in each VLAN, so in each VLAN different interfaces can be forwarding/blocking.

---

### **Bridge ID Structure (Modern Format)**

Bridge ID =

| Component                    | Size    |
| ---------------------------- | ------- |
| Bridge Priority              | 4 bits  |
| Extended System ID (VLAN ID) | 12 bits |
| MAC Address                  | 48 bits |

Lowest Bridge ID wins.

> In the VLAN of 1, the default bridge priority is actually 32769 (32768 + 1).


---

### **Default Values**

- Default bridge priority = 32768
    
- Default VLAN = 1
    
- Actual default bridge ID priority = 32768 + 1 = **32769**
    

If priorities tie → lowest MAC address wins.

---

### **Important Rule**

Lowest Bridge ID = Root Bridge

All ports on root bridge:

- Are **Designated Ports**
    
- Are in **Forwarding State**
    

---

### **Bridge Priority Rules**

Priority can only change in increments of:

**4096**

Valid configurable priorities:

0, 4096, 8192, 12288, 16384, 20480, 24576, 28672, 32768, etc.

Total Bridge Priority = Configured Priority + VLAN ID


- When a switch is powered on, it assumes it is the root bridge.
- It will only give up its position if it receives a 'superior' BPDU (lower bridge ID).
- Once the topology has converged and all switches agree on the root bridge, only the root bridge sends BPDUs.
- Other switches in the network will forward these BPDUs, but will not generate their own original BPDUs.

![[Pasted image 20260228164322.png]]

![[Pasted image 20260228164344.png]]


---
### **STP Port Roles**

1. The switch with the lowest bridge ID is elected as the root bridge. All ports on the root bridge are designated ports (forwarding state).
2. Each remaining switch will select ONE of its interfaces to be its root port. The interface with the lowest root cost will be the root port. Root ports are also in a forwarding state.

| **Speed** | **STP Cost** |
| --------- | ------------ |
| 10 Mbps   | 100          |
| 100 Mbps  | 19           |
| 1 Gbps    | 4            |
| 10 Gbps   | 2            |

> Root cost is the total cost of the outgoing interfaces along to the path to the root bridge.

![[Pasted image 20260228191454.png]]

#### SW2's logic:
- It's gigabit ethernet so the cost will be 4, if SW2 wanna go to SW1 - he can go with it's own g0/1 interface which will cost him 4 root cost while if he goes through SW3 then the total cost will be `SW2 to SW3 (4) + SW3 to SW1 (4) = 8` 

#### SW3's Logic
- It's gigabit ethernet so the cost will be 4, if SW3 wanna go to SW1 - he can go with it's own g0/0 interface which will cost him 4 root cost while if he goes through SW2 then the total cost will be `SW3 to SW2 (4) + SW2 to SW1 (4) = 8` 

![[Pasted image 20260228192214.png]]

> The ports connected to another switch's root port MUST be designated. Because the root port is the switch's path to the root bridge, another switch must not block it.

> The Root switch does not has any Root port

---

![[Pasted image 20260228192344.png]]

![[Pasted image 20260228192420.png]]

---

#### Priority Number

![[Pasted image 20260228193000.png]]

- Priority number is number which is given to every interface which shows it's priority, `lower priority interfaces are preferred`.

In the 4th question there are two interfaces connected with the same switch, in this case lower priority interfaces will be chosen.

> The NEIGHBOR switch's port ID is used to break the tie, not the local switch's port ID. In the 4th question g0/1 of SW1 is the tie breaker and will act as a designated port while g0/2 of SW3 will be the root port.

#### Collision Domain

- The connection between two switch's interface can be called a *collision domain*. 

> Every collision domain has a single STP designated port, other must be a non-designated or a root port.

- The switch with the lowest root cost will make its post designated.
- If the root cost is same, the switch with the lowest bridge ID will make its port designated.
- The other switch will make its port non-designated.


---

## **Summary**

- One switch is elected as the root bridge. All ports on the root bridge are designated ports (forwarding state). Root bridge selection:
	1. Lowest bridge ID

- Each remaining switch will select ONE of its interfaces to be its root port (forwarding state). Ports across from the root port are always designated ports.
	Root port selection:
	1. Lowest root cost
	2. Lowest neighbor bridge ID
	3. Lowest neighbor port ID

- Each remaining collision domain will select ONE interface to be a designated port (forwarding state). The other port in the collision domain will be non-designated (blocking)
	Designated port selection:
	1. Interface on switch with lowest root cost
	2. Interface on switch with lowest bridge ID

---

![[Pasted image 20260228194045.png]]

### Commands

#### `show spanning-tree`

![[Pasted image 20260303190007.png]]

#### `show spanning-tree vlan <vlan nubmer>`

![[Pasted image 20260303190203.png]]

#### `show spanning-tree detail`

![[Pasted image 20260303190441.png]]


#### `show spanning-tree summary`

![[Pasted image 20260303190639.png]]












