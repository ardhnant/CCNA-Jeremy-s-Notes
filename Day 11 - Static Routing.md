## **1. Purpose of Static Routing**

Routers use a **routing table** to decide where to forward packets.

Previously covered routes:

### **1. Local Routes (L)**

- Automatically added when an IP address is configured on an interface.
    
- Provide a route to the router’s **own IP address**.
    
- Always a **/32** route (single IP).
    

Example:

```
192.168.1.1/32
```

---

### **2. Connected Routes (C)**

- Automatically added when an interface has an IP address and is enabled.
    
- Provide a route to the **directly connected network**.
    

Example:

```
192.168.1.0/24
```

---

### **Limitation**

Local and Connected routes only allow communication with:

- The router itself
    
- Directly connected networks
    

Routers **cannot reach remote networks** without additional routes.

---

## **2. Static Routes**

### **Definition**

A **static route** is a manually configured route that tells a router how to reach a remote network.

Used when:

- Destination network is not directly connected.
    
- Router needs explicit instructions on where to forward packets.
    

---

### **Key Characteristics**

- Not added automatically.
    
- Must be configured manually.
    
- Appear in routing table with code:
    

```
S = Static
```

---

### **Purpose**

Static routes act as instructions:

> “To reach network X, send packets to next-hop Y.”

---

## **3. Review: Routing Tables Example**

### **General Rule**

For each active interface:

- 1 Connected route
    
- 1 Local route
    

So:

|Interfaces|Connected Routes|Local Routes|Total|
|---|---|---|---|
|2 interfaces|2|2|4 routes|
|3 interfaces|3|3|6 routes|

---

### **Example: R2**

![[Pasted image 20260211125834.png]]

Interfaces:

- 192.168.12.2/24
    
- 192.168.24.2/24
    

Routing table contains:

- C 192.168.12.0/24
    
- C 192.168.24.0/24
    
- L 192.168.12.2/32
    
- L 192.168.24.2/32
    

R2 knows:

- Its own IPs
    
- Connected networks
    

R2 does NOT know:

- Remote networks → packets dropped.
    

---

### **Same Logic Applies to R3 and R4**

Each router:

- Knows local IPs and connected networks.
    
- Drops packets destined for unknown networks.
    

---

![[Pasted image 20260211130023.png]]

---

## **4. Default Gateway (End Host Concept)**

End hosts (PCs):

- Can directly communicate within their local network.
    
- Must use a **default gateway** to reach remote networks.
    

### **Default Gateway**

- Another name for the router.
    
- Configured on hosts.
    

Example:

```
gateway 192.168.1.1
```

---

### **Default Route**

A default gateway is implemented as:

```
0.0.0.0/0
```

Meaning:

- Matches all IP addresses.
    
- Least specific route possible.
    

Comparison:

|Route Type|Specificity|
|---|---|
|/32 (Local)|Most specific|
|/24|Less specific|
|/0 (Default)|Least specific|

---

### **Default Route Behavior**

If no more specific route exists:

- Use default route instead of dropping packet.
    

---
### **Layer 2 (Ethernet Layer)**

MAC addresses change at every hop.

Process:

1. PC1 sends frame to default gateway MAC.
    
2. Router removes Ethernet frame.
    
3. Router checks routing table.
    
4. Router re-encapsulates packet with new MAC addresses.
    
5. Process repeats until destination.

![[Pasted image 20260211130739.png]]
this is the routing table at the moment when frame was received by R1

Only at final hop:

- Destination IP and MAC belong to same device.


---

## **6. Need for Static Routes (PC1 ↔ PC4 Example)**

Problem:

- Routers only know connected networks.
    
- No routes to remote networks.
    

Result:

- Packets dropped.
    

Solution:

- Configure static routes on routers along the path.
    

---

### **Two-Way Reachability**

Required for communication.

Each router must know:

- Route to source network
    
- Route to destination network
    

Otherwise:

- Replies fail.
    
- Ping fails.
    

---

### **Important Concept**

Routers do NOT need routes to every intermediate network.

Example:

- R1 only needs to know:
    
    - “To reach 4.0/24 → send to R3”
        
- R3 handles next decision.
    

---

## **7. Static Route Configuration**

### **Command Format**

```
ip route DESTINATION_NETWORK NETMASK NEXT_HOP_IP
```

Example: `ip route 192.168.4.0 255.255.255.0 192.168.13.3`

```
ip route 192.168.4.0 255.255.255.0 192.168.13.3
```

Meaning:

- Send traffic for 4.0/24 to next-hop 192.168.13.3.
    

---

### **Routing Table Entry**

Shows:

```
S 192.168.4.0/24 [1/0] <-- this number
```

Numbers represent:

- Administrative Distance
    
- Metric
    

(Explained later in course.)

---

## **8. Static Route Configuration Example**

Required routes for communication:

![[Pasted image 20260211130023.png]]

### **R1**

- Route to 192.168.4.0/24 via R3

![[Pasted image 20260211133415.png]]

### **R3**

- Route to 192.168.1.0/24 via R1
    
- Route to 192.168.4.0/24 via R4

![[Pasted image 20260211133447.png]]

### **R4**

- Route to 192.168.1.0/24 via R3

![[Pasted image 20260211133521.png]]

After configuration:

- Ping successful
    
- Confirms two-way reachability.
    

---

## **9. Static Route Options**

Static routes can be configured in three ways:

### **1. Next-Hop Only (Most Common)**

```
ip route NETWORK MASK NEXT_HOP_IP
```

---

### **2. Exit Interface Only**

```
ip route NETWORK MASK INTERFACE
```

Example:

```
ip route 192.168.1.0 255.255.255.0 g0/0
```

Routing table may show:

```
is directly connected
```

Even if it is not truly connected.

Uses **Proxy ARP** internally.

---

### **3. Both Exit Interface and Next-Hop**

```
ip route NETWORK MASK INTERFACE NEXT_HOP_IP
```

Example:

```
ip route 192.168.4.0 255.255.255.0 g0/1 192.168.24.4
```

![[Pasted image 20260211135500.png]]

---

### **Note**

None of these methods is inherently better.  
Next-hop only is commonly used.

---

## **10. Default Routes on Cisco Routers**

### **Definition**

A default route matches all destinations:

```
0.0.0.0/0
```

Used when:

- No specific route exists.
    

Common usage:

- Sending traffic to the Internet.
    

---

### **Configuration**

```
ip route 0.0.0.0 0.0.0.0 NEXT_HOP_IP
```

Example:

```
ip route 0.0.0.0 0.0.0.0 203.0.113.2
```

---

### **Routing Table Indicators**

- `S*` → candidate default route
    
- “Gateway of last resort” → selected default route
    

---

## **11. Packet Flow Summary (High Level)**

1. PC sends packet to default gateway.
    
2. Router checks routing table.
    
3. Finds most specific route.
    
4. Forwards packet to next hop.
    
5. Re-encapsulation occurs at every hop.
    
6. IP addresses remain unchanged.
    
7. MAC addresses change each hop.
    

---

## **12. Important Exam Concepts**

- Static routes must be manually configured.
    
- Each router needs routes for both directions.
    
- Most specific route always wins.
    
- Default route used only if no better match exists.
    
- Source/Destination IP do not change across network.
    
- MAC addresses change at each hop.
    
- Cisco static routes require subnet mask (not /prefix).
    

---

## **13. Static Routing Commands Summary**

|Command|Purpose|
|---|---|
|ip route A.B.C.D MASK NEXT_HOP|Static route using next-hop|
|ip route A.B.C.D MASK INTERFACE|Static route using exit interface|
|ip route A.B.C.D MASK INTERFACE NEXT_HOP|Both methods|
|ip route 0.0.0.0 0.0.0.0 NEXT_HOP|Default route|

---

## **14. Quiz Logic (What CCNA Expects)**

1. ![[Pasted image 20260211142244.png]]
2. ![[Pasted image 20260211142340.png]]
3. ![[Pasted image 20260211142414.png]]
4. ![[Pasted image 20260211142853.png]]
5. ![[Pasted image 20260211143009.png]]