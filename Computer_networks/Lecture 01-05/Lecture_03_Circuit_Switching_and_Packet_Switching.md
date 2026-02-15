# Lecture 3 — Circuit Switching and Packet Switching

## Complete Study Guide (NPTEL: Computer Networks and Internet Protocol)

**Instructor:** Prof. Soumya Kanti Ghosh, IIT Kharagpur

---

## Concept 1: Switched Network — The Foundation

### 📌 What is a Switched Network?

### 🧠 Simple Explanation

Imagine you want to talk to your friend who lives far away. Your phone is not directly connected to your friend's phone. Instead, there are many intermediate devices (called **switching nodes**) between you and your friend. These nodes help "switch" or "pass" your voice/data from one node to another until it reaches your friend.

This is what a **Switched Network** is — a network where data travels from a source to a destination by being **switched (hopped) from one intermediate node to another**.

**Key points from the transcript:**

- Communication between distant stations (end devices) is done through a **network of switching nodes**.
- The switching nodes **do not care about the content** of the data. Their only job is to provide a **path** (route) from source to destination.
- A collection of **nodes** (devices) and **connections** (links between them) together form a **communication network**.
- Data entering the network from the source station is **routed** to the destination by being switched from node to node.

### 🛠 Real-world Example (from transcript)

Think of a telephone call. Your phone connects to your **nearest telephone exchange** (this is the first switching node). Then through **trunk connections**, the call is passed from exchange to exchange until it reaches the other person's exchange, and finally their phone. At every exchange, the call is "switched" to the next hop.

### 🎯 Exam Important Points

- Switching nodes are **not concerned with data content** — they only provide a switching path.
- Data is **routed** from source to destination via intermediate switching nodes.
- A switch network has **nodes + connections = communication network**.

### ⚠️ Common Confusions

- "Switching node" does NOT mean it reads or processes the data. It only forwards/routes the data.
- Don't confuse "switching node" with "end device/station". End devices are computers, phones, servers, etc. Switching nodes are the intermediate routers/exchanges.

---

## Concept 2: Typical Switching Network — Multiple Paths, Challenges

### 📌 How Does a Switching Network Look?

### 🧠 Simple Explanation

The transcript describes a typical switching network diagram (referenced from "Data Communication by William Stallings"). In this network:

- There are **end systems** (also called stations) — these can be mainframes, servers, personal computers, mobile devices, etc.
- In between, there is a **switching cloud** — this is the collection of switching nodes numbered 1, 2, 3, 4, 5, 6, 7.
- If station **B** wants to communicate with station **D**, the data goes through intermediate switching nodes.

**Important observations:**

- **Multiple paths can exist** between source and destination. For example, B to D can go through path 1→4→5→3→D, or some other combination.
- **There can be no path** — if all nodes or links are down.
- **A path can break during communication** — if a node fails or an edge (link) fails while you are communicating, the communication can be disrupted.
- The intermediate switches **decide which path** to follow. This can be a **predefined path** (someone fixed it), or can be decided by a **routing algorithm**.

### 🎯 Exam Important Points

- Multiple paths can exist from source to destination.
- Path failure (node or link failure) can disrupt communication.
- The route can be predefined or decided dynamically by routing algorithms.

### ⚠️ Common Confusions

- Users (end devices) don't need to worry about the switching — it is handled by the switching cloud internally.
- "Switching nodes" and "stations" are two different categories of nodes in the network.

---

## Concept 3: Two Predominant Switching Technologies

### 📌 Circuit Switching vs. Packet Switching

### 🧠 Simple Explanation

The transcript tells us that there are **two main switching techniques**:

**1. Circuit Switching:**
- A **dedicated circuit (path)** is established from source to destination.
- Once the path is set up, the entire path is **reserved** only for that communication.
- Example: Traditional telephone calls.

**2. Packet Switching:**
- Data is **broken into small chunks** called **packets**.
- Each packet is transmitted **independently** through the network.
- Packets may take **different routes** and arrive at different times.
- Example: Internet data communication.

The transcript also mentions:
- A network is **usually partially connected** — not all routes are pre-established. Routes are set up on demand.
- **Redundant connections** are desirable for **reliability**. If one path fails, another path can be used.

### 🎯 Exam Important Points

- Two predominant technologies: **Circuit Switching** and **Packet Switching**.
- Circuit switching = dedicated path; Packet switching = data split into independent packets.
- Redundant paths improve reliability.

---

## Concept 4: Circuit Switching — In Detail

### 📌 Circuit Switching — Dedicated Communication Path

### 🧠 Simple Explanation

In circuit switching, a **dedicated communication path** is established between two stations. Think of it like building a private road just for you — no one else can use it while you are using it.

**Three Phases of Circuit Switching:**

| Phase | What Happens | Analogy |
|-------|-------------|---------|
| **Phase 1: Path Establishment (Connection Setup)** | Resources are **acquired/allocated** along the path from source to destination. The path is set up through all intermediate switching nodes. | You book a private highway from your city to another city. |
| **Phase 2: Data Transfer** | Communication happens. It can be **bi-directional (full-duplex)**. The path is transparent — it works like a direct line. | You drive on your private highway freely in both directions. |
| **Phase 3: Disconnection (Teardown)** | Once communication is over, the connection is **released**. Resources become free for others. | You return the highway — now others can use it. |

**Important details from transcript:**

- "Acquire resources" means reserving capacity at each switching node and each link along the path.
- Once established, the path is **transparent** — meaning data flows as if it is a direct dedicated line.
- When one connection is active, those resources are **blocked** (occupied). If all connections are occupied, a new request gets a "no connection available" message — this is called **blocking**.

### 🎯 Exam Important Points

- Circuit switching has **3 phases**: establishment, data transfer, disconnection.
- The path is **dedicated** — reserved exclusively for one communication session.
- Resources are **acquired** during setup and **released** during teardown.
- Communication can be **full-duplex** (both directions at the same time).

### ⚠️ Common Confusions

- "Transparent" does NOT mean invisible. It means the connection works so smoothly that it feels like a direct line — you don't notice the intermediate switches.
- Blocking happens when ALL available connections are busy, not when just one is busy.

---

## Concept 5: Blocking vs. Non-blocking Architecture

### 📌 Blocking and Non-blocking

### 🧠 Simple Explanation

The transcript introduces two important architecture types:

- **Blocking Architecture:** If, say, 10 connections are possible and all 10 are already occupied, then the 11th connection request will get a "no connection available" message. The system is **blocked** — cannot serve more.
- **Non-blocking Architecture:** The system has **enough resources** to handle all possible connection requests simultaneously. No one is ever denied.

### 🎯 Exam Important Points

- Blocking = limited connections; when all are busy, new ones are refused.
- Non-blocking = sufficient resources to handle all requests simultaneously.

---

## Concept 6: Requirements for Circuit Switching

### 📌 What Does Circuit Switching Need?

### 🧠 Simple Explanation

For circuit switching to work, the transcript says two things are needed:

1. **Switching Capacity:** Enough free paths must be available. The switch must have free ports/connections.
2. **Channel Capacity:** The communication channel (link) must have enough capacity to carry the data/information.

Additionally:
- The network must have enough **intelligence** (routing algorithm or predefined rules) to **work out the routing** — i.e., to decide which path the connection should follow.
- In some cases, routes can be **dedicated** (fixed), and in other cases, a **routing algorithm** dynamically finds the best path.

### 🎯 Exam Important Points

- Circuit switching needs both **switching capability** and **channel capacity**.
- Intelligence (routing algorithm) is needed to determine the path.
- Path can be predefined or algorithmically determined.

---

## Concept 7: Types of Circuit Switching Approaches

### 📌 Four Types of Circuit Switching

### 🧠 Simple Explanation

The transcript mentions **four basic approaches** for circuit switching:

| Approach | Meaning |
|----------|---------|
| **Space Division Switch** | Uses physical cross-connect points (like a matrix). Input 1 can connect to Output 4 by switching on the appropriate crosspoint. A controller manages which crosspoints are active. |
| **Time Division Switch** | Data is switched based on **time slots**. The order of data in time slots can be rearranged (e.g., BCDA can become BADC) to route data to the correct destination. |
| **TDM Bus (Time Division Multiplexing Bus)** | Uses a shared bus with time division multiplexing. |
| **Combination** | A mix of the above techniques. |

### 🎯 Exam Important Points

- Four approaches: Space Division, Time Division, TDM Bus, and Combination.
- Space division uses a **matrix of crosspoints**.
- Time division uses **rearranging time slots**.

---

## Concept 8: Space Division Switching — Single Stage and Multi Stage

### 📌 Space Division Switching

### 🧠 Simple Explanation

**Single Stage:**
- Think of it as a **matrix** (grid). You have inputs on one side and outputs on the other. At each intersection, there is a **crosspoint** that can be switched ON or OFF.
- A **controller** decides which crosspoints to turn on, thereby connecting a specific input to a specific output.
- Example: If you have 10 inputs and 10 outputs, you need a 10×10 matrix = 100 crosspoints.

**Multi Stage:**
- Instead of one huge matrix, you break it into **multiple smaller stages** of switches.
- This **reduces the total number of crosspoints** needed — it is an optimization.
- However, multi-stage switching introduces the concern of whether it becomes a **blocking or non-blocking** architecture — i.e., can all connections happen simultaneously or not?

### 🎯 Exam Important Points

- Single stage = one big matrix; Multi stage = smaller switches in stages.
- Multi stage **reduces crosspoints** but may introduce blocking.
- Overall planning is needed when designing multi-stage switching.

---

## Concept 9: Time Division Switching & Time Slot Interchange (TSI)

### 📌 Time Division Switching

### 🧠 Simple Explanation

In time division switching, data arrives in **time slots** (like BCDA), and by **rearranging the order** of these slots, you can route data to the correct destination.

**Example from transcript:**
- Input order: B, C, D, A
- If D needs to communicate with output 2, the switch rearranges the time slots.
- Output order becomes: B, A, D, C (or whatever is needed).

**Time Slot Interchange (TSI):**
- There is a **control unit** that decides the order.
- Data comes in **sequentially** (one slot after another).
- The control unit **selectively controls** which data goes out and in what order.
- This effectively simulates switching — by changing the position of data in time slots, you route it to the right destination.

### 🎯 Exam Important Points

- Time division switching works by rearranging time slots.
- TSI uses a control unit to selectively output data in a different order.
- This simulates switching without physical crosspoints.

---

## Concept 10: Circuit Switching — Properties and Issues

### 📌 Properties and Issues of Circuit Switching

### 🧠 Simple Explanation

The transcript lists several important properties and issues:

**Properties:**

1. **Transparent once connected:** Once the circuit is set up, data flows as if there is a direct dedicated line. You don't feel the intermediate switches. This is like your traditional telephone connection — once connected, you just talk.

2. **Primarily developed for voice traffic:** Circuit switching was originally designed for telephone (voice) communication.

3. **Dedicated connection:** The entire path and its resources are reserved exclusively for one session.

**Issues:**

1. **Inefficiency:** The channel capacity is **fully dedicated** even when no data is being transmitted. Even if you hold the phone and don't speak, the channel is **wasted** — no other party can use it.

2. **Setup time:** The **establishment phase takes time**. You cannot start communication until the full end-to-end connection is established.

3. **Fixed data rate:** Both source and destination must operate at the **same data rate**. If they don't match, there will be data accumulation or overflow problems.

4. **Could have been multiplexed:** The channel could have been shared using techniques like frequency division multiplexing, but in circuit switching, it is fully dedicated.

### 🎯 Exam Important Points

- Once connected → transparent (feels like direct line).
- Developed for **voice traffic**.
- **Inefficient** — wastes bandwidth when no data flows.
- **Setup takes time** — no communication until full path is established.
- **Fixed data rate** — both ends must operate at same speed.
- Channel is **dedicated and wasted** if idle.

### ⚠️ Common Confusions

- "Transparent" is a good thing — it means smooth, seamless communication.
- "Fixed data rate" does NOT mean the rate is always low. It means source and destination must match their speeds.

---

## Concept 11: Packet Switching — In Detail

### 📌 Packet Switching — Breaking Data into Packets

### 🧠 Simple Explanation

Packet switching is the **primary interest** for data networks (and for this course). Here is how it works:

1. The source station **breaks the data/message into small packets** — typically around 1000 octets (1 octet = 8 bits, so about 1000 bytes per packet).
2. Each packet contains **two things:**
   - **A portion of the actual data** (part of the message).
   - **Control information** — this includes routing/addressing information (where the packet should go) and other details.
3. Packets are sent into the network **sequentially** — one after another.
4. Packets travel through the network and are **delivered to the destination**.

**Store and Forward Mechanism:**
- When a packet arrives at an intermediate switching node, the node:
  - **Receives** the packet.
  - **Stores** (buffers) it briefly.
  - **Forwards** it to the next node.
- This is called the **store-and-forward** mechanism.

### 🎯 Exam Important Points

- Data is broken into **small packets** (typically ~1000 octets).
- Each packet has **data + control information** (for routing).
- Packets move independently through the network.
- Intermediate nodes use **store-and-forward**: receive → store → forward.
- Control information is primarily for **routing/addressing**.

### ⚠️ Common Confusions

- A "packet" is NOT the entire message. It is a **small piece** of the message plus some control information.
- Store-and-forward means the node waits until it receives the complete packet before forwarding — it does NOT forward bit by bit.

---

## Concept 12: Advantages of Packet Switching

### 📌 Why is Packet Switching Better for Data Networks?

### 🧠 Simple Explanation

The transcript lists several advantages:

**1. Line Efficiency:**
- A single node-to-node link can be **shared by many packets** over time.
- Unlike circuit switching (where one link is dedicated to one connection), here the same link carries packets from many different communications.

**2. Data Rate Conversion:**
- Each station connects to its local node at **its own speed**.
- The nodes can **buffer the data** and **equalize/synchronize the rates** if source and destination operate at different speeds.
- This is possible because of the store-and-forward mechanism — packets are small and handled independently.

**3. Accepts Packets Even When Network is Busy:**
- Even if the network is congested, packets can still be **accepted** (stored in buffers).
- Delivery may be **slower**, but the packet is not immediately rejected.
- In circuit switching, if no circuit is available, you simply cannot communicate.

**4. Prioritization:**
- You can assign **priority** to certain packets.
- Important packets can be sent first; less important ones can wait.

### 🎯 Exam Important Points

- Line efficiency: links are **shared** by many packets.
- Data rate conversion: nodes **buffer and equalize** speeds.
- Network accepts packets even when busy (delivery may be slow).
- Packets can be **prioritized**.

---

## Concept 13: Two Techniques of Packet Switching

### 📌 Datagram vs. Virtual Circuit

### 🧠 Simple Explanation

Packet switching has **two approaches**:

| Feature | Datagram | Virtual Circuit |
|---------|----------|-----------------|
| **Path** | No predefined path. Each packet moves **independently**. | A **preplanned route** is established before data transfer. |
| **Routing decision** | Made **individually for each packet** at every node. | Made **once** during setup. Packets follow the established path. |
| **Packet identification** | Each packet carries full **destination address**. | Each packet carries a **Virtual Circuit Identifier (VCI)** instead of full address. |
| **Order of arrival** | Packets may arrive **out of order**. | Packets arrive **in sequence**. |
| **Setup** | **No call setup** needed. | **Call request and call accept** packets are exchanged first (handshaking). |
| **Packet loss** | Packets may get **lost or delayed**. | Less likely because path is pre-established. |
| **Receiver responsibility** | Receiver must **reorder packets** and recover missing ones (e.g., request retransmission). | Sequencing and error control are provided by the network. |

### 🎯 Exam Important Points

- Datagram = independent packets, no setup, may arrive out of order.
- Virtual Circuit = pre-established path, uses VCI, packets arrive in order.
- Datagram: each packet has full destination address.
- Virtual Circuit: each packet has VCI (not destination address).

### ⚠️ Common Confusions

- Virtual Circuit is NOT the same as Circuit Switching! Virtual circuit is still packet switching — packets are still store-and-forwarded. The path is "virtual" — it is not a physically dedicated line. The path can even be shared by other virtual circuits.
- In datagram, different packets of the same message CAN take different routes.

---

## Concept 14: Virtual Circuit — How VCI Works

### 📌 Virtual Circuit Identifier (VCI) and Routing Tables

### 🧠 Simple Explanation

In a virtual circuit, every switch (node) has a **routing table**. This table tells the switch:

- "If I receive a packet with **VCI X** on **Port A**, I should forward it to **Port B** with **VCI Y**."

**Example from transcript:**

A switch receives data with VCI 77 on Port 1. Its table says:
- Port 1, VCI 77 → Forward to Port 2, VCI 14.

The **VCI numbers are local** — they only have meaning at that particular switch. A different switch can use the same VCI number for a completely different connection. This is important because it means VCI numbers don't need to be globally unique — they are **localized**, which makes the system simpler and more efficient.

**Larger example from transcript:**
- Source A sends a packet with VCI 14 on Port 1 of Switch 1.
- Switch 1 table: Port 1, VCI 14 → Port 3, VCI 66.
- Switch 2 receives VCI 66 on Port 1 → forwards to Port 2 with VCI 22.
- Switch 3 receives VCI 22 on Port 2 → forwards to Port 3 to destination B.

This chain of VCI mappings establishes the **virtual circuit from A to B**.

### 🎯 Exam Important Points

- VCI is **local** to each switch — same VCI number can exist at different switches for different circuits.
- Each switch has a routing table: (incoming port, incoming VCI) → (outgoing port, outgoing VCI).
- No individual routing decision needed per packet once the circuit is established.
- VCI replaces the full destination address in the packet header.

### ⚠️ Common Confusions

- VCI is NOT a global address. It changes at every hop (switch).
- The VCI at one switch has NO relation to the VCI at another switch for the same connection.

---

## Concept 15: Virtual Circuit vs. Datagram — Detailed Comparison

### 📌 Comparing the Two Packet Switching Techniques

### 🧠 Simple Explanation

The transcript provides a direct comparison:

**Virtual Circuit Advantages:**
- Can provide **sequencing** (packets arrive in order) and **error control**.
- Packets are **forwarded more quickly** because no routing decision is needed at each node — the path is already decided.

**Virtual Circuit Disadvantages:**
- **Less reliable** in one sense: if a node on the path **fails**, ALL virtual circuits passing through that node are **lost**. This means not only reliability loss, but also the **extra cost of re-establishing** those circuits and retransmitting the data.

**Datagram Advantages:**
- **No call setup** needed — each packet moves independently.
- **Better when the number of packets is small** — no overhead of setting up a path.
- **More flexible** — routing can dynamically **avoid congested** parts of the network by sending packets through less busy routes.

**Datagram Disadvantages:**
- If there is a **huge volume of packets**, there is a lot of overhead (each packet needs individual routing decisions).
- Packets may arrive out of order; receiver must reorder.

### 🎯 Exam Important Points

- Virtual Circuit: faster forwarding, sequencing, but less reliable if a node fails.
- Datagram: no setup, flexible routing, better for small number of packets.
- Node failure in virtual circuit = ALL circuits through that node are lost.
- Datagram can avoid congestion by dynamic routing.

---

## Concept 16: Circuit Switching vs. Packet Switching — Final Comparison

### 📌 The Big Picture Comparison

### 🧠 Simple Explanation

This is the most important comparison in this lecture. The transcript gives a clear side-by-side:

| Feature | Circuit Switching | Packet Switching |
|---------|-------------------|------------------|
| **Bandwidth** | **Guaranteed** — once established, bandwidth is reserved. | **Dynamically allocated** — bandwidth is given as needed. Not guaranteed. |
| **Effect of network traffic** | Circuit capacity is **NOT reduced** by other traffic — it is dedicated. | May have **concurrent transmissions** over the same physical channel. |
| **Cost vs. data** | Cost is **independent of data amount** — you pay the same whether you send data or not. This leads to **wastage of bandwidth**. | **More effective** and better performance — no blocking of channel when not communicating. |
| **Delay and congestion** | No congestion on the dedicated path (but setup delay exists). | May have **delay and congestion** because packets share resources. |
| **Best suited for** | **Voice communication** (telephone networks). | **Data communication** (computer networks, internet). |

### 🎯 Exam Important Points

- Circuit Switching: guaranteed bandwidth, dedicated path, wasteful if idle, good for voice.
- Packet Switching: dynamic bandwidth, shared resources, may have delay/congestion, good for data.
- This course (Computer Networks and Internet Protocol) primarily focuses on **packet switched networks**.
- Packet switching provides **best effort service** — it is inherently **unreliable**. For reliability, additional mechanisms/protocols are needed (to be studied later in the course).

### ⚠️ Common Confusions

- "Best effort service" does NOT mean it tries its best and succeeds. It means the network tries to deliver packets but makes **no guarantees** — packets may be lost, delayed, or arrive out of order.
- Packet switching is NOT always better than circuit switching. For real-time voice, circuit switching is actually preferred.

---

## Concept 17: Telephone Network as Circuit Switching Example

### 📌 Real-world Circuit Switching

### 🧠 Simple Explanation

The transcript uses the **traditional telephone network** as the primary example of circuit switching:

- Your phone connects to the **nearest exchange** (first office).
- The exchange connects through **connecting trunks** and **intercity trunks** to other exchanges.
- A complete **dedicated path** is established from your phone to the other person's phone.
- This path is reserved — **no one else can use these resources** until you disconnect.
- All exchanges and trunks involved are primarily **circuit-switched**.

### 🎯 Exam Important Points

- Traditional telephone networks = circuit switching.
- Path: Phone → Nearest Exchange → Connecting Trunk → Intercity Trunk → ... → Destination Exchange → Destination Phone.
- The whole path is **reserved/dedicated**.

---

## Concept 18: Course Direction — Focus on Packet Switching

### 📌 What Will This Course Focus On?

### 🧠 Simple Explanation

The transcript ends by clearly stating:

- For **computer networks and internet protocols**, we will primarily look at **packet-switched networks**.
- In packet-switched networks, packets move **independently** from source to destination.
- In many cases, this is an **unreliable, best-effort service**.
- If we want **reliability**, we need **additional mechanisms** (protocols) — these will be studied in subsequent lectures.

### 🎯 Exam Important Points

- This course focuses on **packet switching**.
- Packet switching provides **best-effort, unreliable** service by default.
- Reliability requires **additional protocols** (studied later).

---
---

## 📝 10 MCQs from Lecture 3

---

**Q1.** In a switched communication network, switching nodes are primarily concerned with:

(A) Reading and processing the content of data
(B) Providing a switching facility or path between source and destination
(C) Storing data permanently at intermediate nodes
(D) Encrypting the data before forwarding

**Answer: (B)**
**Explanation:** The transcript clearly states: "Switching nodes do not concerned with the content of the data. The purpose is to provide a switching facility or a path between the source and the destination."

---

**Q2.** Which of the following is the correct sequence of phases in circuit switching?

(A) Data Transfer → Establishment → Disconnection
(B) Disconnection → Establishment → Data Transfer
(C) Establishment → Data Transfer → Disconnection
(D) Data Transfer → Disconnection → Establishment

**Answer: (C)**
**Explanation:** The transcript describes three phases: first path establishment (acquire resources), then data transfer (communication), then disconnection (release resources).

---

**Q3.** In packet switching, what does each packet contain?

(A) Only the actual data
(B) Only control/routing information
(C) A portion of the data plus control information
(D) The entire message and destination address

**Answer: (C)**
**Explanation:** The transcript says: "Each packet contains a portion of the data plus some control information" — the control information is primarily for routing/addressing.

---

**Q4.** In a virtual circuit packet switching, what does each packet carry instead of the full destination address?

(A) Source MAC address
(B) Virtual Circuit Identifier (VCI)
(C) IP address
(D) Sequence number only

**Answer: (B)**
**Explanation:** The transcript states: "Each packet contains a virtual circuit identifier or VCI instead of the destination address."

---

**Q5.** Which of the following is a disadvantage of circuit switching as described in the lecture?

(A) Packets may arrive out of order
(B) Channel capacity is wasted if no data is being transmitted
(C) Each packet needs individual routing decisions
(D) Data rate can vary between source and destination

**Answer: (B)**
**Explanation:** The transcript says: "Even if you are not communicating holding the phone or even no data communication is not there, the channel capacity is wasted — no other party can use the things."

---

**Q6.** In datagram packet switching, which of the following is TRUE?

(A) Packets always arrive in order
(B) A predefined path is established before sending
(C) Packets may arrive out of order and may get lost or delayed
(D) No routing decision is needed

**Answer: (C)**
**Explanation:** The transcript states: "Packets may arrive out of order... packet may get lost or delayed." In datagram, each packet moves independently and can take any practical route.

---

**Q7.** What is the store-and-forward mechanism in packet switching?

(A) Packets are stored permanently at every node
(B) Packets are received, stored briefly (buffered), and then forwarded to the next node
(C) Packets are forwarded without being stored
(D) Only the first packet is stored; rest are forwarded directly

**Answer: (B)**
**Explanation:** The transcript says: "Once a packet reaches an intermediate switching node, it receives it, store it and forward it."

---

**Q8.** In a virtual circuit, the VCI (Virtual Circuit Identifier) is:

(A) Globally unique across the entire network
(B) The same at every switch along the path
(C) Localized — it has meaning only at a particular switch
(D) Assigned by the destination station

**Answer: (C)**
**Explanation:** The transcript explains that VCI is "interestingly localized" — at each switch, the VCI can change. The same VCI number can be used at different switches for different connections.

---

**Q9.** Which switching technique is more suitable for voice communication according to the lecture?

(A) Datagram packet switching
(B) Virtual circuit packet switching
(C) Circuit switching
(D) TDM bus switching

**Answer: (C)**
**Explanation:** The transcript states: "Circuit switch network is more amicable for voice communication." Traditional telephone networks use circuit switching.

---

**Q10.** What is a key advantage of packet switching over circuit switching?

(A) Guaranteed bandwidth
(B) No delay or congestion
(C) A single node-to-node link can be shared by many packets (line efficiency)
(D) Fixed data rate at both ends

**Answer: (C)**
**Explanation:** The transcript lists "line efficiency" as the first advantage of packet switching: "Single node to node link can be shared by many packets over the time because now it is no dedicated path, I have small packets."

---

## Summary of All Topics Covered in Lecture 3

| # | Topic |
|---|-------|
| 1 | Switched Network — concept and purpose |
| 2 | Typical Switching Network — diagram, multiple paths, challenges |
| 3 | Two switching technologies: Circuit Switching and Packet Switching |
| 4 | Circuit Switching — 3 phases (establishment, transfer, disconnection) |
| 5 | Blocking vs. Non-blocking architecture |
| 6 | Requirements for circuit switching (switching capacity + channel capacity + routing intelligence) |
| 7 | Four types of circuit switching approaches |
| 8 | Space Division Switching — single stage and multi stage |
| 9 | Time Division Switching and Time Slot Interchange (TSI) |
| 10 | Properties and issues of circuit switching |
| 11 | Packet Switching — breaking data into packets, store-and-forward |
| 12 | Advantages of packet switching |
| 13 | Datagram vs. Virtual Circuit |
| 14 | Virtual Circuit Identifier (VCI) and routing tables |
| 15 | Virtual Circuit vs. Datagram — detailed comparison |
| 16 | Circuit Switching vs. Packet Switching — final comparison |
| 17 | Telephone network as circuit switching example |
| 18 | Course direction — focus on packet switching |
