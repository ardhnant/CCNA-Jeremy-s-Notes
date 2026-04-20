## **Spanning Tree Port States**

- Root/Designated port remain stable in a Forwarding state.
- Non-Designated ports remain stable in a Blocking state.
- Listening and Learning are transitional states which are passed through when an interface is activated, or when a Blocking port must transition to a Forwarding state due to a change in network topology.

![[Pasted image 20260302133057.png]]


### **Blocking State**

- Non-Designated ports are in a blocking state.
- Interfaces in a blocking state are effectively disabled to prevent loops.
- Interfaces in a Blocking state do not send/receive regular network traffic.
- Interfaces in a Blocking state do NOT forward STP BPDUs.
- Interfaces in a Blocking state do NOT learn MAC addresses.

### **Listening State**

- After the Blocking state, interfaces with the Designated or Root role enter the Listening state.
- Only Designated or Root ports enter the Listening state (Non-designated ports are always Blocking).
- The Listening state is 15 seconds long by default. This is determined by the Forward delay timer.
- An interface in the Listening state ONLY forwards/receives STP BPDUs.
- An interface in the Listening state does NOT send/receives regular traffic.
- An interface in the Listening state does NOT learn MAC address from regular traffic that arrives on the interface.

### **Learning State**

- After Listening state, a Designated or Root port will enter the Learning state.
- The Learning state is 15 seconds long by default. This determined by the Forward delay timer (the same timer is used for both the Listening and learning states)
- An interface in the Learning state ONLY sends/receives STP BPDUs.
-  An interface in the Learning state does NOT sends/receives regular traffic.
-  An interface in the Learning state ONLY learns MAC addresses from regular traffic that arrives on the interface.

### **Forwarding State**

- Root and Designated ports are in Forwarding state.
- A port in the Forwarding state operates as normal.
- A port in the Forwarding state sends/receives BPDUs.
- A port in the Forwarding state sends/receives normal traffic.
- A port in the Forwarding state learns MAC addresses.

### **Summary of Spanning Tree Port States**

![[Pasted image 20260303123741.png]]


## **Spanning Tree Timers**


![[Pasted image 20260303124023.png]]

> Switches do not forward the BPDUs out of their root ports and non-designated ports, only their *designated* ports.


### **Max Age**

- If another BPDU is received before the max age timer counts down to 0, the time will reset to 20 seconds and no changes will occur.
- If another BPDU is not received, the max age timer counts down to 0 and the switch will reevaluate its STP choices, including root bridge, and local root, designated and non-designated port.
- If  a non-designated port is selected to become a designated or root port, it will transition from the blocking state to listening state (15 seconds), learning state (15 seconds), and interface to transition to forwarding.
- These timers and transitional states are to make sure that loops aren't accidentally created by an interface moving to forwarding state too soon.

> A forwarding interface can move directly to a blocking state (there is no worry about creating a loop by blocking an interface). A blocking interface cannot move directly to forwarding state. It must go though the listening and learning states.

![[Pasted image 20260303131301.png]]

> PVST = Only ISL trunk encapsulation 
> PVST+ = Supports 802.1Q

> Regular STP (non Cisco's PVST+) uses a destination MAC address of 0180.c200.00g0
> On PVST, it uses destination MAC address of 0100.0ccc.cccd.


## **Portfast**

- Portfast allows a port to move immediately to the forwarding state, bypassing listening and learning.
- If used, it must be enabled only on ports connected to end hosts.
- If enabled on a port connected to another switch it could cause a Layer 2 loop.

![[Pasted image 20260303132926.png]]


``` cisco

SW1(config)#interface g0/2
SW1(config-if)#spanning-tree portfast
%Warning: portfast should only be enabled on ports connected to a single host.
Connecting hubs, concentrators, switches, bridges, etc... to this interface
when portfast is enabled, can cause temporary bridging loops. Use with  CAUTION 

%Portfast has been configured on GigabitEthernet0/2 but will only have effect
 when the interface is in a non-trunking mode.

SW1(config-if)#
```

>You can also enable portfast with the following command: `SW1(config)# spanning-tree portfast default`. This enables on all access ports (not trunk ports).


### **BPDU Guard**

- If an interface with BPDU Guard enabled receives a BPDU from another switch, the interface will be shut down to prevent a loop from forming.

``` cisco

SW1(config)#interface g0/2
SW1(config-if)#spanning-tree bpdugrard enabled
SW1(config-if)#
```

>You can also enable BDPU Guard with the following command: `SW1(config)# spanning-tree portfast bdpuguard default`. This enables BDPU Guard on all Portfast-enabled interfaces.
 
![[Pasted image 20260303134517.png]]

- This is the screenshot of a switch where BDPU Guard was enabled and then a BDPU message arrived at that port, The port was forcefully shutdown immediately.

#### To enable the port again 

We can use the following command:

![[Pasted image 20260303134740.png]]

#### Root Guard 

- If you enable root guard on an interface, even if it receives a superior BPDU (lower bridge ID) on that interface, the switch will not accept the new switch as the root bridge. The interface will be disabled.

#### Loop Guard

- If you enable loop guard on an interface, even if the interface stops receiving BPDUs, it will not start forwarding. The interface will be disabled.


> You prolly don't have to know these STP optional features (or other such as UplinkFast, Backbone Fast, etc) for the CCNA. But make sure you know Portfast and BPDU Guard. If you want to read more about the other just in case, do a Google search.

### **Mode in STP**

![[Pasted image 20260303135541.png]]

#### Manipulating Root Bridges

![[Pasted image 20260303135639.png]]

> The `spanning-tree vlan *vlan-number* root primary` command sets the STP priority to 24576. If another switch already has a priority lower than 24576, it sets this switch's priority 4096 less that the other switch's priority.

![[Pasted image 20260303140137.png]]


![[Pasted image 20260303140205.png]]

- The `spanning-tree vlan *vlan-number* root secondary` command sets the STP priority to 28672.

![[Pasted image 20260303140442.png]]

![[Pasted image 20260303140457.png]]


![[Pasted image 20260303140727.png]]


- To increase the cost of a certain port you must first get into the interface:

```cisco
int f0/1
spanning-tree vlan 1 cost 100
```

![[Pasted image 20260307151304.png]]


























































































