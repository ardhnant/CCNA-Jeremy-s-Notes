# **What Is Networking?**

A **computer network** is a digital communication system that allows **nodes** (devices) to share resources and exchange data.

> **Node:** Any device connected to a network that can send or receive data.

---

# **Types of Network Devices**

## **1. Router**

**Image:**  
![[Pasted image 20251116183340.png]]

**Purpose:**

- Connects multiple LANs.
- Sends data between networks and out to the internet.

**Key Points:**

- Has fewer ports compared to switches.
- Works at the network layer (Layer 3).
- Makes forwarding decisions based on IP addresses.

**Examples:**

- Cisco ISR 1000
- ASR 1000-X Router
- Cisco ISR 4000
- Cisco ISR 900 Series

---

## **2. Switch**

**Image:**  
![[Pasted image 20251116183750.png]]

**Purpose:**

- Forwards traffic **within** a LAN.
- Connects end hosts like PCs, printers, servers.

**Key Points:**

- Comes with many Ethernet ports.
- Works at the data link layer (Layer 2), some at Layer 3.
- Does **not** connect LANs to the internet.

**Examples:**

- Cisco Catalyst 9200
- Cisco Catalyst 3650 Series

---

## **3. Firewall**

**Image:**  
![[Pasted image 20251116184039.png]]

**Purpose:**

- Secures the network by controlling incoming and outgoing traffic.
- Allows or blocks traffic based on security rules.

**Placement:**

- **Outside:** Before the router to protect the whole network.
- **Inside:** After the router to secure internal segments.

### **Types of Firewalls**

- **Network Firewalls:**  
    Hardware devices that filter traffic between networks.
- **Host-Based Firewalls:**  
    Software on a single device (like Windows Firewall).
- **Next-Generation Firewalls (NGFW):**  
    Include advanced features such as IPS (Intrusion Prevention System).

**Examples:**

- Cisco ASA 5500-X Series
- Cisco Firepower 2100 Series

---

## **4. Server**

**Image:**  
![[Pasted image 20251116185032.png]]

**Purpose:**

- Provides services, data, storage, or applications to clients.

**Examples:**

- IBM Server
- Dell Server

---

## **5. Client**

**Image:**  
![[Pasted image 20251116185222.png]]

**Purpose:**

- A device that requests and uses services from a server.

**Notes:**

- Clients and servers are also called **end hosts** or **endpoints**.

---
# **Quiz Questions and Answers**

## **1. Company needs hardware to connect 30 PCs in a department. Which device?**

**Reasoning:**  
A switch connects multiple end hosts in the same LAN and provides many ports.  
Routers connect networks, firewalls filter traffic, and servers provide services rather than interconnecting devices.

---

## **2. Your friend's iPhone sends you a video via AirDrop. What was his iPhone functioning as?**

**Reasoning:**  
A server provides a service. His phone provided the video file.  
Your device requested and received it, so it acted as the client.

---

## **3. What is your computer/smartphone functioning as while watching a video online?**

**Reasoning:**  
Your device is receiving a service (the video stream) from YouTube’s servers.

---

## **4. Company wants hardware to connect its separate networks together. What device?**

**Reasoning:**  
Routers are designed to connect, route, and forward traffic between different networks.

---

## **5. Company wants to upgrade an old network firewall to one with advanced functions. What kind?**

**Reasoning:**  
NGFWs include advanced features like IPS.  
Host-based firewalls are software, and "next-level" or "top-layer" aren't real types.

---
---
# Answers
1. **Answer:** **C. Switch**  
2. **Answer:** **A. Server**  
3. **Answer:** **C. Client**  
4. **Answer:** **D. Router**  
5. **Answer:** **C. Next-Generation Firewall**  





















































