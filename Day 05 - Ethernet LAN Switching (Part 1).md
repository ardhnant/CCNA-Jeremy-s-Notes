## **1. Where This Lesson Fits (Big Picture)**

- This lesson focuses on **Layer 2 (Data Link Layer)** behavior.
    
- Specifically: **Ethernet LAN switching**.
    
- Ethernet operates at:
    
    - **Layer 1**: Physical (signals, cables)
        
    - **Layer 2**: Frames, MAC addresses, switching logic
        

What’s _not_ covered yet:

- Routing between LANs (Layer 3). That’s later.
    

---

## **2. LAN (Local Area Network) Basics**

### What is a LAN?

- A network within a **small geographic area**:
    
    - Home
        
    - Office floor
        
    - Building
        

### Key rule (VERY IMPORTANT):

- **Switches do NOT separate LANs**
    
- **Routers DO separate LANs**
    

### Examples:

- Multiple switches connected together → **still ONE LAN**
    
- Same switches connected to **different router interfaces** → **SEPARATE LANs**
    

So:

- Switch = expands a LAN
    
- Router = separates LANs
    

---

## **3. OSI Layer Review (Relevant Parts Only)**

### **Layer 1 – Physical**

- Voltage levels
    
- Cables
    
- Connectors
    
- Distance limits
    
- Electrical or radio signals
    

### **Layer 2 – Data Link**

- Node-to-node communication
    
- Error detection (sometimes correction)
    
- Frame formatting
    
- **MAC addressing**
    
- Switches operate here
    

---

## **4. Encapsulation (How Data Becomes a Frame)**

|OSI Layer|Name of Data Unit|
|---|---|
|Layer 7–5|Data|
|Layer 4|Segment|
|Layer 3|Packet|
|Layer 2|Frame|

This lesson focuses on:

- **Layer 2 frames**
    
- How switches receive and forward them
    

---

## **5. Ethernet Frame Structure**

![Image](https://www.ionos.ca/digitalguide/fileadmin/DigitalGuide/Screenshots_2018/EN-ethernet-frame-structure6.jpg)

![Image](https://media.licdn.com/dms/image/v2/D4D12AQH2HFTcRbWY-g/article-inline_image-shrink_400_744/article-inline_image-shrink_400_744/0/1706622908748?e=2147483647&t=bhdu9QOnwW0XDNH-Vpii-T9RyMbBdbXAuHvGN4CsGLM&v=beta)

### Ethernet Frame = Header + Data + Trailer

### **Ethernet Header Fields**

|Field|Size|Purpose|
|---|---|---|
|Preamble|7 bytes|Clock synchronization|
|SFD (Start Frame Delimiter)|1 byte|Marks start of frame|
|Destination MAC|6 bytes|Receiver|
|Source MAC|6 bytes|Sender|
|Type / Length|2 bytes|L3 protocol OR payload length|

### **Ethernet Trailer**

|Field|Size|Purpose|
|---|---|---|
|FCS|4 bytes|Error detection (CRC)|

### Total Ethernet Overhead

- **26 bytes** (Header + Trailer)
    

---

## **6. Preamble & SFD (Synchronization Stuff)**

### Preamble

- 7 bytes = **56 bits**
    
- Pattern: `10101010` repeated
    
- Purpose:
    
    - Synchronizes receiver’s clock
        
    - Gets device ready to read frame
        

### SFD (Start Frame Delimiter)

- 1 byte
    
- Pattern: `10101011`
    
- Signals:
    
    - End of preamble
        
    - Start of actual frame data
        

---

## **7. MAC Addresses (Layer 2 Addressing)**

- MAC = **Media Access Control**
    
- Length:
    
    - **48 bits**
        
    - **6 bytes**
        
- Assigned at manufacturing time
    
- Also called:
    
    - **Burned-In Address (BIA)**
        

### MAC Address Format

- 12 hexadecimal characters
    
- Example:
    
    - `E8BA.7011.2874`
        

### Structure of MAC Address

|Portion|Size|Meaning|
|---|---|---|
|OUI|First 24 bits|Manufacturer|
|Device ID|Last 24 bits|Unique device|

---

## **8. Hexadecimal (Enough to Survive CCNA)**

- Hex uses **16 symbols**:
    
    - `0–9` and `A–F`
        
- Values:
    
    - A = 10
        
    - B = 11
        
    - …
        
    - F = 15
        

### Why hex?

- Compact way to represent binary
    
- Used heavily in:
    
    - MAC addresses
        
    - IPv6
        
    - Ethernet Type field
        

---

## **9. Ethernet Type / Length Field**

| Value Range | Meaning                |
| ----------- | ---------------------- |
| ≤ 1500      | Payload length (bytes) |
| ≥ 1536<br>  | Protocol type          |


> Minimum size of packet (header + payload(46) + trailer) is 64 byte
> Maximum size of packet (header + payload(max payload 1500) + trailer) is 1520 bytes

### Common Type Values

|Hex|Decimal|Protocol|
|---|---|---|
|0x0800|2048|IPv4|
|0x86DD|34525|IPv6|

---

## **10. FCS (Frame Check Sequence)**

- Size: **4 bytes**
    
- Uses:
    
    - **CRC (Cyclic Redundancy Check)**
        
- Purpose:
    
    - Detects corrupted frames
        
- If CRC fails:
    
    - Frame is discarded
        

No correction. Just detect and drop.

---

## **11. Switch MAC Address Table**

- Stores:
    
    - MAC address
        
    - Associated interface
        
- Built **dynamically**
    
- Uses:
    
    - **Source MAC address only**
        

---

## **12. MAC Learning Process (Critical Concept)**

![Image](https://networklessons.com/wp-content/uploads/2013/02/switch-learns-mac-address.png)

![Image](https://www.cisco.com/c/dam/en/us/support/docs/switches/catalyst-6000-series-switches/23563-143-00.gif)

### Rule:

> **Switch learns MAC addresses from the SOURCE MAC field**

### Example:

1. Frame arrives on F0/1
    
2. Source MAC = AAAA.AA00.0001
    
3. Switch records:
    
    - MAC → F0/1
        

---

## **13. Unknown Unicast Frames**

### Definition:

- Destination MAC **not in MAC table**
    

### Switch behavior:

- **Flood**
    
- Sends frame out:
    
    - All ports
        
    - Except the incoming port
        

---

## **14. Known Unicast Frames**

### Definition:

- Destination MAC **exists in MAC table**
    

### Switch behavior:

- Forwards frame:
    
    - Only out the correct interface
        
- No flooding
    

---

## **15. Why Flooding Stops Eventually**

- When destination replies:
    
    - Switch learns destination MAC
        
- After learning:
    
    - Traffic becomes known unicast
        
    - Flooding stops
        

---

## **16. Multiple Switch Scenario**

Key points:

- Each switch builds **its own MAC table**
    
- MAC entries point to:
    
    - Interface to reach the MAC
        
    - Not necessarily directly connected host
        

---

## **17. MAC Address Aging**

- On Cisco switches:
    
    - Dynamic MAC entries expire after **5 minutes**
        
- If traffic resumes:
    
    - MAC is relearned automatically
        

---

# **Quiz Questions and Answers**

## **1. Which Ethernet field synchronizes the receiver clock?**

**Reasoning:**  
Synchronization is needed before frame decoding begins.

---

## **2. What is the length of a MAC address?**

**Reasoning:**  
MAC addresses are fixed-length physical addresses.

---

## **3. What is the OUI of MAC address E8BA.7011.2874?**

**Reasoning:**  
The OUI is always the first half of the MAC address.

---

## **4. Which Ethernet field does a switch use to build its MAC table?**

**Reasoning:**  
Switches learn where devices are by observing incoming frames.

---

## **5. Which type of frame is flooded by a switch?**

**Reasoning:**  
Flooding happens only when the destination is unknown.

---

## **Answers**

1. **Answer:** **A. Preamble**
    
2. **Answer:** **D. 48 bits**
    
3. **Answer:** **B. E8BA.70**
    
4. **Answer:** **C. Source MAC Address**
    
5. **Answer:** **A. Unknown unicast**
    

---