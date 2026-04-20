- Virtual Routing & Forwarding is used to divide a single router into multiple virtual routers.  
	   - Similar to how VLANs are used to divide a single switch (LAN) into multiple virtual switches (VLANs).
   
- It does this by allowing a router to build multiple separate routing tables.  
   - Interfaces (Layer 3 only) & routes are configured to be in a specific VRF (aka VRF Instance).  
   - Router interfaces, SVIs & routed ports on multilayer switches can be configured in a VRF.
   
- Traffic in one VRF cannot be forwarded out of an interface in another VRF.  
   - As an exception, VRF Leaking can be configured to allow traffic to pass between VRF’s.
   
- VRF is commonly used to facilitate MPLS.  
   - The kind of VRF we are talking about is VRF-lite (VRF without MPLS).
   
- VRF is commonly used by service providers to allow one device to carry traffic from multiple customers.  
   - Each customer’s traffic is isolated from the others.  
   - Customer IP addresses can overlap without issues.

![[Pasted image 20260403152957.png]]


## VRF Configuration 

![[Pasted image 20260403154537.png]]

![[Pasted image 20260403154551.png]]

![[Pasted image 20260403154620.png]]

![[Pasted image 20260403154656.png]]


---

![[Pasted image 20260403154723.png]]

![[Pasted image 20260403154734.png]]

![[Pasted image 20260403154754.png]]


# **Quiz**

![[Pasted image 20260403154824.png]]

![[Pasted image 20260403154841.png]]

![[Pasted image 20260403154858.png]]
