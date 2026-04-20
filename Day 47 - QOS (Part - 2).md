
## Classification 

- The purpose of QoS is to give certain kinds of network traffic priority over others during congestion.
- Classification organizes network traffic (packets) into traffic classes (categories).
- Classification is fundamental to QoS. To give priority to certain types of traffic, you have to identify which types of traffic to give priority to.

- There are many methods of classifying traffic. Some examples:  
	- An ACL. Traffic which is permitted by the ACL will be given certain treatment, other traffic will not.  
	- NBAR (Network Based Application Recognition) performs a deep packet inspection, looking beyond the Layer 3 and Layer 4 information up to Layer 7 to identify the specific kind of traffic.  
	- In the Layer 2 and Layer 3 headers there are specific fields used for this purpose.

- The PCP (Priority Code Point) field of the 802.1Q tag (in the Ethernet header) can be used to identify high/low priority traffic.  
	- Only when there is a dot1q tag!

- The DSCP (Differentiated Services Code Point) field of the IP header can also be used to identify high/low priority traffic.

## PCP/CoS

![[Pasted image 20260404224431.png]]

- Because PCP is found in the dot1q header, it can only be used be used over the following connections:
	- trunk lines
	- access links with a voice VLAN

- In the diagram below, traffic between R1 and R2 or between R2 and external destinations will not have a dot1q tag. So, traffic over those links PCP cannot be marked with a PCP value.

![[Pasted image 20260404224655.png]]


## The IP ToS Byte

![[Pasted image 20260404224725.png]]

![[Pasted image 20260404224734.png]]


## IP Precedence

![[Pasted image 20260404224813.png]]

- Standard IPP markings are similar to PCP:
	- 6 and 7 are reserved for 'network control' traffic (ie. OSPF messages between routers)
	- 5 = voice
	- 4 = video 
	- 3 = voice signaling 
	- 0 = best effort

- With 6 and 7 reserved, 6 possible values remain.
- Although 6 values is sufficient for many networks, the QoS requirements of some networks demand more flexibility.

## DSCP 

![[Pasted image 20260404225128.png]]

- RFC 2474 (1998) defines the DSCP field, and other ‘DiffServ’ RFCs elaborate on its use.

- With IPP updated to DSCP, new standard markings had to be decided upon.  
	- By having generally agreed upon standard markings for different kinds of traffic, QoS design & implementation is simplified, QoS works better between ISPs and enterprises, among other benefits.

- You should be aware of the following standard markings:  
	- Default Forwarding (DF) – best effort traffic  
	- Expedited Forwarding (EF) – low loss/latency/jitter traffic (usually voice)  
	- Assured Forwarding (AF) – A set of 12 standard values  
	- Class Selector (CS) – A set of 8 standard values, provides backward compatibility with IPP

![[Pasted image 20260404225220.png]]


## DF/EF

DF (Default Forwarding)

![[Pasted image 20260404225320.png]]

- DF is used for best-effort traffic.
- The DSCP marking for DF is 0.


EF (Expedition Forwarding)

![[Pasted image 20260404225415.png]]

- EF is used for traffic that requires low loss/latency/jitter.
- The DSCP marking for EF is 46.

