# Lecture 11: Transport Layer — Part 1 (Services)

**Course:** Computer Networks and Internet Protocol  
**Instructor:** Prof. Sandip Chakraborty, IIT Kharagpur

---

## Overview of This Lecture

This lecture introduces the **Transport Layer** of the TCP/IP protocol stack. It is the **second layer from the top** in the TCP/IP model. The lecture covers:

1. How devices are connected in the network (End hosts vs Intermediate devices)
2. Protocol stack implementation inside a host (Hardware, Firmware, Kernel, Application)
3. How application data passes through different layers (Headers at each layer)
4. Transport Layer Services (Reliable delivery, Connection establishment, Flow control, Congestion control, Ordered delivery)
5. Difference between TCP and UDP at a high level

---

## Concept 1: How Devices Are Connected in the Network

### 📌 Concept Name
**Network Device Connectivity and Layer Distribution**

### 🧠 Simple Explanation

In a network, there are two types of devices:

- **End Hosts** (your computer, a Google server, etc.) — These are the devices that actually send and receive data. End hosts have **all 5 layers** of the TCP/IP protocol stack: Application, Transport, Network, Data Link, and Physical.

- **Intermediate Devices** — These are devices in between (like routers and switches) that help forward data. They do **NOT** have all 5 layers.
  - **Layer 2 Switch (L2 Switch):** Has layers only up to the **Data Link layer** (Layer 2). It forwards data based on MAC addresses.
  - **Layer 3 Switch / Router:** Has layers up to the **Network layer** (Layer 3). It finds paths and forwards packets based on IP addresses.

The **Transport Layer sits on top of the Network Layer** and exists **only at the two end hosts**, not at intermediate devices. Its job is to ensure **end-to-end performance and functionality**.

### 🛠 Real-world Example (from transcript)

Think of sending data from a machine at IIT Kharagpur to a Google server in the USA. The two end machines have all 5 layers. In between, there are multiple routers (Layer 3 devices) that forward packets hop by hop. But only the two end machines run the transport layer.

### 🎯 Exam Important Points

- End hosts have all 5 layers of the TCP/IP stack.
- L2 switches have layers up to Data Link layer only.
- Routers (L3 devices) have layers up to Network layer only.
- Transport layer exists **only at end hosts** — not at intermediate devices.
- Transport layer ensures **end-to-end** functionality.

### ⚠️ Common Confusions

- Students sometimes think routers have all 5 layers — they do NOT. Routers only go up to the Network layer.
- The Transport layer is NOT present at routers or switches.

### 📝 Possible NPTEL-style Questions

- "Which layer is present only at the end hosts?" → Transport Layer
- "Up to which layer does a Layer 2 switch operate?" → Data Link Layer
- "A router operates up to which layer?" → Network Layer

---

## Concept 2: Protocol Stack Implementation Inside a Host

### 📌 Concept Name
**Protocol Stack Implementation in a Host**

### 🧠 Simple Explanation

If you look at a single computer, its networking protocol stack is implemented across three modules:

1. **Hardware (Bottom):** This contains the **Network Interface Card (NIC)**. The **entire Physical layer** is implemented in hardware. In some cases (especially wireless networks), part of the Data Link layer is also in hardware to make it faster.

2. **Firmware / Device Driver (Middle):** This is the software that talks to the NIC. It implements the **MAC layer** (which is part of the Data Link layer). When you install a driver for your network card, that driver primarily handles the MAC layer.

3. **Software / Kernel (Top):**
   - The **upper part of the Data Link layer** (called **Logical Link Control** or LLC) is in the kernel.
   - The **entire Network layer** (IP layer) is implemented in the kernel.
   - The **entire Transport layer** is implemented in the kernel.
   - In UNIX-type operating systems, the kernel contains the implementation of the Network layer and Transport layer.

4. **Applications (Topmost):** On top of the kernel, you have user applications — browser, chat apps, Facebook, YouTube, etc. These are the **Application layer**.

The **Transport layer acts as an interface** between user applications and the operating system. When data from an application goes to the OS, it passes through the transport layer.

### 🎯 Exam Important Points

- Physical layer → Implemented in **Hardware (NIC)**
- MAC layer (Data Link) → Implemented in **Firmware / Device Driver**
- Logical Link Control (upper Data Link), Network layer, Transport layer → Implemented in **Kernel (Software)**
- Application layer → Implemented as **User applications**
- Transport layer is the interface between user applications and the OS.

### ⚠️ Common Confusions

- The Data Link layer is split across TWO parts: MAC (firmware/driver) and LLC (kernel). Don't assume the whole Data Link layer is in one place.
- Physical layer is in hardware, not in software.

### 📝 Possible NPTEL-style Questions

- "Where is the Transport layer implemented?" → Kernel / OS software
- "Which layer is implemented in the Network Interface Card?" → Physical layer
- "The MAC layer is implemented in which module?" → Firmware / Device Driver

---

## Concept 3: How Application Data Passes Through Different Layers (Headers)

### 📌 Concept Name
**Data Encapsulation — Adding Headers at Each Layer**

### 🧠 Simple Explanation

When you send data (for example, from a web browser), each layer of the protocol stack adds its own **header** to the data before passing it down. This process is called **encapsulation**.

Here is how it works step by step:

1. **Application Layer:** You have HTTP data from the browser. The HTTP protocol adds an **HTTP header** on top of this data. This header contains application-level connectivity information.

2. **Transport Layer:** The entire data from the application layer (HTTP data + HTTP header) becomes the transport layer's data. The transport layer adds its own **Transport header** (TCP header or UDP header). As an application developer, you choose which transport protocol to use — TCP or UDP.

3. **Network Layer:** The transport layer data + transport header becomes the network layer's data. The network layer adds its own **IP header**.

4. **Data Link Layer:** The network layer data + IP header becomes the data link layer's data. The data link layer adds a **MAC header**.

5. **Physical Layer:** The data link layer data + MAC header reaches the physical layer. The physical layer adds a **Physical header**, and for some protocols, it also adds a **trailer** at the end to identify the end of a frame.

So at the physical layer, you can see: a small amount of original HTTP data, surrounded by multiple headers added by each layer.

### 🎯 Exam Important Points

- Each layer adds its own header.
- The order of headers (from inner to outer): HTTP header → Transport header (TCP/UDP) → IP header → MAC header → Physical header (+ sometimes a trailer).
- The application developer decides which transport protocol (TCP or UDP) to use.
- Some protocols also add a **trailer** at the physical layer to mark the end of a frame.

### ⚠️ Common Confusions

- The "data" for each layer includes ALL the headers from upper layers. For example, the Network layer's "data" is: (HTTP data + HTTP header + Transport header).
- A trailer is not added by every protocol — only some protocols add it at the physical layer.

### 📝 Possible NPTEL-style Questions

- "Which layer adds the MAC header?" → Data Link Layer
- "In what order are headers added?" → Application → Transport → Network → Data Link → Physical
- "Who decides whether TCP or UDP is used?" → The application developer

---

## Concept 4: Why Do We Need the Transport Layer?

### 📌 Concept Name
**Need for the Transport Layer — Unreliable Network Layer**

### 🧠 Simple Explanation

The layer just below the Transport layer is the **Network layer**. The Network layer's job is **datagram delivery** — it looks at the destination address in the packet and forwards it hop by hop through routers.

But the Network layer provides **unreliable datagram delivery**. This means:

- There is **no guarantee** that a packet will reach the destination.
- Packets can get **dropped** at intermediate routers.

**Why do packets get dropped?**

Intermediate routers have a **finite buffer** (a temporary storage queue). When traffic is heavy, multiple routers send data to one router. That router's buffer fills up. When the buffer is full, new incoming packets are **dropped** — this is called **buffer overflow**.

Other reasons for packet loss include: **errors during physical transmission** and **channel interference** (especially in wireless networks).

Since the Network layer cannot guarantee delivery, the **Transport layer** sits on top and its job is to:
- **Monitor** whether data reached the other end.
- If data was lost, **retransmit** it.
- Eventually ensure the application's message is fully delivered.

In simple words: The Network layer tries its best but can fail. The Transport layer fixes those failures and ensures reliable delivery.

### 🎯 Exam Important Points

- Network layer provides **unreliable datagram delivery**.
- Packets can be lost due to: **buffer overflow** at routers, **physical transmission errors**, **channel interference** (wireless).
- Transport layer provides **reliable data delivery on top of unreliable datagram delivery**.
- Transport layer monitors, detects loss, and retransmits data.
- The application should NOT be affected by packet loss in the lower layers — the transport layer handles it.

### ⚠️ Common Confusions

- "Unreliable" does not mean the network is broken — it means there is no guarantee of delivery. The network layer does a "best effort."
- Reliability is NOT a network layer responsibility — it is a transport layer responsibility.

### 📝 Possible NPTEL-style Questions

- "Why is the network layer called unreliable?" → Because it cannot guarantee that every packet will reach the destination.
- "What causes packet loss at intermediate routers?" → Buffer overflow due to finite buffer size.
- "Which layer ensures reliable data delivery?" → Transport layer.

---

## Concept 5: Transport Layer Services — The Five Key Services

### 📌 Concept Name
**Five Services of the Transport Layer**

### 🧠 Simple Explanation

The transport layer provides five main services:

---

### Service 1: Connection Establishment

**What is it?**
Before sending data, one end host sends a "hello" message to the other end to make sure the other end is alive and ready to receive.

**Analogy from transcript:** It is like making a phone call. When the other person picks up, you say "Hello." The other person says "Hello" back. Now both of you know the connection is established, and you can start talking.

In data transfer, connection establishment works the same way — one device sends a hello, the other acknowledges it, and then actual data transfer begins.

---

### Service 2: End-to-End Packet Delivery

This is the basic service — ensuring packets are delivered from one end host to the other end host. There are two approaches:

- **TCP (Transmission Control Protocol):** Provides all the services (reliability, ordering, flow control, etc.)
- **UDP (User Datagram Protocol):** Just acts like a **wrapper** over the network layer. It does minimal checking and passes data directly to the application. It does NOT provide reliability, connection establishment, or ordering.

---

### Service 3: Reliable Data Delivery

TCP ensures that if a packet is lost, it is detected and **retransmitted**. The transport layer monitors whether data reached the other end. If not, it retransmits. This way, the application always gets complete data.

UDP does NOT provide reliability.

---

### Service 4: Flow Control and Congestion Control

**Flow Control:**

Imagine a transmitter can send data at **10 Mbps**, but the receiver can only receive at **1 Mbps**. If the transmitter keeps sending at 10 Mbps, most of the extra data is wasted — it congests the network but the receiver cannot process it.

Flow control means the transmitter and receiver **communicate and agree** so that the transmitter only sends data at a rate the receiver can handle. This avoids wasting network resources.

**Congestion Control:**

Think of the network as a road traffic system. At a road junction, if traffic comes from many roads at once, the junction becomes **congested**. Similarly, in a network, if one intermediate router receives packets from many paths at once, it gets congested.

The transport layer's congestion control helps **avoid this congestion** by controlling how much data is being pumped into the network.

---

### Service 5: Ordered Packet Delivery

When data travels from one host to another, different packets may take **different paths** through the network. Because of delay differences between paths, packets may arrive **out of order**.

For example: You send packets 1, 2, 3, 4. But packet 3 takes a faster path and arrives before packet 2. So the receiver gets: 3, 2, 1, 4 (out of order).

The transport layer **reorders** these packets and delivers them to the application in the **correct sequence** (1, 2, 3, 4). This is important because applications need data in the correct order to work properly.

### 🎯 Exam Important Points (for all five services)

- Transport layer provides: Connection Establishment, End-to-End Packet Delivery, Reliable Data Delivery, Flow Control & Congestion Control, Ordered Packet Delivery.
- TCP provides **ALL** of these services.
- UDP provides **NONE** of these services — it is just a wrapper over the network layer.
- Flow control = matching transmitter speed to receiver's capacity.
- Congestion control = preventing the network from getting overloaded.
- Ordered delivery = reordering out-of-order packets before giving them to the application.

### ⚠️ Common Confusions

- Flow control and Congestion control are different things:
  - **Flow control** = receiver side limitation (receiver cannot handle the speed).
  - **Congestion control** = network side limitation (network is overloaded).
- UDP does NOT mean "no transport layer" — UDP IS a transport layer protocol, but it provides minimal services.

### 📝 Possible NPTEL-style Questions

- "Which transport layer protocol provides reliable data delivery?" → TCP
- "What is the role of flow control?" → To ensure the transmitter sends data at a rate the receiver can handle.
- "What causes out-of-order packet delivery?" → Packets taking different paths with different delays.
- "Which service reorders packets before delivering to the application?" → Ordered Packet Delivery.

---

## Concept 6: TCP vs UDP — The Two Groups of Transport Layer Protocols

### 📌 Concept Name
**TCP vs UDP**

### 🧠 Simple Explanation

The transport layer has **two broad groups** of protocols:

**Group 1: TCP (Transmission Control Protocol)**
- Provides: Connection establishment, Reliable data delivery, Flow control, Congestion control, Ordered packet delivery.
- Used when **reliability and correctness** are important.
- Examples of applications that use TCP: **HTTP** (web browsing), **FTP** (file transfer).

**Group 2: UDP (User Datagram Protocol)**
- Provides: Almost nothing extra. Just wraps the network layer data and passes it to the application.
- Does NOT provide: reliability, connection establishment, ordered delivery, flow control, congestion control.
- Used when **speed and timeliness** are more important than reliability.
- Examples of applications that use UDP: **Multimedia protocols** (like video streaming), **DNS** (Domain Name System).

### Why would anyone use UDP?

Because providing all those services (reliability, ordering, etc.) takes **extra time**. The transport layer has to do extra processing, wait for acknowledgments, retransmit lost packets — all of this introduces **delay**.

For some applications like video streaming, getting the data **quickly** is more important than getting **every single packet**. If a few video frames are lost, the multimedia protocol can approximate the missing frame by averaging the frames before and after it. But if there is too much delay, the video will buffer and the user experience will be bad.

So UDP trades reliability for speed.

### 🛠 Real-world Example (from transcript)

- When you browse a website (HTTP), you use **TCP** because you need all the web page data correctly and in order.
- When you stream video, you use **UDP** because a few dropped frames are tolerable, but delay is not.
- DNS uses **UDP** because DNS queries are small and need to be fast.

### 🎯 Exam Important Points

| Feature | TCP | UDP |
|---|---|---|
| Connection Establishment | Yes | No |
| Reliable Data Delivery | Yes | No |
| Flow Control | Yes | No |
| Congestion Control | Yes | No |
| Ordered Packet Delivery | Yes | No |
| Speed | Slower (due to extra services) | Faster (minimal processing) |
| Example Protocols | HTTP, FTP | DNS, Multimedia protocols |

- UDP can tolerate loss but not delay.
- TCP can tolerate delay but not loss.
- Multimedia protocols tolerate loss up to a certain level — they can approximate missing frames.

### ⚠️ Common Confusions

- UDP is NOT "unreliable transport layer." UDP IS a transport layer protocol — it just does not add reliability services.
- DNS uses UDP (not TCP) because DNS queries are small and need quick responses.
- HTTP uses TCP (not UDP) because web data must be complete and in order.

### 📝 Possible NPTEL-style Questions

- "Which protocol is used by HTTP at the transport layer?" → TCP
- "Which protocol is used by DNS at the transport layer?" → UDP
- "Why do multimedia applications prefer UDP?" → Because they need fast delivery and can tolerate some loss.
- "Which transport protocol does NOT provide congestion control?" → UDP

---

## Concept 7: Why UDP Is Still Useful (Multimedia Tolerance of Loss)

### 📌 Concept Name
**Loss Tolerance in Multimedia Applications**

### 🧠 Simple Explanation

Multimedia applications (like video streaming) receive data **frame by frame**. If a frame is lost, the application can do an **averaging** of the frame received before and the frame received after the lost one, and **approximate** the missing frame.

So up to a certain level of data loss, multimedia protocols can tolerate it. But if you add reliability services (like TCP does), the transport layer will spend time retransmitting the lost packet instead of sending the next new packet. This causes **more delay**, which is bad for real-time applications.

So for multimedia:
- Loss = tolerable (up to a certain level)
- Delay = NOT tolerable
- Therefore, UDP is preferred.

For file transfer and web:
- Loss = NOT tolerable (you need every byte)
- Delay = tolerable (you can wait a bit)
- Therefore, TCP is preferred.

### 🎯 Exam Important Points

- Multimedia apps approximate missing frames — they can tolerate some loss.
- TCP retransmits lost packets → introduces extra delay → bad for real-time apps.
- UDP does NOT retransmit → faster delivery → good for real-time apps.
- File transfer and web browsing need TCP because every bit of data must be correct.

---

## Summary Table of Lecture 11

| Topic | Key Point |
|---|---|
| End Hosts | Have all 5 layers of TCP/IP stack |
| L2 Switch | Has layers up to Data Link layer |
| Router (L3 device) | Has layers up to Network layer |
| Transport Layer location | Only at end hosts |
| Physical Layer | Implemented in Hardware (NIC) |
| MAC Layer | Implemented in Firmware/Device Driver |
| Network + Transport Layer | Implemented in Kernel |
| Application Layer | User applications |
| Network Layer delivery | Unreliable datagram delivery |
| Packet loss reasons | Buffer overflow, physical errors, wireless interference |
| Transport Layer job | Reliable delivery over unreliable network layer |
| TCP services | Connection establishment, reliability, flow control, congestion control, ordered delivery |
| UDP services | Almost none — just a wrapper |
| Flow Control | Match sender speed to receiver capacity |
| Congestion Control | Prevent network overload |
| Ordered Delivery | Reorder out-of-order packets |
| HTTP, FTP use | TCP |
| DNS, Multimedia use | UDP |

---

## 10 MCQs — Strictly from Lecture 11

---

### Q1. Which layer of the TCP/IP protocol stack exists ONLY at the end hosts?

A) Network Layer  
B) Data Link Layer  
C) Transport Layer  
D) Physical Layer  

**Answer: C) Transport Layer**

**Explanation:** The transcript clearly states that the transport layer sits on top of the network layer and is present only at the two end hosts, not at intermediate devices like routers or switches.

---

### Q2. A Layer 2 (L2) switch has protocol stack layers up to which layer?

A) Physical Layer  
B) Data Link Layer  
C) Network Layer  
D) Transport Layer  

**Answer: B) Data Link Layer**

**Explanation:** The transcript says a Layer 2 switch has the protocol stack up to the second layer, which is the Data Link layer.

---

### Q3. Where is the Physical Layer of the protocol stack implemented in a host?

A) Kernel  
B) Firmware  
C) Hardware (NIC)  
D) Application software  

**Answer: C) Hardware (NIC)**

**Explanation:** The transcript states that the entire Physical Layer is implemented as a part of the hardware — specifically, the Network Interface Card (NIC).

---

### Q4. The Network Layer provides which type of data delivery?

A) Reliable datagram delivery  
B) Unreliable datagram delivery  
C) Ordered packet delivery  
D) Connection-oriented delivery  

**Answer: B) Unreliable datagram delivery**

**Explanation:** The transcript explicitly says the network layer provides unreliable datagram delivery because packets can get dropped at intermediate routers due to buffer overflow, physical errors, or channel interference.

---

### Q5. What is the main cause of packet loss at intermediate routers?

A) Transport layer timeout  
B) Application crash  
C) Buffer overflow  
D) DNS failure  

**Answer: C) Buffer overflow**

**Explanation:** The transcript explains that intermediate routers have finite buffer space. When the buffer becomes full due to heavy network load, packets start dropping — this is buffer overflow.

---

### Q6. Which of the following services is NOT provided by UDP?

A) End-to-end delivery  
B) Reliable data delivery  
C) Passing data to application  
D) Wrapping network layer data  

**Answer: B) Reliable data delivery**

**Explanation:** The transcript states that UDP does not provide reliability, connection establishment, ordered delivery, or flow/congestion control. It just wraps the network layer data and passes it to the application.

---

### Q7. Flow control in the transport layer ensures that:

A) The network does not get congested  
B) Packets are delivered in order  
C) The transmitter only sends data at a rate the receiver can handle  
D) A connection is established before sending data  

**Answer: C) The transmitter only sends data at a rate the receiver can handle**

**Explanation:** The transcript gives the example where a transmitter sends at 10 Mbps but the receiver can only handle 1 Mbps. Flow control makes them agree so the transmitter sends only what the receiver can receive.

---

### Q8. Which application layer protocol uses UDP at the transport layer?

A) HTTP  
B) FTP  
C) DNS  
D) None of the above  

**Answer: C) DNS**

**Explanation:** The transcript explicitly states that DNS at the application layer uses UDP type of protocol, while HTTP and FTP use TCP.

---

### Q9. Why do multimedia applications prefer UDP over TCP?

A) They need all data to be complete and in order  
B) They need fast delivery and can tolerate some data loss  
C) They require connection establishment  
D) They need congestion control  

**Answer: B) They need fast delivery and can tolerate some data loss**

**Explanation:** The transcript explains that multimedia protocols can tolerate loss up to a certain level by approximating missing frames. Getting data quickly is more important than getting every packet. TCP's reliability services add delay, which is bad for real-time multimedia.

---

### Q10. In the TCP/IP model, the Transport Layer and Network Layer are implemented in which part of the host system?

A) Hardware (NIC)  
B) Firmware / Device Driver  
C) Kernel (OS software)  
D) User Applications  

**Answer: C) Kernel (OS software)**

**Explanation:** The transcript states that in a UNIX-type operating system, the kernel implements the higher part of the Data Link layer (LLC), the entire Network layer, and the entire Transport layer.

---

*End of Lecture 11 Notes — All content strictly from the transcript.*
