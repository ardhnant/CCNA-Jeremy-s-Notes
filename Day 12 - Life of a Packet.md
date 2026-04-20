## **1. Purpose of This Lesson — The Life of a Packet**

This lesson explains the **complete process a packet goes through** when traveling from one network to another.

### **Key Goals**

- Understand the **end-to-end process** of packet forwarding.
    
- Combine previously learned concepts into one flow.
    
- Review:
    
    - ARP (Address Resolution Protocol)
        
    - Encapsulation
        
    - De-encapsulation
        
    - Routing decisions
        
    - MAC address usage
        
- Focus level is **CCNA depth**, not CCNP or CCIE detail.
    

This lesson is conceptual rather than configuration-based.

---

## **2. Network Scenario Used**

![[Pasted image 20260212114018.png]]

---

## **3. MAC Address Assumptions**

For simplicity, shortened MAC addresses are used.

| Device | Interface | MAC Address |
| ------ | --------- | ----------- |
| PC1    | NIC       | 1111        |
| R1     | G0/2      | AAAA        |
| R1     | G0/0      | BBBB        |
| R2     | G0/0      | CCCC        |
| R2     | G0/1      | DDDD        |
| R4     | G0/1      | EEEE        |
| R4     | G0/2      | FFFE        |
| PC4    | NIC       | 4444        |

### **Important Concepts**

- Each interface has its **own unique MAC address**.
    
- Broadcast MAC address = `FFFF.FFFF.FFFF`.
    
- Switch MAC addresses exist but are not relevant here.
    

---

## **4. Initial Packet Creation (PC1 → PC4)**

PC1 creates an IP packet:

| Field          | Value       |
| -------------- | ----------- |
| Source IP      | 192.168.1.1 |
| Destination IP | 192.168.4.1 |

### **Key Decision**

PC1 determines:

- Destination is in a **different network**.
    
- Packet must be sent to the **default gateway (R1)**.
    

---

## **5. ARP Process — PC1 Learns R1 MAC Address**


![[Pasted image 20260212114018.png]]


Since PC1 has not communicated before:

### **ARP Request**

PC1 sends a broadcast frame:

- Source IP → PC1
    
- Destination IP → R1 (default gateway)
    
- Source MAC → 1111
    
- Destination MAC → FFFF.FFFF.FFFF (broadcast)
    

Meaning:

> “Who has 192.168.1.254? Tell me your MAC address.”

### **Switch Behavior**

- SW1 floods the broadcast frame.
    
- SW1 learns PC1’s MAC on incoming interface.
    

### **ARP Reply (R1 → PC1)**

R1 responds unicast:

- Source MAC → AAAA
    
- Destination MAC → 1111
    

Meaning:

> “I am 192.168.1.254. My MAC is AAAA.”

SW1 learns R1’s MAC from this frame.

---

## **6. Encapsulation and Forwarding to R1**

PC1 encapsulates packet:

| Layer           | Address Used    |
| --------------- | --------------- |
| IP Destination  | PC4 (unchanged) |
| MAC Destination | R1              |

### **Important Rule**

- IP header is **not changed**.
    
- Only Layer 2 information changes.
    

---

## **7. Processing at R1**

### **Step 1 — De-encapsulation**

R1 removes Ethernet header.

### **Step 2 — Routing Decision**

Routing table lookup:

```
192.168.4.0/24 → next hop 192.168.12.2 (R2)
```

### **Step 3 — ARP for Next Hop**

R1 does not know R2’s MAC.

#### ARP Request

- Broadcast MAC used.
    
- Source MAC → BBBB
    

Meaning:

> “Who has 192.168.12.2?”

#### ARP Reply

R2 replies:

- MAC address → CCCC
    

### **Step 4 — Re-encapsulation**

New Ethernet header:

| Field           | Value |
| --------------- | ----- |
| Source MAC      | BBBB  |
| Destination MAC | CCCC  |

Packet sent to R2.

---

## **8. Processing at R2**

### **Step 1 — De-encapsulation**

Ethernet header removed.

### **Step 2 — Routing Decision**

Routing entry:

```
192.168.4.0/24 → next hop 192.168.24.4 (R4)
```

### **Step 3 — ARP for R4**

R2 does not know R4’s MAC.

ARP Request:

> “Who has 192.168.24.4?”

ARP Reply:

- MAC address → EEEE
    

### **Step 4 — Re-encapsulation**

New Ethernet header:

| Field           | Value |
| --------------- | ----- |
| Source MAC      | DDDD  |
| Destination MAC | EEEE  |

Packet sent to R4.

---

## **9. Processing at R4**

### **Step 1 — De-encapsulation**

Ethernet header removed.

### **Step 2 — Routing Decision**

Destination network is directly connected:

```
192.168.4.0/24 via G0/2
```

### **Step 3 — ARP for PC4**

R4 does not know PC4’s MAC.

ARP Request:

> “Who has 192.168.4.1?”

PC4 replies:

- MAC address → 4444
    

Switch learns MAC addresses during this exchange.

### **Step 4 — Final Encapsulation**

Ethernet header:

|Field|Value|
|---|---|
|Source MAC|FFFE|
|Destination MAC|4444|

Packet delivered to PC4.

---

## **10. Key Observations from Entire Process**

### **IP Header Behavior**

- Source IP never changes.
    
- Destination IP never changes.
    
- Original packet remains the same end-to-end.
    

### **MAC Address Behavior**

- Source and destination MAC change at every hop.
    
- New Ethernet header created by each router.
    

### **Switch Behavior**

- Switches forward frames.
    
- Switches learn MAC addresses.
    
- Switches do NOT:
    
    - de-encapsulate packets
        
    - modify Ethernet headers
        

---

## **11. Return Traffic (PC4 → PC1)**

Major difference:

- ARP process already completed.
    
- Devices already know MAC addresses.
    

Result:

- No ARP requests needed.
    
- Routers simply de-encapsulate and re-encapsulate frames as they forward traffic.
    

---

## Overview

![[Pasted image 20260212114018.png]]

1. Static routes already exist on R1, R2, R4.  
    ✔ Correct. So routers already know _where_ to send packets at Layer 3.
    
2. PC1 wants to ping PC4.  
    ✔ Correct.
    
3. PC1 ARPs for the default gateway (R1).  
    ✔ Correct.
    
4. R1 replies with its MAC address.  
    ✔ Correct.
    
5. PC1 sends the packet to R1.  
    ✔ Correct.
    
6. R1 ARPs for R2, learns MAC, sends packet.  
    ✔ Correct.
    
7. R2 ARPs for R4, learns MAC, sends packet.  
    ✔ Correct.
    
8. R4 ARPs for PC4 before sending to it.  
    ✔ Correct

```
PC1 ARP → send packet
       ↓
R1 ARP → send packet
       ↓
R2 ARP → send packet
       ↓
R4 ARP → send packet
```