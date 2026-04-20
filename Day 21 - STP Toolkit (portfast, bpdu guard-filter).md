# **Toolkit**

- When an end host connects to a switch port, the port becomes up/up but can't send/receive data yet.
	- It is a Designated port but will take 30 seconds before it enters the Forwarding state:
		- 15 seconds in Listening 
		- 15 seconds in Learning

- This leads to a poor user experience.
- This wait is unnecessary, because there is no risk of a Layer 2 loop occurring between a switch/PC.

![[Pasted image 20260303230215.png]]

- When *Portfast* is configured on a port, the port immediately enters the Forwarding state when connected to another device.
- It bypass Listening/Learning and can send/receive data right away.


## Configuring Portfast

- You can configure PortFast in two ways:
	1. Interface config mode:
		`SW1(config-if)#spanning-tree portfast`
	2. Global config mode:
		`SW1(config)#spanning-tree portfast default`
		- this enables PortFast on all access ports.

> Connections between switches are almost always trunk links and connections to end hosts are almost always access links.


### Method 1 (Port based)

![[Pasted image 20260303230925.png]]


#### Command Summary

- `interface g0/1`
- `spanning-tree portfast`
- `show spanning-tree interface g0/1 detail`

### Method 2 (PortFast configuration: default)

![[Pasted image 20260303231303.png]]

![[Pasted image 20260303231500.png]]

- Access ports only (not truck ports).
- To disable PortFast on a specific access port:
	- `SW1(config-if)#spanning-tree portfast disable`

#### g0/1 interface 

![[Pasted image 20260303231654.png]]

- G0/1 is connected to an end host and is an access port which mean that PortFast in enabled.

#### g0/1 interface

![[Pasted image 20260303231832.png]]

- We can't see any mention of Portfast, So we can be sure of that there is no PortFast enabled on this interface.

## PortFast on trunk ports

![[Pasted image 20260303232000.png]]

- The standard PortFast configuration commands only enable PortFast on access ports.
- In some cases, you might want to enable PortFast on a trunk port:
	- Like a port connected to a virtualization server with virtual machines in different VLAN or port connected to router via router-on-a-stick (ROAS)

- This can only be configured per-port in interface config mode

![[Pasted image 20260303232418.png]]

## Portfast edge

- In modern Cisco switches, if you use the commands covered in this lecture, the device will automatically add the edge keyword to the configuration.

![[Pasted image 20260303232609.png]]




---


# **BPDU Guard & BPDU Filter**

## How does PortFast handles BPDUs

![[Pasted image 20260304120604.png]]

- PortFast makes a port start in the Forwarding state when it is connected, but it doesn't disable STP on the port.
	- The port will continue to send BPDUs every 2 seconds.

- Because end hosts don't run STP and send BPDUs, a PortFast-enabled port shouldn't receive BPDUs.
- If a PortFast-enabled port receives an STP BPDU, it will revert to acting like a regular STP port (without PortFast).


## BPDU Guard - the problem

![[Pasted image 20260304120908.png]]

- PortFast should only be enabled on ports connected to non-switch devices (end hosts, routers).
- A PortFast-enabled port still sends BPDUs and will operate like regular STP port if it receives BPDUs from a neighbor.
- If an end user carelessly connects a switch to a port meant for end hosts, it could affect the STP topology.
- *BPDU Guard* acts as a safeguard against this.


- BPDU Guard protects the network from unauthorized switches being connected to ports intended for end hosts.
- It can be configured separately from PortFast, but both features are usually used together.
	- They both enhance STP's functionality on ports intended for end hosts.

- A BPDU Guard-enabled port continues to send BPDUs, but if it receives a BPDU it enters the error-disabled state.
	- In effect, this disables the port.

### BPDU Guard configuration

- Like PortFast, BPDU Guard can be configured in two ways:

#### Per-port: 

![[Pasted image 20260304124058.png]]

#### Default:

![[Pasted image 20260304124128.png]]

- When enabled by default, BPDU Guard is activated on all PortFast-enabled ports.
- Use spanning-tree bpduguard disable in interface config to disable it on specific ports.

### ErrDisable

![[Pasted image 20260304124326.png]]

![[Pasted image 20260304124342.png]]

- *ErrDisable* is a Cisco switch feature that disables a port under certain conditions, such as a BPDU Guard violation.
	- You will learn a few other for the CCNA exam, such as:
		- Power Policing violations
		- Port Security violations
		- DAI (Dynamic ARP Inspection) violations

- To re-enable an err-disable port, first solve the underlying issue.
	- If you re-enable the port without fixing the issue, it will just be err-disabled again.

- You can re-enable an err-disabled port in two ways:
	1. Manual: use `shutdown` and `no shutdown` to reset the disabled port.
	2. Automatic: `ErrDisabled Recovery`

- *ErrDisable Recovery* is a feature that automatically re-enables err-disabled ports after a certain period of time.

![[Pasted image 20260304135836.png]]

- Use `errdisable recovery cause` cause to enable Errdisable Recovery for ports disabled by a particular cause.

> ErrDisable Recovery is disabled by default.

- The default timer is 300 seconds (5 minutes).
	- Err-disabled interfaces will be automatically re-enabled after 5 minutes.

- Use `SW1(config)# errdisable recovery interval` seconds to modify the interval.

![[Pasted image 20260304170827.png]]

## BDPU Filter - the problem

![[Pasted image 20260304171026.png]]

- A switch port connected to an end host continues sending BPDUs every 2 seconds.
	- Regardless of whether PortFast and/or BPDU Guard are enabled

- If the port doesn't connect to a switch, sending BPDUs is unnecessary and undesirable for a couple of reasons:
	1. Sending BPDUs uses some bandwidth and processing power on the switch (although it's minimal).
	2. BPDUs contain information about the LAN's STP topology.
		- If maximum security is a concern, you should avoid sending this info to user devices.

- BPDU Filter solves this by preventing a port from sending BPDUs.

## BDPU Filter - the solution 

- BPDU Filter stops a port from sending BPDUs.
	- Unlike BPDU Guard, it does not disable the port if it receives a BPDU.

- BPDU Filter can be enabled in two ways:
	- Per-port: SW3 (config-if)# `spanning-tree bpdufilter enable`
		- The port will not send BPDUs.
		- The port will ignore any BPDUs it receives.
		- In effect, this disables STP on the port. Use with caution!

- Default: SW3 (config)# `spanning-tree portfast [edge] bpdufilter default`
	- BPDU Filter will be activated on all PortFast-enabled ports.
		- You can use spanning-tree bpdufilter disable to disable it on specific ports.
	- The port will not send BPDUs.
	- If the port receives a BPDU, PortFast and BPDU Filter are disabled, and it operates as a normal STP port.

## Recommended Settings

- Enable PortFast and BPDU Guard however you prefer (per-port or by default).
	- Only enable BPDU Filter by default (global config mode).
		- Unless you have a very good reason to enable it per-port


**BPDU Guard and BPDU Filter can be enabled on the same port at the same time:**

- If BPDU Filter is enabled in global config mode and the port receives a BPDU:
	1. BPDU Filter will be disabled.
	2. BPDU Guard will be triggered (and err-disable the interface).
- If BPDU Filter is enabled in interface config mode and the port receives a BPDU:
	1. The BPDU will be ignored.
	2. BPDU Guard will not be triggered

## **Summary**

• **PortFast** should only be enabled on ports connected to non-switch devices (end hosts, routers) that don’t send BPDUs.  
 • A PortFast-enabled port still sends BPDUs and will operate like a regular STP port if it receives BPDUs from a neighbor.  
 • If an end user carelessly connects a switch to a port meant for end hosts, it could affect the STP topology

• **BPDU Guard** protects the network from unauthorized switches being connected to ports intended for end hosts.  
 • If the port receives a BPDU, it enters the **error-disabled (err-disabled)** state, effectively disabling the port.  
 • Per-port: SW1(config-if)# **spanning-tree bpduguard enable**  
 • Default: SW1(config)# **spanning-tree portfast [edge] bpduguard default**  
  • Enables BPDU Guard on **all PortFast-enabled ports.**  
  • Use **spanning-tree bpduguard disable** to disable it on specific ports.

• An err-disabled port can be re-enabled in two ways:  
 1. Manual: **shutdown** and **no shutdown**  
 2. Automatic: **ErrDisable Recovery**  
  • SW1(config)# **errdisable recovery cause bpduguard**  
 • In either case, make sure you fix the underlying problem that caused the port to be err-disabled.

• **BPDU Filter** prevents a port from sending BPDUs.  
 • Unlike BPDU Guard, it does not disable the port if it receives a BPDU.  
 • Per-port: SW1(config-if)# **`spanning-tree bpdufilter enable`**  
  • The port will ignore any BPDUs it receives. ***Use with caution!***  
 • Default: SW1(config)# **`spanning-tree portfast [edge] bpdufilter default`**  
  • Enables BPDU Filter on **all PortFast-enabled ports.**  
  • If the port receives a BPDU, PortFast and BPDU Filter are disabled, and it operates as a normal STP port.  
  • Use **`spanning-tree bpdufilter disable`** to disable it on specific ports.


---


# **Root Guard**

- STP prevents loops by electing a root bridge and ensuring that each other switch has only one valid path to reach it.

- You shouldn't randomly select the root bridge. Some things you should consider include:
	- Optimal traffic flow
		- minimize latency
		- minimize congestion 
	- Stability and reliability 

![[Pasted image 20260304182123.png]]


## Root Guard: The Problem

![[Pasted image 20260304182535.png]]

- Within your own LAN, you can easily control the root bridge by setting its priority to 0.
	- But there are cases where you might connect your LAN to other switches outside of your direct control:
		- A service provider offering *Metro Ethernet* service to customers often used to connect sites within a MAN (Metropolitan Area Network)

- Even if you set your root bridge's priority to 0, its role can be taken by another switch with a lower MAC address.

![[Pasted image 20260304183430.png]]

- With no safeguard in place, SW1, SW2 and SW3 accept SW6 as the root bridge, affecting the service provider's STP topology.
	- Frames from SW3 to SW1 must take a detour through the customer's LAN.

- **Root Guard** can be configured to protect your STP topology by preventing your switches from accepting superior BPDUs from switches outside of your control.
	- Superior BPDU = a BPDU that is superior in the STP algorithm (e.g. claiming a better root bridge ID).


## Root Guard: The Solution

![[Pasted image 20260304183844.png]]

- If you want to ensue that the root bridge remains in your LAN, you can configure Root Bridge on the ports connected to switches outside of your control.
	- SW2 G0/2, SW3 G0/2

- Use `SW2(config-if)#spanning-tree guard root enable` Root Guard on a port.
	- There is no command to enable it by default from global config mode.

- If a Root Guard-enabled port receives a BPDU, it will enter the Broken (*Root Inconsistent*) state, effectively disabling it.
	- The port will not be able to forward data frames and will discard any frames it receives.
	- SW1, SW2 and SW3 won't accept SW6 as the root bridge.


![[Pasted image 20260304184432.png]]

- To re-enable a port disabled by Root Guard, you must solve the issue that disabled the port.
	- The disabled port must stop receiving superior BPDUs.
	- Tell the customer to increase the priority value of their switch.

- Once the superior BPDUs received by SW2 G0/2 and SW3 G 0/3 age out, the ports will automatically be re-enabled.
	- A BPDU's Max Age is 20 seconds by default.


### Root Guard: CLI demonstration

![[Pasted image 20260304184933.png]]

## **Summary**

• When selecting a LAN’s root bridge, you should consider the following:  
 • Optimal traffic flow  
  • minimize latency  
  • minimize congestion  
 • Stability and reliability

• Within your own LAN, you can easily control the root bridge by setting its priority to 0.  
 • There are cases where you might connect your switches to other switches outside of your control (e.g. service provider + client).

• **Root Guard** can be configured on specific ports to prevent them from accepting **superior BPDUs** from switches outside of your control.

• Use SW(config-if)# **`spanning-tree guard root`** to enable Root Guard on a port.  
 • There is no command to enable it by default from global config mode.

• Root Guard prevents a port from becoming a root port if it receives a superior BPDU.  
 • If the port receives a superior BPDU, it becomes Broken (BKN) / Root Inconsistent (ROOT_Inc).

• If the port stops receiving superior BPDUs, it will automatically recover.

# **Loop Guard**

## Unidirectional Links

![[Pasted image 20260304190435.png]]

- A *unidirectional link* is a network link where data transmission occurs in only one direction.
	- e.g. SW1 can send frames to SW2 can't send frame to SW2

![[Pasted image 20260304190809.png]]

- For a fiber-optic interface to be up/up, both fibers must be connected and functional.
- If there is a physical problem with either fiber, the devices should be able to detect it and disable their interfaces.
- If the devices fail to detect the physical problem, it could result in a unidirectional link.

## Loop Guard - The Problem

![[Pasted image 20260304191130.png]]

- BPDUs originate from the Root bridge and are forwarded out of Designated ports.
- SW3 G0/1 is a Non-Designated blocking port because it receives superior BPDUs from SW2.
	- SW2 has a superior root cost or bridge ID.

- If the SW2-SW3 link becomes unidirectional and SW2's BPDUs can't reach SW3, what will happen?
	- SW3 G0/1 will become a Designated port and start forwarding BPDUs.

- Because SW3's BPDUs are inferior to SW2's, SW2 simply ignores SW3's BPDUs and forward it to SW1. 
	- SW2 G0/1 and SW3 G0/1 are both in the forwarding state.
		- ***SW1-SW3-SW2 LOOP!***

## Loop Guard - The Solution 

- When a **Loop Guard-enabled port’s Max Age timer counts down to 0**, it doesn’t become a **Designated port** and start transitioning to **Forwarding**.
    - It enters the **Broken (Loop Inconsistent)** state.
    - Like the **Broken (Root Inconsistent)** state triggered by a **Root Guard** violation, this blocks the port.


>In both cases, the port remains **up/up**, but **STP blocks it**.



- If the broken port starts receiving **BPDUs** again, it will be automatically re-enabled.

- **Loop Guard can be enabled in two ways:**
    - **Per-port:** `SW3(config-if)# spanning-tree guard loop`
    - **Default:** `SW3(config)# spanning-tree loopguard default`
        - This enables Loop Guard on all ports
        - Use `SW3(config-if)# spanning-tree guard none` to disable it on specific ports if needed.


>Loop Guard should be enabled on **Root and Non-Designated ports** (ports that are supposed to receive BPDUs).

![[Pasted image 20260304224027.png]]

![[Pasted image 20260304224123.png]]

![[Pasted image 20260304224158.png]]

- Loop Guard and Root Guard are mutually exclusive.
	- They can't be enabled on the same port at the same time.
	- Root Guard is meant to prevent Designated ports from becoming Root ports.
	- Loop Guard is meant to prevent Non-Designated ports from becoming Designated ports.

- If Loop Guard is configured on a port (`spanning tree guard loop`) and you then configure Root Guard (`spanning-tree guard root`), Loop Guard will disabled on th eport.

- If Loop Guard is enabled by default (`spanning-tree loopguard default`) and you then configure Root Guard on the port, Loop Guard will be disabled on the port.
	- The more specific configuration (interface vs globals) takes effect.

## **Summary**


• **Loop Guard** protects the network from loops by blocking a port if it unexpectedly stops receiving BPDUs.  
 • A software bug preventing a switch from sending BPDUs  
 • A hardware issue causing a unidirectional link.

• **A unidirectional link** is a network link where data transmission occurs in only one direction.  
 • Typically caused by Layer 1 issues on fiber-optic cables.  
  • If the connected devices don’t detect the issue and disable their interfaces, it can result in a unidirectional link.  
  • If a Root or Non-Designated port stops receiving BPDUs, it will become a Designated port, potentially causing a Layer 2 loop.

• If a **Loop Guard**-enabled port stops receiving BPDUs, it enters the **Broken (Loop Inconsistent)** state, effectively disabling the port.  
 • If it starts receiving BPDUs again, it will be automatically re-enabled.

• **Loop Guard can be enabled in two ways:**  
 • Per-port: `SW3(config-if)# spanning-tree guard loop`  
 • Default: `SW3(config)# spanning-tree loopguard default`  
  • This enables Loop Guard on all ports  
  • Use `SW3(config-if)# spanning-tree guard none` to disable it on specific ports if needed.

• **Loop Guard and Root Guard are mutually exclusive.**  
 • If Loop Guard is configured on a port and you then configure Root Guard, Loop Guard will be disabled on the port (and vice-versa).  
 • If Loop Guard is enabled by default (`spanning-tree loopguard default`) and you then configure Root Guard on a port, Loop Guard will be disabled on the port.









































