## LAN Architectures

- You have studied various network technologies: routing, switching, STP, EtherChannel, OSPF, FHRPs, switch security features, etc.
	- Now let’s look at some basic network design/architecture
	
• There are standard ‘best practices’ for network design.
	- However there are few universal ‘correct answers’.
	- The answer to most general questions about network design is ‘it depends’.

- In the early stages of your networking career, you probably won’t be asked to design networks yourself.
- However, to understand the networks you will be configuring and troubleshooting it’s important to know some basics of network design.

## Common Terminologies

**Star:** When several devices all connect to one central device we can draw them in a 'star' shape like below, so this is often called a 'star topology'.

![[Pasted image 20260331181850.png]]

**Full Mesh:** When each device is connected to each other device.

![[Pasted image 20260331182008.png]]

**Partial Mesh:** When some devices are connected to each other, but not all.

![[Pasted image 20260331182105.png]]

## Two-Tier Campus LAN Design 

- The two-tier LAN design consists of two hierarchical layers:
	- Access Layer
	- Distribution Layer

- Also called a ‘Collapsed Core’ design because it omits a layer that is found in the Three Tier design: the Core Layer

- Access Layer:
	- the layer that end hosts connect to (PCs, printers, cameras, etc.)
	- typically Access Layer Switches have lots of ports for end hosts to connect to
	- QoS marking is typically done here
	- Security services like port security, DAI, etc are typically performed here
	- switchports might be PoE-enabled for wireless APs, IP phones, etc.
- Distribution Layer:
	- aggregates connections from the Access Layer Switches
	- typically is the border between Layer 2 and Layer 3
	- connects to services such as Internet, WAN, etc.

In a collapsed core desing, the Distribution Layer is sometimes called the Core-Distribution Layer.

![[Pasted image 20260331182427.png]]

>Connection between Distribution Switches are Layer 3. Routing information can be shared via OSPF, for example.

![[Pasted image 20260331182549.png]]

![[Pasted image 20260331182605.png]]

![[Pasted image 20260331182621.png]]

In large LAN networks with many Distribution Layer switches (for example in a separate buildings), the umber connection required between Distribution Layer switches grows rapidly.

![[Pasted image 20260331182818.png]]

To help scale large LAN networks, you can add a Core Layer. Cisco recommends adding a Core Layer if there are more than three Distribution Layers in a single location.

![[Pasted image 20260331183218.png]]

## Three-Tier Campus LAN Design 

- The three-tier LAN design consists of three hierarchical layers:
    - Access Layer
    - Distribution Layer
    - Core Layer

- Core Layer:
    - Connects Distribution Layers together in large LAN networks
    - The focus is speed (‘fast transport’)
    - CPU-intensive operations such as security, QoS marking/classification, etc. should be avoided at this Layer
    - Connections are all Layer 3. No spanning-tree!
    - Should maintain connectivity throughout the LAN even if devices fail

![[Pasted image 20260331183433.png]]


## Spine-Leaf Architecture

- Data centers are dedicated spaces/buildings used to store computer systems such as servers and network devices.
- Traditional data center designs used a three-tier architecture (Access-Distribution-Core) like we just covered.
- This worked well when most traffic in the data center was North-South.

![[Pasted image 20260331183605.png]]

- With the precedence of virtual servers, applications are often deployed in a distributed manner (across multiple physical servers), which increases the amount of East-West traffic in the data center.
- The traditional three-tier architecture led to bottlenecks in bandwidth as well as variability in the server-to-server latency depending on the path the traffic takes.
- To solve this, Spine-Leaf architecture (also called Clos architecture) has become prominent in data centers.

- There are some rules about Spine-Leaf architecture:
    - Every Leaf switch is connected to every Spine switch.
    - Every Spine switch is connected to every Leaf switch.
    - Leaf switches do not connect to other Leaf switches.
    - Spine switches do not connect to other Spine switches.
    - End hosts (servers etc.) only connect to Leaf switches.

- The path taken by traffic is randomly chosen to balance the traffic load among the Spine switches.
- Each server is separated by the same number of ‘hops’ (except those connected to the same Leaf), providing consistent latency for East-West traffic.

![[Pasted image 20260331183751.png]]

## SOHO Networks

- Small Office/Home Office (SOHO) refers to the office of a small company, or a small home office with few devices.
    - Doesn’t have to be an actual home ‘office’, if your home has a network connected to the Internet it is considered a SOHO network.

- SOHO networks don’t have complex needs, so all networking functions are typically provided by a single device, often called a ‘home router’ or ‘wireless router’.

- This one device can serve as a:
    - Router
    - Switch
    - Firewall
    - Wireless Access Point
    - Modem


![[Pasted image 20260331183926.png]]

![[Pasted image 20260331184714.png]]

# **Quiz**

![[Pasted image 20260331184751.png]]

![[Pasted image 20260331184810.png]]

![[Pasted image 20260331184825.png]]

![[Pasted image 20260331184843.png]]

![[Pasted image 20260331184902.png]]

![[Pasted image 20260331184930.png]]