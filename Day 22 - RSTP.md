## Spanning Tree Protocol
###  Industry standards (IEEE)

*Spanning Tree Protocol (802.1D)* 

- The original STP
- All VLANs share one STP instance.
- Therefore, cannot load balance.

*Rapid Spanning Tree Protocol (802.1w)*

- Much faster at converging/adapting to network changes than 802.1D
- All VLANs share one STP instance.
- Therefore, cannot load balance.

*Multiple Spanning Tree Protocol (802.1s)*

- Uses modified RSTP mechanics.
- Can group multiple VLANs into different instances (ie. VLAN 6-10 in instance 2) to perform load balancing.

### Cisco versions

*Per-VLAN Spanning Tree Plus (PVST+)*

- Cisco's upgrade to 802.1D 
- Each VLAN has its own STP instance.
- Can load balance by blocking different ports in each VLAN.

*Rapid Per-VLAN Spanning Tree Plus (Rapid PVST+)*

- Cisco's upgrade to 802.1w 
- Each VLAN has its own STP instance.
- Can load balance by blocking different ports in each VLAN.

> This is what falls under CCNA.


## RSTP

**Cisco Summary**
RSTP is not a timer-based spanning tree algorithm like 802.1D. Therefore, RSTP offers an improvement over the 30 seconds or more that 802.1D takes to move a link to forwarding. The heart of the protocol is a new bridge-bridge handshake mechanism, which allows ports to move directly to forwarding.

**Similarities between STP and RSTP:**
- RSTP serves the same purpose as STP, blocking specific ports to prevent Layer 3 loops.
- RSTP elects a root bridge with the same rules as STP.
- RSTP elects root ports with the same rules as STP.
- RSTP elects designated ports with the same rules as STP.

#### Cost in RSTP 

![[Pasted image 20260306005008.png]]


#### Spanning Tree Port States

![[Pasted image 20260306005131.png]]

- If a port is administratively disabled (shutdown command) = discarding state
- If a port is enabled but blocking traffic to prevent Layer 2 loops = discarding state.

#### RSTP port roles

- The root port role remains unchanged in RSTP.
	- The port that is closest to the root bridge becomes the root port for the switch.
	- The root bridge is the only switch that doesn't have a root port.

- The designated port role remains unchanged in RSTP.
	- The port on a segment (collision domain) that sends the best BPDU is the segment's designated port (only one per segment)

- The non-designated port role is split into two separate roles in RSTP:
	- the alternate port role
	- the backup port role

#### RSTP: Alternate port role

- The RSTP alternate port role is a discarding port that receives a superior BPDU from another switch.
- This is the same as what you've learned about blocking ports in classic STP.
- Function as a backup to the root port.
- If the root port fails, the switch can immediately move its best alternate port to forwarding.

![[Pasted image 20260306105319.png]]

>This immediate move to forwarding state function like a classic STP optional feature called UplinkFast. Because it is build into RSTP, you do not deed to activate UplinkFast when using RSTP/Rapid PVST+.


#### RSTP: BackboneFast functionality 

- One more STP optional feature that was built into RSTP is BackboneFast.
- BackboneFast allows SW3 to expire the made age timers on its interface and rapidly forward the superior BPDUs to SW2.
- This functionality is built into RSTP, so it does not need to be configured.

![[Pasted image 20260306125831.png]]


### UplinkFast / BackboneFast Summary 

- **UplinkFast** and **BackboneFast** are two optional features in classic STP. They must be configured to operate on the switch (not necessary to know for CCNA).
- Both features are built into RSTP, so you do not have to configure them. They operate by default.
- You do not need to have a detailed understanding of them for the CCNA. Know their names and their basic purpose (to help blocking/discarding ports rapidly move to forwarding).
- If you want to learn more, do a Google search for 'spanning tree uplinkfast' or 'spanning tree backbonefast'.

## RSTP: Backup port role

![[Pasted image 20260306131013.png]]

 - The RSTP *backup* port role is a discarding port that receives a superior BPDU from another interface on the same switch.
 - This only happens when two interfaces are connected to the same collision domain (via a hub)
 - Hubs are not used in modern networks, so you will probably not encounter an RSTP backup port.
 - Function as a backup for a designated port.

>The interface with the lowest port ID will be selected as the designated port, and the other will be the backup port.

## Rapid Spanning Tree Protocol 

![[Pasted image 20260306131435.png]]

![[Pasted image 20260306131459.png]]


> Rapid STP is compatible with Classic STP. The interface(s) on the Rapid STP-enabled switch connected to the Classic STP-enabled switch will operate in Classic STP mode (timers, blocking -> listening -> learning -> forwarding process, etc).


### Rapid Spanning Tree BPDU
  ![[Pasted image 20260306132052.png]]

**Things to remember**
- STP has a version of 0 while RSTP has version of 2.
- STP use 2 bits while RSTP use all 8 bits in order to communicate.
- That's all you need to know about this shit.

> In classic STP, only the root bridge originated BPDUs, and other switches just forwarded the BPDUs they received. In RSTP, all switches originate and send their own BPDUs from their designated ports.


---

- All switches running Rapid STP send their own BPDUs every hello time (2 seconds).
- Switches 'age' the BPDU information much more quickly. In classic STP, a switch waits 10 hello intervals (20 seconds). In rapid STP, a switch considers a neighbor lost if it misses 3 BPDUs (6 seconds). It will then 'flush' all MAC addresses learned on that interface.

![[Pasted image 20260306133625.png]]

![[Pasted image 20260306133649.png]]

## RSTP Link Types

- RSTP distinguishes between three different 'link types'.
- **Edge**: a port that is connected to an end host. Moves directly to forwarding, without negotiation.
- **Point-to-point**: a direct connection between two switches.
- **Shared**: a connection to a hub. Must operate in half-duplex mode.

### RSTP Link Types: Edge

- Edge port are connected to end hosts.
- Because there is no risk of creating a loop, they can move straight to the forwarding state without the negotiation process.
- They function like a classic STP port with PortFast enabled.

**Command**: `spanning-tree portfast`

(e=portfast ports, p=point-to-point ports, s=shared ports)

![[Pasted image 20260306134151.png]]


### RSTP Link Type: Point-to-point

- Point-to-point ports connect directly to another switch.
- They function in fully-duplex.
- You don't need to configure the interface as point-to-point (it should be detected).

**Command**: `spanning-tree link-type point-to-point`

![[Pasted image 20260306134600.png]]


### RSTP Link Type: Shared

- Shared ports connect to another switch (or switches) via a hub.
- They function in half-duplex.
- You don't need to configure the interface as shared (it should be detected).

**Command**: `spanning-tree link-type shared`

![[Pasted image 20260306134802.png]]

# Quiz

2. ![[Pasted image 20260306134901.png]]

3. ![[Pasted image 20260306135359.png]]

4. ![[Pasted image 20260306135435.png]]
























































































