# **1. What Are Interfaces and Cables?**

- Devices need some way to connect to each other.  
- That connection is usually through **cables** (wired network).
- Wireless comes later, but CCNA starts with **wired** because it’s the foundation.


---

# **2. Switch Ports (Interfaces)**

- A **switch** is like a multi-socket extension board for computers.    
- You plug PCs, servers, printers, whatever into it so they can talk.

Example:  
A switch with **24 ports** means you can connect 24 devices.

Those labels like **10/100/1000Base-T** just tell you what speeds the port supports.

---

# **3. RJ-45 Ports & Connectors**

- The square network port you see everywhere?  
    That’s the **RJ-45 port**.    
- The plastic plug at the end of the cable?  
    That’s the **RJ-45 connector**.

You shove the connector into the port, internet magic happens, everyone’s happy.

![[Pasted image 20260204055738.png]]
![[Pasted image 20260204055556.png]]

---

# **4. What Is Ethernet?**

- Ethernet is basically the **rules and standards** for wired networking.    
- It decides:
    - what cables we use
    - how fast data travels
    - how devices talk

Think of it like traffic rules for network cables.

---

# **5. Why Do We Need Standards?**

Simple:  
If every company built their own cable size, nothing would fit anywhere.

Standards make sure:

- your cable fits my switch    
- my router understands your signals
- everything behaves like a decent citizen

---

# **6. Bits and Bytes (Speed Stuff)**

- **Bit = 0 or 1.**    
- **Byte = 8 bits.**
- Network speed = measured in **bits** (like Mbps, Gbps).
- Storage = measured in **bytes** (like MB, GB).

So:

- **1 GB file = 8 Gb of network data.**

Your 1 GB file isn’t teleporting through the cable; it’s being chopped into billions of tiny bits.

---

# **7. Ethernet Cable Types (Copper)**

Copper cables = the normal plastic cables you see everywhere.

### Common Types (Just remember these)

| Speed    | Name         | AKA        | Max Length | Official IEEE Standard |
| -------- | ------------ | ---------- | ---------- | ---------------------- |
| 10 Mbps  | Ethernet     | 10BASE-T   | 100 m      | 802.3i                 |
| 100 Mbps | FastEthernet | 100BASE-T  | 100 m      | 802.3u                 |
| 1 Gbps   | Gigabit      | 1000BASE-T | 100 m      | 802.3ab                |
| 10 Gbps  | 10-Gig       | 10GBASE-T  | 100 m      | 802.3an                |

All these cables are **UTP** (Unshielded Twisted Pair) cables.

---

# **8. UTP Cables (What’s Inside?)**

Inside the cable:

- 4 twisted pairs (total 8 wires)
- twisting = reduces interference
- RJ-45 connector has 8 pins for these 8 wires

Lower-speed Ethernet (10/100 Mbps) uses **4 wires**.  
Higher-speed (1G/10G) uses **all 8 wires**.

---

# **9. How Devices Send & Receive Signals**

Old devices had fixed pins:

| Device                 | Transmit (Tx) | Receive (Rx) |
| ---------------------- | ------------- | ------------ |
| PC / Router / Firewall | Pins 1 & 2    | Pins 3 & 6   |
| Switch                 | Pins 3 & 6    | Pins 1 & 2   |

Meaning:

- PC → Switch works normally (they are opposite).    
- PC → PC or Switch → Switch doesn’t work unless cable flips the pins.

This is where straight-through vs crossover comes in.

---

# **10. Straight-Through Cable**

Use for **different** devices:

- PC ↔ Switch    
- Router ↔ Switch

Pins are 1→1, 2→2, etc.

---

# **11. Crossover Cable**

Use for **same type** devices:

- PC ↔ PC
- Switch ↔ Switch
- Router ↔ Router

Pins are crossed:

- 1 ↔ 3    
- 2 ↔ 6

---

# **12. Auto MDI-X**

Modern devices don’t care about cable type.  
They auto-adjust the pins.

So even if you plug in the “wrong” cable, it still works.

---

# **13. Fiber-Optic Cables**

When copper can’t go far enough (max 100 m), fiber steps in.

Fiber uses **light**, not electricity.  
Two connectors:

- Tx (send light)
- Rx (receive light)    

![[Pasted image 20260204060249.png]]

---

# **14. Fiber Types**

### **1. Multimode Fiber (MMF)**

- Cheaper
- For medium distances (hundreds of meters)
- Uses LED light
- Core diameter is wider than single-mode fiber.
- Allows multiple angles (modes) of light waves to enter the fiberglass core.

![[Pasted image 20260204060359.png]]

### **2. Single-Mode Fiber (SMF)**

- Expensive
- For long distances (kilometers)
- Uses lasers
- Core diameter is narrower than mutimode fiber.
- Light enters at a single angle (mode) from a laser-based transmitter.

![[Pasted image 20260204060442.png]]

### **3. Small Form-Factor Pluggable Transceiver (SFP)**

-  Ports on which MMF and SMF are plugged in.

![[Pasted image 20260204060531.png]]

---

# **15. Fiber Ethernet Standards**

| Speed   | Name        | Fiber   | Max Distance | Official IEEE Standard |
| ------- | ----------- | ------- | ------------ | ---------------------- |
| 1 Gbps  | 1000BASE-LX | MMF/SMF | 550 m / 5 km | 802.3z                 |
| 10 Gbps | 10GBASE-SR  | MMF     | 400 m        | 802.3ae                |
| 10 Gbps | 10GBASE-LR  | SMF     | 10 km        | 802.3ae                |
| 10 Gbps | 10GBASE-ER  | SMF     | 30 km        | 802.3ae                |
![[Pasted image 20260305020208.png]]

---

# **16. UTP vs Fiber Quick Comparison**

| Feature      | UTP      | Fiber     |
| ------------ | -------- | --------- |
| Cost         | Cheap    | Expensive |
| Max Distance | 100 m    | Many km   |
| EMI          | Affected | Immune    |
| Security     | Weak     | Strong    |
| Ports        | RJ45     | SFP       |

---

# **17. Quiz (Simplified)**

### **Q1. Two old routers connected with UTP but not working?**

Straight-through cable used. They need crossover.

### **Q2. Buildings 150m apart, cheap connection?**

Multimode fiber.

### **Q3. Offices 3 km apart?**

Single-mode fiber.

### **Q4. Two switches with Auto MDI-X & straight-through cable.**

Works normally.

### **Q5. Connect many PCs on same floor.**

UTP.
