## **1. Purpose of Routing (Routing Fundamentals Overview)**

Routing is a **Layer 3 (Network Layer)** function performed by routers.

Its purpose is to:

- Forward packets toward their correct destination
    
- Determine the best path for IP packets across networks
    
- Allow communication between different networks
    

Routing is different from switching:

- **Switches** forward frames using a **MAC address table**
    
- **Routers** forward packets using a **routing table**
    

This lesson focuses on:

- What routing is
    
- How routing tables work
    
- Connected and Local routes
    
- Route selection fundamentals
    

This topic is emphasized as **extremely important for CCNA and networking fundamentals**.

---

## **2. What is Routing?**

Routing is:

> The process routers use to determine the path that IP packets should take over a network to reach their destination.

When a router receives a packet:

1. It examines the destination IP address.
    
2. It checks its routing table.
    
3. It forwards the packet using the best route.
    

Routers store all known destinations in a **routing table**.

A route is essentially an instruction:

- To reach destination **X**, send packet to **next-hop Y**
    
- Next-hop = next router along the path
    

Possible router actions:

- Forward packet to next router
    
- Send packet directly to destination (if directly connected)
    
- Receive packet itself (if destination is its own IP)
    

---

## **3. Types of Routing Methods**

Routers learn routes using different methods.

### **3.1 Dynamic Routing**

- Routers automatically share routing information.
    
- Routing tables are built automatically.
    
- Uses routing protocols such as:
    
    - OSPF (covered later in the course)
        

### **3.2 Static Routing**

- Routes manually configured by a archgod.
    
- Covered in the next lesson.
    

### **3.3 Automatically Added Routes (Focus of This Lesson)**

Connected and Local routes:

- Not dynamic routes
    
- Not static routes
    
- Automatically added when an interface is configured and enabled
    

---

## **4. Example Network Used in the Lesson**

The example topology contains:

- Four routers (R1, R2, R3, R4) connected together
    
- Represents a **WAN (Wide Area Network)**
    
    - Network spanning large geographical areas
        
    - Routers may exist in different cities or countries
        

Two LANs exist:

- LAN connected to R1
    
- LAN connected to R4
    

Each LAN represents an office network with multiple hosts (only one PC shown for simplicity).

### **IP Addressing Scheme**

![[Pasted image 20260211090346.png]]

Note:

- Router IP addresses match router numbers (.1 for R1, .2 for R2, etc.)
    
- This is simplified for learning purposes and not typical in production networks.
    

---

## **5. Interface Configuration (R1 Example)**

Interfaces configured using:

```
IP ADDRESS <ip> <subnet-mask>
NO SHUTDOWN
```

Important behavior:

- From interface configuration mode, you can directly enter another interface using:
    
    ```
    INTERFACE G0/1
    ```
    
- No need to exit back to global configuration mode first.
    

Verification command:

```
SHOW IP INTERFACE BRIEF
```

Displays:

- Interface names
    
- Assigned IP addresses
    
- Interface status
    

---

## **6. Routing Table Overview**

Command: `SHOW IP ROUTE`

Displays the routing table.
![[Pasted image 20260211121225.png]]

The output contains two main sections:

1. **Codes legend**
    
    - Shows routing protocols and their codes
        
2. **Actual routes**
    

Important codes in this lesson:

|Code|Meaning|
|---|---|
|L|Local|
|C|Connected|

---

## **7. Automatically Added Routes**

When:

- An IP address is configured on an interface
    
- The interface is enabled (`NO SHUTDOWN`)
    

The router automatically adds:

- **1 Connected route**
    
- **1 Local route**
    

So:

- Each interface adds **two routes**.
    

Example:

3 interfaces → 6 routes total.

---

## **8. Connected Routes (Code C)**

A connected route:

- Represents the network connected to an interface.
    

Example:

Interface IP:

```
192.168.1.1/24
```

Connected route:

```
192.168.1.0/24
```

Explanation:

- First 24 bits = network portion
    
- Last 8 bits = host portion
    
- Setting host bits to 0 gives the network address.
    

Meaning:

- Router knows all devices in that network are reachable via that interface.
    
- Example matches:
    
    - 192.168.1.10
        
    - 192.168.1.100
        
    - 192.168.1.232
        

Routing table entry example:

```
192.168.1.0/24 is directly connected, GigabitEthernet0/2
```

---

## **9. Local Routes (Code L)**

A local route:

- Represents the exact IP address configured on the router interface.
    

Example:

Interface IP:

```
192.168.1.1/24
```

Local route:

```
192.168.1.1/32
```

Explanation:

- /32 mask = 255.255.255.255
    
- All bits fixed
    
- Matches only one IP address
    

Purpose:

- Tells router:
    
    - Packets to this address are for the router itself.
        
    - Do not forward them.
        

Router receives and processes the packet.

---

## **10. Route Matching**

A route **matches** a packet when:

- The destination IP belongs to the network specified by the route.
    

Example:

Route:

```
192.168.1.0/24
```

Matches:

- 192.168.1.2
    
- 192.168.1.7
    
- 192.168.1.89
    

Does NOT match:

- 192.168.2.1
    

If no matching route exists:

- Router drops the packet.
    

Important difference:

- Switches flood unknown frames.
    
- Routers never flood packets.
    

---

## **11. Route Selection (Most Important Concept)**

Routers may have multiple matching routes.

Rule:

> The router selects the **most specific matching route**.

Definition:

- Most specific = longest prefix length.
    

Example:

Two matching routes:

- 192.168.1.0/24 → 256 addresses
    
- 192.168.1.1/32 → 1 address
    

The /32 route is more specific.

Result:

- Router chooses the /32 local route.
    
- Packet is received by router instead of forwarded.
    

Summary rule:

1. Route must match destination.
    
2. Among matching routes, choose longest prefix length.
    

---

## **12. “Variably Subnetted” Output in Routing Table**

Example output:

![[Pasted image 20260211121225.png]]

```
192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
```

Important:

- These lines are **not routes**.
    
- They summarize routing table entries.
    

Meaning:

- Multiple subnet masks exist within that network
    
    - Example: /24 and /32 routes.
        

Can generally be ignored for now.

Understanding fully requires subnetting knowledge (covered later).

---

## **13. Route Selection Examples**

### Example 1

![[Pasted image 20260211121626.png]]

Destination: `192.168.1.1`

Matches:

- 192.168.1.0/24
    
- 192.168.1.1/32
    

Selected:

- /32 local route
    

Action:

- Router receives packet.
    

---

### Example 2

![[Pasted image 20260211121652.png]]

Destination: `192.168.13.3`

Matches:

- Connected route to 13.0/24
    

Action:

- Forward out G0/0.
    

---

### Example 3

![[Pasted image 20260211121708.png]]


Destination: `192.168.1.244`

Matches:

- Connected route to 1.0/24
    

Action:

- Forward packet.
    

---

### Example 4

![[Pasted image 20260211121722.png]]

Destination: `192.168.12.1`

Matches:

- Local route /32
    

Action:

- Router receives packet.
    

---

### Example 5

![[Pasted image 20260211121750.png]]

Destination: `192.168.4.10`

Matches:

- No routes
    

Action:

- Packet dropped.
    

---

## **14. Key Differences: Routers vs Switches**

|Feature|Switch|Router|
|---|---|---|
|Forwarding basis|MAC address|IP address|
|Unknown destination|Floods frame|Drops packet|
|Matching|Exact match|Most specific match|

---

## **15. Summary (One-Slide Review Concept)**

![[Pasted image 20260211121957.png]]

- Routing determines how packets travel across networks.
    
- Routers use routing tables to make forwarding decisions.
    
- A route is an instruction telling the router where to send packets.
    
- Configuring an interface automatically adds:
    
    - Connected route (network)
        
    - Local route (interface IP)
        
- A route matches when destination IP belongs to its network.
    
- If no route matches → packet is dropped.
    
- If multiple routes match → router selects the most specific (longest prefix).
    

---

## **16. Quiz Concepts (Key Exam Points)**

1. ![[Pasted image 20260211122428.png]]
2. ![[Pasted image 20260211122545.png]]




---

ans. 1. b,c
ans. 2. c