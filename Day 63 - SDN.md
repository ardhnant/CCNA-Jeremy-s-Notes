## SDN Review 

- Software-Defined Networking (SDN) is an approach to networking that centralizes the control plane into an application called a _controller_.
- Traditional control planes use a distributed architecture.
- An SDN controller centralizes control plane functions like calculating routes.
- The controller can interact programmatically with the network devices using APIs.
- The SBI is used for communications between the controller and the network devices it controls.
- The NBI is what allows us to interact with the controller with our scripts and applications.

![[Pasted image 20260408024618.png]]

## SD-Access

- Cisco SD-Access is Cisco's SDN solution for automating campus LANs.
	- ACI(Application Centric Infrastructure) is their SDN solution for automating data center networks.
	- SD-WAN is their SDN solution for automating WANs.
- Cisco DNA(Digital Network Architecture) Center to the controller at the center of SD-Access.

![[Pasted image 20260408025000.png]]

- The underlay is the underlying physical network of devices and connections (including wired and wireless) which provide IP connectivity (ie. using IS-IS).  
    - Multilayer switches and their connections.
- The overlay is the virtual network built on top of the physical underlay network.  
    - SD-Access uses VXLAN (Virtual Extensible LAN) to build tunnels.
- The fabric is the combination of the overlay and underlay; the physical and virtual network as a whole.

![[Pasted image 20260408025157.png]]

![[Pasted image 20260408025215.png]]


## SD-Access Underlay

- The underlay’s purpose is to support the VXLAN tunnels of the overlay.
- There are three different roles for switches in SD-Access:  
    - Edge nodes: Connect to end hosts  
    - Border nodes: Connect to devices outside of the SD-Access domain, ie. WAN routers.  
    - Control nodes: Use LISP (Locator ID Separation Protocol) to perform various control plane functions.
- You can add SD-Access on top of an existing network (brownfield deployment) if your network hardware and software supports it.  
    - Google ‘Cisco SD-Access compatibility matrix’ if you’re curious.  
    - In this case DNA Center won’t configure the underlay.
- A new deployment (greenfield deployment) will be configured by DNA Center to use the optimal SD-Access underlay:  
    - All switches are Layer 3 and use IS-IS as their routing protocol.  
    - All links between switches are routed ports. This means STP is not needed.  
    - Edge nodes (access switches) act as the default gateway of end hosts (routed access layer).

![[Pasted image 20260408025445.png]]

![[Pasted image 20260408025518.png]]


## SD-Access Overlay

Here it is, neatly repeated:

- LISP provides the control plane of SD-Access.  
    - A list of mappings of EIDs (endpoint identifiers) to RLOCs (routing locators) is kept.  
    - EIDs identify end hosts connected to edge switches, and RLOCs identify the edge switch which can be used to reach the end host.  
    - There is a LOT more detail to cover about LISP, but I think you can see how it differs from the traditional control plane.
- Cisco TrustSec (CTS) provides policy control (QoS, security policy, etc).
- VXLAN provides the data plane of SD-Access.

![[Pasted image 20260408025701.png]]

## Cisco DNA Center

- Cisco DNA Center has two main roles:  
    - The SDN controller in SD-Access  
    - A network manager in a traditional network (non-SD-Access)
- DNA Center is an application installed on Cisco UCS server hardware.
- It has a REST API which can be used to interact with DNA center.
- The SBI supports protocols such as NETCONF and RESTCONF (as well as traditional protocols like Telnet, SSH, SNMP).
- DNA Center enables Intent-Based Networking (IBN).  
    - More buzzwords! Yay!  
    - The goal is to allow the engineer to communicate their intent for network behavior to DNA Center, and then DNA Center will take care of the details of the actual configurations and policies on devices.
- Traditional security policies using ACLs can become VERY cumbersome.  
    - ACLs can have thousands of entries.  
    - The intent of entries is forgotten with time and as engineers leave and new engineers take over.  
    - Configuring and applying the ACLs correctly across a network is cumbersome and leaves room for error.


- DNA Center allows the engineer to specify the intent of the policy (this group of users can’t communicate with this group, this group can access this server but not that server, etc.), and DNA Center will take care of the exact details of implementing the policy.


![[Pasted image 20260408025924.png]]


# Quiz 

![[Pasted image 20260408030055.png]]

![[Pasted image 20260408030120.png]]

![[Pasted image 20260408030151.png]]

![[Pasted image 20260408030206.png]]

![[Pasted image 20260408030217.png]]

![[Pasted image 20260408030232.png]]