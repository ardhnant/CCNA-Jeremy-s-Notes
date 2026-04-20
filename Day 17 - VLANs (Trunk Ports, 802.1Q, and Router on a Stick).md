## **1. Lesson Overview**

This lesson continues VLAN fundamentals and introduces the remaining basic concepts required to properly use VLANs in real networks.

### **Topics Covered**


1. Trunk ports and their purpose
 
2. 802.1Q (dot1q) VLAN tagging
    
3. Trunk port configuration on Cisco switches
    
4. Native VLAN concept
    
5. Router on a Stick (ROAS) for inter-VLAN routing
    

Goal: understand how multiple VLANs communicate efficiently using fewer physical interfaces.

---

## **2.  Trunk Port**

- Carries traffic from **multiple VLANs**
    
- Used between:
    
    - Switch ↔ Switch
        
    - Switch ↔ Router
        
- Frames are **tagged** to identify VLAN membership
    

Other terminology:

- Trunk port = **tagged port**
    
- Access port = **untagged port**
    

---

## **3. Why Trunk Ports Are Needed**

In small networks:

- Separate physical links can be used for each VLAN.
    

Problem as network grows:

- Too many interfaces required.
    
- Waste of physical ports.
    
- Routers may not have enough interfaces.
    

### **Solution**

Use a **single physical link** that carries traffic for multiple VLANs → trunk port.

Benefits:

- Efficient interface usage
    
- Reduced cabling
    
- Easier scalability
    

---

## **4. Example Topology Concept**

![[Pasted image 20260215170636.png]]    

Important concept:

- Even without a VLAN20 link between switches, VLAN20 devices can reach VLAN10 devices because:
    
    - Traffic goes to router (default gateway)
        
    - Router routes traffic between VLANs
        
    - Traffic returns in destination VLAN
        

Layer 2 switches:

- Forward traffic **only within same VLAN**
    
- Cannot route between VLANs
    

---

## **6. Trunking Protocols**

Two trunking protocols exist:

### **1. ISL (Inter-Switch Link)**

- Cisco proprietary
    
- Older technology
    
- Rarely used today
    
- Not supported on modern equipment
    

### **2. IEEE 802.1Q (dot1q)**

- Industry standard
    
- Used in modern networks
    
- Required for CCNA
    

For CCNA:

- Focus on **802.1Q**
    

---

## **7. 802.1Q Tag Basics**

![[Pasted image 20260215171020.png]]

### **Tag Placement**

- Inserted into Ethernet frame
    
- Between:
    
    - Source MAC Address
        
    - Type/Length field
        

### **Tag Size**

- 4 bytes (32 bits)
    

---

## **8. Structure of the 802.1Q Tag**

The tag consists of two main fields:

![[Pasted image 20260215171621.png]]

### **1. TPID (Tag Protocol Identifier)**

- Length: 16 bits (2 bytes)
    
- Value: **0x8100**
    
- It works as the identity of 802.1Q-tag... it tells that here is where the dot1q-tagged frame starts    

---

### **2. TCI (Tag Control Information)**

TCI contains three subfields:

#### **a. PCP (Priority Code Point)**

- 3 bits
    
- Used for Class of Service (CoS)
    
- CoS prioritizes important traffic
    

#### **b. DEI (Drop Eligible Indicator)**

- 1 bit
    
- Indicates frames that may be dropped during congestion

- So basically make way for important traffic to pass through first.
    

#### **c. VID (VLAN ID)**

- 12 bits
    
- Identifies VLAN number


---

## **9. VLAN ID Range**

VID is 12 bits:

```
2^12 = 4096 VLANs
```

Reserved:

- VLAN 0
    
- VLAN 4095
    

Usable VLAN range:

```
1 – 4094
```

---

### **VLAN Ranges**

|Type|Range|
|---|---|
|Normal VLANs|1 – 1005|
|Extended VLANs|1006 – 4094|

Notes:

- Older switches may not support extended VLANs.

- Modern switches typically support full range.
    

---

## **10. Native VLAN**

- 802.1Q has a feature called the native VLAN.
- ISL does not have this feature.
- The native VLAN is VLAN 1 by default on all trunk ports however this can be manually configured on each trunk port. 
- The switch does not add an 802.1Q tag to frames in the native VLAN.
- When a switch receives an untagged rame on a trunk port, it assumes the frame belongs to the native VLAN.

![[Pasted image 20260215182326.png]]



---

### **Native VLAN Mismatch Problem**

If SW1 native VLAN = 30 and SW2 native VLAN = 10:

- Untagged frame from VLAN10 arrives
    
- Receiving switch assumes wrong VLAN
    
- Traffic dropped or misdirected

 ![[Pasted image 20260215182433.png]]
   

---

## **11. Configuring Trunk Ports**

![[Pasted image 20260215182535.png]]

### **Basic Trunk Configuration**

![[Pasted image 20260215182704.png]]

- Modern switches do not support Cisco's ISL at all.
- Switches do not support both have a a trunk encapsulation of 'Auto' by default.
- To manually configure the interface as a trunk port, you must first set the encapsulation to 802.1Q or ISL. On switches that only support 802.1Q, this is not necessary.
- After you set the encapsulation type, you can then configure the interface as a trunk.


---

## **12. Verifying Trunk Configuration**

### **Command**

```
show interfaces trunk
```

![[Pasted image 20260216113158.png]]

Displays:

- Trunk interfaces
    
- Mode (on/manual)
    
- Encapsulation type
    
- Native VLAN
    
- Allowed VLANs
    
- Active VLANs
    

---

## **13. VLANs Allowed on a Trunk**

Default:

```
All VLANs (1–4094)
```

For security and performance, limit allowed VLANs.

---

![[Pasted image 20260216113815.png]]
### **Commands**

#### Allow specific VLANs

```
switchport trunk allowed vlan 10,30
```

#### Add VLAN

```
switchport trunk allowed vlan add 20
```

#### Remove VLAN

```
switchport trunk allowed vlan remove 20
```

#### Allow all VLANs

```
switchport trunk allowed vlan all
```

#### Allow all except specific VLANs

```
switchport trunk allowed vlan except 1-5,10
```

#### Allow none

```
switchport trunk allowed vlan none
```


---

## **14. Native VLAN Configuration**

- It is better to have native VLAN as an unused VLAN for security purposes.

Command:

```
switchport trunk native vlan 1001
```

Notes:

- Configure per interface.
    
- Must match on both trunk ends.
    

---

## **15. SHOW VLAN BRIEF Behavior**

Important concept:

- Displays **access ports only**
    
- Does NOT show trunk ports
    

Use:

```
show interfaces trunk
```

to verify trunk interfaces.

## **Quick Summary** 

![[Pasted image 20260216114817.png]]


---

## **16. Router on a Stick (ROAS)**

### **Definition**

Method of inter-VLAN routing using:

- One physical router interface
    
- Multiple logical subinterfaces
    

Purpose:

- Route between VLANs efficiently
    
- Avoid multiple physical interfaces

![[Pasted image 20260216115048.png]]

---

### **Concept**

Single router interface:

```
G0/0
```

Subinterfaces:

```
G0/0.10 → VLAN10
G0/0.20 → VLAN20
G0/0.30 → VLAN30
```

Each subinterface behaves like a separate interface.


---

## **18. Router Subinterface Configuration**

### **Steps**

1. Enable physical interface
    

```
no shutdown
```

2. Create subinterface
    

```
interface g0/0.10
```

3. Assign VLAN tag
    

```
encapsulation dot1q 10
```

4. Assign IP address
    

```
ip address X.X.X.X MASK
```

5. Make a native VLAN

```
encapsulation dot1q {vlan-id} native
```

Repeat for each VLAN.

![[Pasted image 20260216115224.png]]

---

### **Important Notes**

- Subinterface number does not have to match VLAN ID.
    
- Matching numbers is recommended for clarity.
    
- Router tags outgoing frames with configured VLAN ID.
    

---

## **ROAS Key Summary**

- ROAS is used to route between multiple VLANs using a single interface on the router and switch.
- The switch interface is configured as a regular trunk.
- The router interface is configured using **subinterfaces**. You can configure the VLAN tag and IP address on each **subinterfaces.**
- The route will behave as if frames arriving with a certain VLAN tag have arrived on the subinterface configured with that VLAN tag.
- The router will tag frames sent out of each subinterfaces with the VLAN tag configured on the subinterface.

---

## **21. Key Concepts Summary**

- Trunk port carries multiple VLANs.
    
- Access port carries single VLAN.
    
- 802.1Q adds VLAN tag to frames.
    
- VID field identifies VLAN.
    
- Native VLAN traffic is untagged.
    
- Native VLAN must match across trunk.
    
- Allowed VLAN list improves security and efficiency.
    
- Router on a Stick enables inter-VLAN routing using one interface.
    
- Router uses subinterfaces with dot1q encapsulation.
    

