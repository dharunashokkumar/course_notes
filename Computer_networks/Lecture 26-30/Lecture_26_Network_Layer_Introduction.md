# Lecture 26 – Network Layer I: Introduction

## Course: Computer Networks and Internet Protocol (NPTEL)
### Prof. Sandip Chakraborty, IIT Kharagpur

---

## Overview of This Lecture

This lecture marks the beginning of the **Network Layer (Layer 3)** of the TCP/IP protocol stack. After studying the Application Layer and Transport Layer, we now move to the layer responsible for delivering packets from one end host to another across multiple hops. The lecture covers what the network layer does, why it is needed, how the internet is organized hierarchically, what autonomous systems are, and how addressing works at a high level.

---

## Concept 1: Transition from Transport Layer to Network Layer

### 🧠 Simple Explanation

So far in the course, you studied two layers from the top of the TCP/IP protocol stack: the **Application Layer** (HTTP, DNS, FTP, etc.) and the **Transport Layer** (TCP, UDP). Now, we move to the **third layer from the top** — the **Network Layer**, also called the **Internet Layer**.

Think of it this way: the Application Layer decides *what* data to send (like a web page request). The Transport Layer decides *how reliably* to send it (TCP ensures no loss). Now the Network Layer decides **where and through which path** to send it across the internet.

### 🎯 Exam Important Points

- Network Layer = Layer 3 = Internet Layer (these are all the same thing in this course).
- It sits in the **middle** of the TCP/IP protocol stack.
- It comes **after** Application and Transport layers (when counting top-to-bottom).

### ⚠️ Common Confusions

- Students sometimes confuse "Internet Layer" with "Application Layer" (because the internet is accessed via applications). Remember: Internet Layer = Network Layer = Layer 3. It deals with packet routing, not with web browsers or apps.

---

## Concept 2: Broad Objective of the Network Layer

### 🧠 Simple Explanation

The main job of the network layer is simple to understand: **deliver the data packet from the source host to the destination host**, possibly across many intermediate devices (routers).

Imagine you are in Kharagpur and you want to send a letter to someone in the USA. The postal system has to figure out the route — from your local post office, to the regional office, to the national office, across the ocean, and then down to the destination. The network layer does the same thing for data packets.

However, there is an important catch: the network layer **does not guarantee** that every packet will be delivered. In a packet-switching network, routers have limited buffer space. If the buffer is full, packets get **dropped**. So the network layer only provides a **"best effort"** service — it tries its best, but cannot promise 100% delivery.

The reliability (making sure lost packets are retransmitted) is handled by the **Transport Layer** (TCP), which sits above the network layer.

### 🎯 Exam Important Points

- Network layer provides **unreliable datagram delivery**.
- This means it is a **best effort service** — it tries to deliver but cannot guarantee.
- Packets can be **dropped** at intermediate routers due to full buffers.
- **Reliability** is NOT the network layer's job — that is handled by TCP at the transport layer.

### ⚠️ Common Confusions

- "Unreliable" does not mean "bad." It simply means the network layer does not have a mechanism to retransmit lost packets. The word "unreliable" is a technical term here, not a quality judgment.

---

## Concept 3: The Datagram — Unit of Data at Network Layer

### 🧠 Simple Explanation

Every layer of the protocol stack has its own name for the unit of data it handles. At the application layer we call it a "message." At the transport layer, we call it a "segment." At the network layer, we call it a **datagram**.

So when the transport layer hands data down to the network layer, the network layer wraps it into a datagram and tries to deliver it to the destination.

### 🎯 Exam Important Points

- The unit of data at the network layer is called a **datagram**.
- At data link layer, we call it a **frame**.
- Remember the terminology: Message → Segment → Datagram → Frame (top to bottom).

---

## Concept 4: Network as a Graph — Multiple Paths Between Source and Destination

### 🧠 Simple Explanation

The entire internet can be thought of as a **graph**. In this graph, each **node** represents a router (also called a Layer 3 device or L3 switch). The **edges** (connections between nodes) represent the links — these can be wired or wireless.

Now, in this graph, when you want to send a packet from a **source** to a **destination**, there are usually **multiple possible paths**. For example, if you have routers A, B, C, D connected in different ways, the packet from source to destination could go through path A→B→D or path A→C→D.

The question becomes: **which path should you choose?**

### 🎯 Exam Important Points

- The internet is represented as a **graph** with routers as nodes and links as edges.
- There are **multiple paths** between any source and destination.
- Choosing the right path is the core challenge of the network layer.

---

## Concept 5: Why Minimum Hop Path Is Not Always the Best

### 🧠 Simple Explanation

You might think: "Just pick the path with the fewest hops (fewest routers in between)." This is called the **minimum hop path**. But the transcript clearly states that the minimum hop path **may not always give optimal performance**.

Why? Because the shortest path might have **very low capacity** (low bandwidth). If you push all your packets through a low-capacity link, it gets congested, and the end-to-end performance degrades.

So instead of just counting hops, networks use **various metrics** (like bandwidth, delay, congestion level, etc.) to decide the best path.

### 🎯 Exam Important Points

- **Minimum hop path ≠ Best path always**.
- A minimum hop path may have **low end-to-end capacity**, leading to **performance degradation**.
- Networks use **different metrics** (not just hop count) to decide the best forwarding path.

### ⚠️ Common Confusions

- Don't assume "shortest path" always means "fewest hops." In networking, "shortest" can mean lowest cost based on whatever metric is chosen (bandwidth, delay, etc.).

---

## Concept 6: Graph Algorithms — Dijkstra's and Bellman-Ford

### 🧠 Simple Explanation

If you could see the **entire network topology** (all routers, all links, all link characteristics like bandwidth), you could use classic graph algorithms to find the best path. The transcript mentions two algorithms:

1. **Dijkstra's Algorithm** — finds the shortest path from a single source to all destinations.
2. **Bellman-Ford Algorithm** — also finds shortest paths, works differently (distributed approach).

The **metric** you choose (like bandwidth, delay, etc.) acts as the **weight** of each link in the graph. Then you run the algorithm to find the path with the lowest total weight.

**But here's the problem:** In real networks, you **do not** have one central place that knows the entire topology. Each router works **independently** in a **completely decentralized way**. No central coordinator exists to build a complete graph and run these algorithms.

### 🎯 Exam Important Points

- If full topology is available, **Dijkstra's** or **Bellman-Ford** algorithms can find the shortest path.
- The **path metric** (bandwidth, delay, etc.) is used as **link weight** in these algorithms.
- Real networks are **decentralized** — there is no central coordinator that knows the entire topology.
- This decentralized nature makes routing a major challenge.

### ⚠️ Common Confusions

- The transcript mentions these algorithms as theoretical possibilities. It does NOT say the internet uses a single centralized algorithm. The emphasis is on the **decentralized** nature of real routing.

---

## Concept 7: Network Routing — The Core Task

### 🧠 Simple Explanation

Because the network is decentralized, each router must **independently decide** where to forward a packet next. This decision-making process is called **routing**.

Here is how it works: A router R receives a packet. The packet has a **destination address D**. The router R has to decide — should I forward this packet to my neighbor router X, or to my neighbor router Y? Whichever gives a better path to destination D, that's where R sends the packet.

This decision is made using a **routing protocol**. The routing protocol is the set of rules and algorithms that routers use to exchange information and build their decision tables.

### 🎯 Exam Important Points

- **Routing** = The process by which each router decides the **next hop** for a packet.
- Routing is done in a **completely decentralized way**.
- A **routing protocol** is used to make these decisions.
- The router looks at the **destination address** in the packet to decide the next hop.

### 📝 Key Definition

**Routing Protocol:** A protocol that helps individual routers decide, in a decentralized way, what the best next hop should be for forwarding a packet toward its destination.

---

## Concept 8: Two Primary Requirements of the Network Layer

### 🧠 Simple Explanation

The transcript identifies **two main things** the network layer must do:

1. **Addressing:** Every host in the network must have a **unique address** so it can be identified. Without an address, how would you know where to send a packet?

2. **Routing:** Given a destination address, the network must figure out **how to forward** the packet through multiple hops to reach that destination.

These two are the pillars of the network layer — first you identify machines (addressing), then you figure out how to reach them (routing).

### 🎯 Exam Important Points

- Network layer has **two primary requirements**: (1) Addressing, (2) Routing.
- **Addressing** = uniquely identify every host.
- **Routing** = decide the path to forward packets to the destination.

---

## Concept 9: Data Link Layer vs. Network Layer — Key Difference

### 🧠 Simple Explanation

This is a very important distinction the transcript makes:

- **Data Link Layer (Layer 2):** Responsible for delivering data from **one node to the directly connected next node** (one hop). It uses Layer 2 devices like switches. Nodes must be directly connected via wire or within wireless range.

- **Network Layer (Layer 3):** Responsible for delivering data across **multiple hops**, from the source all the way to the final destination. It uses Layer 3 devices like routers.

Think of it this way: the data link layer is like walking from one room to the next room in a building. The network layer is like navigating from one city to another city — it involves many steps, and you need a map (routing).

### 🎯 Exam Important Points

- Data Link Layer = **single hop delivery** (node to directly connected node).
- Network Layer = **multi-hop delivery** (source to final destination, across many routers).
- Data Link Layer uses **Layer 2 switches**.
- Network Layer uses **Layer 3 routers** (also called L3 switches).
- The data link layer **provides a service** to the network layer — it forwards packets one hop at a time.

### ⚠️ Common Confusions

- "Layer 2 switch" and "Layer 3 switch/router" are different devices! Layer 2 switches only forward within a LAN. Routers (L3) forward across networks.

---

## Concept 10: Postal Mail Analogy for Network Layer

### 🧠 Simple Explanation

The transcript gives a very helpful analogy: packet delivery in the network is **like postal mail delivery**.

In postal mail, your address has a hierarchy: Name → House Number → Locality → City → Pin Code → State → Country. When a letter is mailed, the postal system uses this hierarchical address to forward it step by step — first to the correct country, then state, then city, then locality, then your house.

Similarly, in the network, the **addressing scheme is hierarchical**. The address tells the network: first, which network (autonomous system) the destination belongs to, and then which specific host inside that network.

### 🎯 Exam Important Points

- Network addressing follows a **hierarchical** structure, just like postal addresses.
- The hierarchy helps routers forward packets step by step toward the destination.
- First, the packet is forwarded to the correct **network**, then to the correct **host** inside that network.

---

## Concept 11: Hierarchical Internet Architecture — Building from Small to Large

### 🧠 Simple Explanation

The transcript builds the internet architecture step by step, from the smallest network to the largest. Here is the hierarchy:

**Step 1 — Local Area Network (LAN):**
Start with a single lab, say Software Lab 1 in CSE Department at IIT Kharagpur. The desktops in this lab are connected by a Layer 2 switch, forming a **LAN** (Local Area Network).

**Step 2 — Department Network:**
Software Lab 1 (LAN 1) and Software Lab 2 (LAN 2) are connected to each other via a **router** (Layer 3 device). Together they form the CSE Department network. The department may also have faculty networks, student networks, research lab networks — all connected by routers.

**Step 3 — Institute Network:**
The CSE network, the EE network, the Mechanical network, the Administrative network — all department networks within IIT Kharagpur are connected via routers. Together they form the **IIT Kharagpur network**.

**Step 4 — ERNET (Education and Research Network):**
IIT Kharagpur, IIT Bhubaneshwar, IIT Bombay, IIT Kanpur — all these institute networks are connected together through **ERNET** (Education and Research Network of India). ERNET is a government initiative to interconnect educational institutes.

**Step 5 — National and Global Level:**
ERNET, BSNL, Airtel, Vodafone — all these networks are connected under larger ISPs like Bharti Airtel at the national level. And national-level networks are connected globally via internet exchange points.

### 🎯 Exam Important Points

- Internet architecture is built **hierarchically**: LAN → Department → Institute → ERNET → National → Global.
- **LAN** is formed by connecting machines via Layer 2 switches.
- **Department networks** are formed by connecting LANs via routers.
- **ERNET** = Education and Research Network of India — connects educational institutes.
- Each level is interconnected via **Layer 3 devices (routers)**.

---

## Concept 12: Autonomous System (AS)

### 🧠 Simple Explanation

An **Autonomous System (AS)** is one of the most important concepts in this lecture. Here is the formal definition from the transcript:

An Autonomous System is **a set of local area networks under a single administrative domain**, identified by a **unique Autonomous System Number (ASN)**, where the **routing policies are controlled by a single administrator**.

In simple terms: an AS is a collection of networks managed by **one organization** with its own set of rules for routing packets.

For example, IIT Kharagpur has its own network managed by its IT administrators — that is one AS. ERNET is another AS. Bharti Airtel is another AS. Each has its own unique AS number.

The key idea is that **inside one AS, one routing policy** is typically followed. Different ASes can follow **different routing policies** — for example, one AS might prefer the fastest path while another might prefer the cheapest path.

### 🎯 Exam Important Points

- **Autonomous System (AS)** = A set of LANs under one administrative domain with a unique AS number.
- Routing policies inside an AS are controlled by a **single administrator**.
- Each AS has a **unique Autonomous System Number (ASN)**.
- Different ASes can have **different routing policies**.
- The entire ISP structure forms a **hierarchical architecture** of autonomous systems.

### 📝 Key Definition (Exam-Critical)

**Autonomous System:** A set of local area networks for an administrative domain, identified by a unique autonomous system number, where routing policies are controlled by a single administrator.

### ⚠️ Common Confusions

- An AS is NOT a single router or a single computer. It is a **collection of networks** under one management.
- While typically one routing policy is used inside an AS, the transcript mentions that there **can be** ASes with multiple routing policies.

---

## Concept 13: Autonomous System Graph of India

### 🧠 Simple Explanation

The transcript mentions that you can visualize the AS structure of India. It references the website **labs.apnic.net**, where APNIC maintains information about autonomous systems.

In this graph, the **edge nodes** are the smaller ASes at the boundary, and the **central nodes** are the major ones that provide services to many other ASes. For example, ERNET India is one such central node that provides service to all educational institutes.

### 🎯 Exam Important Points

- **APNIC** maintains autonomous system information (for the Asia-Pacific region).
- The AS graph has **edge nodes** (smaller ASes) and **central nodes** (large service-providing ASes).
- Educational institutes get service from **ERNET India** as a central AS.

---

## Concept 14: ISP Tier Architecture

### 🧠 Simple Explanation

The transcript describes a **tiered architecture** of Internet Service Providers (ISPs):

**End Users:** These are the people or devices that use the internet (you, me, our phones, laptops).

**Tier 3 ISPs (Local ISPs):** These directly serve the end users. Examples include Airtel, Vodafone, ERNET — these are the ISPs you directly connect to.

**Tier 2 ISPs (Regional ISPs):** These provide service to Tier 3 ISPs. They may have connectivity among themselves.

**Tier 1 ISPs (National Service Providers / NSPs):** These are the country-level ISPs that provide service to Tier 2 ISPs. They connect to each other globally via internet exchange points.

So the flow is: End Users → Tier 3 → Tier 2 → Tier 1 (and back down on the destination side).

### 🎯 Exam Important Points

- ISP architecture has **three tiers**: Tier 1 (national/global), Tier 2 (regional), Tier 3 (local).
- End users connect to **Tier 3 ISPs**.
- Tier 3 ISPs get service from **Tier 2 ISPs**.
- Tier 2 ISPs get service from **Tier 1 ISPs** (NSPs — National Service Providers).
- **ISPs are autonomous systems** that provide internet connectivity to other ASes or end users.

---

## Concept 15: Point of Presence (PoP)

### 🧠 Simple Explanation

The transcript introduces **Point of Presence (PoP)**. A PoP is a **small edge network** that takes service from a larger network but **does not provide service to others**.

The example given is a **military network**. The military network uses the internet for its own internal purposes but does not provide internet service to others. Such networks are directly connected to the central network and act as a PoP.

### 🎯 Exam Important Points

- **Point of Presence (PoP)** = An edge network that receives service but does not provide it to others.
- Example: Military network — uses internet internally, does not serve others.
- PoPs are connected to the Tier 2 or central network directly.

---

## Concept 16: Private Peering Between ISPs

### 🧠 Simple Explanation

The transcript explains an interesting concept called **private peering**. When two regional ISPs (like Airtel and Vodafone) establish a **direct connection** between themselves, they can share data directly without going through the higher-level ISPs.

This is beneficial because it **reduces charges**. When data travels through fewer intermediate ISPs, the cost is lower. That is why sometimes you hear that transferring data between two subscribers of the same ISP, or between ISPs with peering agreements, is cheaper.

### 🎯 Exam Important Points

- **Private Peering** = A direct connection between two ISPs to exchange data.
- Private peering **reduces charges** because data does not need to travel through higher-tier ISPs.
- Regional ISPs can have private peering relationships with each other.

---

## Concept 17: Internet Exchange Points and Transatlantic Lines

### 🧠 Simple Explanation

**Internet Exchange Points** are the places where different national-level ISPs (NSPs) connect to each other globally. They allow packets to travel from one country to another.

The transcript gives the example of **transatlantic lines** — high-speed optical fiber cables laid across the Atlantic Ocean that connect the European continent with the US continent. These transatlantic lines are one example of an internet exchange point infrastructure.

### 🎯 Exam Important Points

- **Internet Exchange Points** connect different national-level ISPs globally.
- **Transatlantic lines** = High-speed optical fiber cables through the Atlantic Ocean connecting Europe and USA.
- They enable **international packet forwarding** between NSPs of different countries.

---

## Concept 18: Packet Forwarding — The Hierarchical Journey

### 🧠 Simple Explanation

When you send a packet from your machine to a destination in another country, the packet follows a **hierarchical path**, just like postal mail:

**Sending side:**
Your Machine → Local ISP → Regional ISP → National ISP (NSP) → Transit ISP

**Receiving side:**
Transit ISP → National ISP → Regional ISP → Local ISP → Destination Machine

This mirrors exactly how postal mail works: your local post office → regional post office → national post office → destination country's post office → regional → local → final address.

### 🎯 Exam Important Points

- Packet forwarding follows a **hierarchical fashion**: local → regional → national → transit → (and then reverse at the destination side).
- This is the **same concept** as postal mail delivery.
- Each ISP at each level is essentially a **set of computers connected via LANs**.

---

## Concept 19: Network Address Must Identify Both Network and Host

### 🧠 Simple Explanation

This is a crucial concept for understanding IP addressing (which will come in later lectures). The transcript emphasizes that the **address** given to each host in the network must do **two things**:

1. **Identify the network** (which autonomous system the host belongs to).
2. **Identify the specific host** inside that network.

The postal analogy works perfectly here. When you mail a letter to "Prof. X, Department of CSE, IIT Bombay," the postal system first routes the letter to IIT Bombay (the network), and then inside IIT Bombay, it identifies the specific person (the host).

Similarly, the network first routes the packet to the correct AS (the network), and then inside that AS, it identifies the specific machine (the host).

### 🎯 Exam Important Points

- The network address has **two parts**: one identifies the **network**, and the other identifies the **host** within that network.
- First, the packet is routed to the correct **network/AS**.
- Then, within that network, the specific **host** is identified.
- This is why network addresses are designed with a **hierarchical structure**.

### ⚠️ Common Confusions

- The address is NOT just a random number. It has a **structured format** where part of the address tells you the network and part tells you the host. This will become clear when you study IP addresses in later lectures.

---

## Concept 20: Routing Between Autonomous Systems

### 🧠 Simple Explanation

When a packet needs to go from Host 1 (connected to AS 10) to Host 2 (connected to AS 12), the packet must travel **from the network of AS 10 to the network of AS 12**.

Between them, there might be intermediate autonomous systems like AS 11 or AS 13. The **routing protocol** decides which intermediate AS to use. Should the packet go through AS 11 or AS 13? This decision depends on the routing protocol and the metrics being used.

### 🎯 Exam Important Points

- Packets travel **between autonomous systems** to reach the destination.
- **Multiple intermediate ASes** may exist between source and destination.
- The **routing protocol** decides which intermediate AS to use.
- Each AS has a **unique AS number** (like AS 10, AS 11, AS 12, AS 13 in the example).

---

## Concept 21: Preview of IP Protocol

### 🧠 Simple Explanation

The lecture concludes by mentioning that in the next class, the course will study how to design a **hierarchical addressing mechanism** using the widely used network protocol called the **IP protocol** (Internet Protocol).

The IP protocol provides the addressing format that identifies both the network and the host, which is exactly what the network layer needs.

### 🎯 Exam Important Points

- The addressing at the network layer is done using the **IP protocol**.
- IP addressing follows a **hierarchical mechanism**.
- This will be covered in detail in the upcoming lectures.

---

## Summary Table: Key Concepts at a Glance

| Concept | Key Point |
|---|---|
| Network Layer Position | Layer 3 of TCP/IP, also called Internet Layer |
| Main Objective | Deliver packets from source to destination across multiple hops |
| Delivery Model | Unreliable datagram delivery (best effort) |
| Data Unit | Datagram |
| Data Link vs Network Layer | Data Link = 1 hop; Network = multiple hops |
| Routing | Decentralized process for deciding next hop at each router |
| Addressing | Hierarchical — identifies both network and host |
| Autonomous System | Set of LANs under one admin with unique ASN |
| ISP Tiers | Tier 1 (national), Tier 2 (regional), Tier 3 (local) |
| Private Peering | Direct link between ISPs to reduce cost |
| Internet Exchange Points | Connect national ISPs globally (e.g., transatlantic cables) |
| PoP | Edge network that takes service but doesn't provide it |
| Graph Algorithms | Dijkstra's and Bellman-Ford — used if full topology is known |
| IP Protocol | Will provide the hierarchical addressing mechanism |

---

## 10 MCQs — Strictly from Lecture 26

---

### Q1. What is the unit of data at the network layer called?

A) Segment  
B) Frame  
C) Datagram  
D) Message  

**Answer: C) Datagram**

**Explanation:** The transcript clearly states that at the network layer, the unit of data is called a **datagram**. Segment is used at the transport layer, frame at the data link layer, and message at the application layer.

---

### Q2. What type of delivery service does the network layer provide in the TCP/IP protocol stack?

A) Reliable delivery with retransmission  
B) Connection-oriented delivery  
C) Unreliable datagram delivery (best effort)  
D) Guaranteed delivery with acknowledgment  

**Answer: C) Unreliable datagram delivery (best effort)**

**Explanation:** The transcript states that the network layer provides **unreliable datagram delivery**, which is a best effort service. It tries its best to deliver but cannot guarantee delivery because packets may get dropped at intermediate router buffers.

---

### Q3. Which layer is responsible for single-hop delivery (one node to the next directly connected node)?

A) Network Layer  
B) Transport Layer  
C) Data Link Layer  
D) Application Layer  

**Answer: C) Data Link Layer**

**Explanation:** The transcript explains that the **data link layer** ensures delivery from one node to the next directly connected node (one hop). The network layer handles multi-hop delivery across the entire network.

---

### Q4. Why is the minimum hop path not always the best choice for packet forwarding?

A) It always has the highest delay  
B) It may have very low end-to-end capacity, leading to performance degradation  
C) Routers on minimum hop paths are always congested  
D) Minimum hop path is not supported by IP protocol  

**Answer: B) It may have very low end-to-end capacity, leading to performance degradation**

**Explanation:** The transcript specifically states that the minimum hop path may have a very low capacity, and pushing all packets through it causes degradation in end-to-end forwarding performance. That is why networks use various other metrics.

---

### Q5. What is an Autonomous System (AS)?

A) A single router in the internet  
B) A set of LANs under one administrative domain with a unique AS number, where routing policies are controlled by a single administrator  
C) A protocol used for packet delivery  
D) A type of internet exchange point  

**Answer: B)**

**Explanation:** The transcript defines an Autonomous System as a set of local area networks for an administrative domain, identified by a unique autonomous system number, where routing policies are controlled by a single administrator.

---

### Q6. What does ERNET stand for, according to the transcript?

A) Electronic Research Network  
B) Education and Research Network of India  
C) Enterprise Resource Network  
D) External Routing and Networking Tool  

**Answer: B) Education and Research Network of India**

**Explanation:** The transcript states that ERNET is the **Education and Research Network of India**, a government initiative to interconnect educational institutes like IITs and central universities.

---

### Q7. What is a Point of Presence (PoP) in the context of internet architecture?

A) A central ISP that provides service to all others  
B) An edge network that takes service but does not provide service to others  
C) A router that connects two autonomous systems  
D) A data center for cloud services  

**Answer: B) An edge network that takes service but does not provide service to others**

**Explanation:** The transcript defines PoP as an edge network that takes service but does not provide service to others. The example given is a military network used only for internal purposes.

---

### Q8. What is "private peering" between ISPs?

A) A connection where one ISP pays the other for all traffic  
B) A direct connection between two ISPs allowing them to share data directly, reducing charges  
C) A government regulation requiring ISPs to share data  
D) A type of routing protocol  

**Answer: B) A direct connection between two ISPs allowing them to share data directly, reducing charges**

**Explanation:** The transcript explains private peering as a direct relationship between two service providers where they can share data directly among themselves, which reduces the charging policy.

---

### Q9. In the ISP tier architecture, which tier directly serves the end users?

A) Tier 1  
B) Tier 2  
C) Tier 3  
D) Transit ISP  

**Answer: C) Tier 3**

**Explanation:** The transcript describes the tier architecture where **Tier 3 ISPs** (local ISPs like Airtel, Vodafone, ERNET) directly provide services to end users. Tier 2 serves Tier 3, and Tier 1 serves Tier 2.

---

### Q10. According to the transcript, what are the two primary requirements of the network layer?

A) Encryption and Compression  
B) Addressing and Routing  
C) Flow control and Congestion control  
D) Multiplexing and Demultiplexing  

**Answer: B) Addressing and Routing**

**Explanation:** The transcript clearly identifies two primary requirements: (1) **Addressing** — uniquely identifying every host with a hierarchical address, and (2) **Routing** — deciding how to forward packets through multiple hops to reach the destination. These are the two pillars of the network layer.

---

*End of Lecture 26 Study Guide*
