## 1. Networking Models

Networking models categorize and structure networking protocols and standards.

- Define logical rules for device communication
    
- Ensure interoperability between devices from different vendors
    
- Two major models:
    
    - **OSI Model** (conceptual, 7 layers)
        
    - **TCP/IP Suite** (practical, 4 layers)
        

---

## 2. OSI Model Overview

**OSI = Open Systems Interconnection**

- Created by ISO (1970s–1980s)
    
- Conceptual, not used in real networks but essential for understanding
    
- Network functions divided into **7 layers**
    

### OSI Layers Summary Table

| Layer Number | Name         | PDU     | Key Functions                                          | Devices      |
| ------------ | ------------ | ------- | ------------------------------------------------------ | ------------ |
| 7            | Application  | Data    | Interfaces with applications                           | —            |
| 6            | Presentation | Data    | Translation, encryption/decryption                     | —            |
| 5            | Session      | Data    | Session establishment, maintenance, termination        | —            |
| 4            | Transport    | Segment | Segmentation, reliability, end-to-end communication    | —            |
| 3            | Network      | Packet  | IP addressing, routing, path selection                 | Router       |
| 2            | Data Link    | Frame   | Node-to-node delivery, MAC addressing, error detection | Switch       |
| 1            | Physical     | Bits    | Signals, media, connectors, voltages                   | Cables, NICs |

---

## 3. OSI Layer Details

### **Layer 7: Application Layer**

- Closest to end users
    
- Interacts with apps like browsers (Firefox, Brave, etc.)
    
- Examples: **HTTP, HTTPS**
    
- Handles:
    
    - Identifying communication partners
        
    - Synchronizing communication
        
- Does NOT include the applications themselves, only protocols
    

### **Layer 6: Presentation Layer**

- Translates between application format and network format
    
- Handles encryption/decryption
    
- Ensures receiver can understand data format
    

### **Layer 5: Session Layer**

- Manages sessions between hosts
    
- Establishes, maintains, and terminates communication sessions
    
- Important for multi-user apps (e.g., YouTube server handling many clients)
    

### **Layer 4: Transport Layer**

- Provides **end-to-end, host-to-host** communication
    
- Segmentation and reassembly
    
- Adds **Layer 4 header** to create **segments**
    
- Ensures reliable/ordered delivery when needed
    

### **Layer 3: Network Layer**

- Logical addressing (**IP addresses**)
    
- Routing and path selection
    
- Routers operate here
    
- Adds **Layer 3 header** (source & destination IP) → creates **packet**
    

### **Layer 2: Data Link Layer**

- Node-to-node delivery
    
- Formats data for transmission over physical media
    
- Uses **MAC addresses**
    
- Adds **Layer 2 header + trailer** → creates **frame**
    
- Switches operate here
    
- Handles error detection
    

### **Layer 1: Physical Layer**

- Electrical/optical/radio signal transmission
    
- Cables, connectors, voltage levels, max distances
    
- Converts bits to signals
    

---

## 4. Encapsulation Process

### Encapsulation steps (sending):

1. **Data** created at Layers 7–5
    
2. Layer 4 adds Header → **Segment**
    
3. Layer 3 adds Header → **Packet**
    
4. Layer 2 adds Header + Trailer → **Frame**
    
5. Layer 1 transmits **Bits**
    

### De-encapsulation (receiving):

- Reverse process from Layer 1 up to Layer 7
    

### PDU Summary Table

|OSI Layer|PDU Name|
|---|---|
|7–5|Data|
|4|Segment|
|3|Packet|
|2|Frame|
|1|Bits|

---

## 5. TCP/IP Suite Overview

- Actual model used in modern networks
    
- Created by U.S. DoD via DARPA
    
- Simpler than OSI: **4 layers**
    

### TCP/IP Layer Comparison Table

|TCP/IP Layer|Equivalent OSI Layers|Description|
|---|---|---|
|Application|7, 6, 5|Protocols for user apps (HTTP, DNS)|
|Transport|4|TCP/UDP|
|Internet|3|IP addressing + routing|
|Link|2 & 1|MAC addressing, physical transmission|

### Notes

- Network engineers still use OSI terminology when discussing issues
    
    - Example: “Layer 2 problem” always refers to OSI Data Link layer


---

## 6. Example: Communication Across Routers

- Host A sends data to Host B
    
- Application generates data (Skype example)
    
- Data encapsulated via Transport → Internet → Link
    
- Sent to Router 1
    
- Router strips Link header/trailer, checks IP header (Layer 3)
    
- Re-encapsulates for next link
    
- Sent to Router 2, same process
    
- Finally delivered to Host B
    
- Host B de-encapsulates and hands data to application
    

### Key ideas:

- Routers only care about **Layer 3 (IP)**
    
- Switches only care about **Layer 2 (MAC)**
    
- Host-to-host communication handled by **Transport layer**
    
- Same-layer interaction allows communication between identical layers across devices
    

---

## 7. Quiz Answers (for revision)

### Q1: HTTP displayed in browser

- **Same-layer interaction (Layer 7)**
    

### Q2: HTTP data + 3 headers + 1 trailer

- **Frame (Layer 2 PDU)**
    

### Q3: Layers relevant to network engineers

- **Transport, Network, Data Link, Physical**
    

### Q4: TCP/IP Link Layer = OSI?

- **Data Link + Physical**
    

### Q5: Layer that provides host-to-host communication?

- **Transport Layer**
