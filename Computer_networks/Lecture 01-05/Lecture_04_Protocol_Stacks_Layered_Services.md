# Lecture 04 — Protocol Stacks – Layered Services

## Course: Computer Networks and Internet Protocol
### Prof. Soumya Kanti Ghosh, IIT Kharagpur

---

## What This Lecture Covers

This lecture gives you a **complete overview** of the services provided at each layer of the TCP/IP protocol stack. It also revisits key ideas like protocols, encapsulation, the OSI model, and how the TCP/IP stack compares to the OSI model. Think of this lecture as a "bird's-eye view" before we go deep into each layer in later lectures.

---

## Concept 1: What is a Protocol? (Recap)

### 📌 Concept Name
Protocol — Definition and Purpose

### 🧠 Simple Explanation

A **protocol** is a **set of rules** that tells different systems (computers, routers, phones, etc.) how to communicate with each other.

Imagine two people speaking different languages. They need a common language and a set of rules (greetings, turn-taking, goodbye) to have a conversation. In the same way, network devices need protocols to exchange data properly.

As per the transcript: *"Protocol defines the interface between the layers in the same system and with the layers of a peer system."*

So a protocol does **two things**:
1. It provides rules for communication **within the same machine** (between layers — like Application talking to Transport).
2. It provides rules for communication **with the same layer on the other machine** (peer-to-peer — like Transport layer on Machine A talking to Transport layer on Machine B).

### 🛠 Real-world Example (from transcript idea)

Think of the postal system. When you write a letter, you follow rules — you put it in an envelope, write the address in a specific format, and drop it in a letterbox. The postal system has its own rules for sorting and delivering. Both sender and receiver follow agreed-upon rules. That is a protocol.

### 🎯 Exam Important Points
- A protocol has **two interfaces**: (1) Service Interface — operations available at that protocol layer, (2) Peer-to-Peer Interface — how messages are exchanged between the same layer on two different machines.
- Protocols include specification of message format, message size, structure, and whether acknowledgement is expected.
- Standardization of protocols is guided by **IETF (Internet Engineering Task Force)**.

### ⚠️ Common Confusions
- Students confuse "service interface" with "peer-to-peer interface." Service interface = talking to the layer above/below in the same machine. Peer-to-peer interface = talking to the same layer on a remote machine.
- IETF is for internet standards. Don't confuse it with IEEE (which handles LAN standards like Ethernet).

---

## Concept 2: Key Elements of a Protocol

### 📌 Concept Name
Three Key Elements — Syntax, Semantics, Timing

### 🧠 Simple Explanation

Every protocol, no matter which layer, is built on **three key elements**:

**1. Syntax (Data Format)**
This defines the **structure or format** of the data. What does the message look like? How big is it? What fields does it have? What signal levels are used?

Think of it as the "grammar" of communication. Just like a letter has a greeting, body, and closing — a protocol message has specific fields in a specific order.

**2. Semantics (Meaning and Error Handling)**
This defines what the **control information** means and how **errors are handled**. Does the protocol have error-handling? If a packet is lost, what happens? Is there an acknowledgement mechanism? If not, then you simply resend after a timeout.

Think of it as the "meaning" behind the message.

**3. Timing (Synchronization and Sequencing)**
This deals with **speed matching** and **sequencing**.

- **Speed matching**: Devices work at different speeds. The protocol must make sure a fast sender doesn't overwhelm a slow receiver.
- **Sequencing**: In a packet-switched network, a message is broken into packets. These packets may travel different paths and arrive out of order. The protocol must have a mechanism (like sequence numbers) to put them back in the right order.

### 🛠 Real-world Example (from transcript idea)

Imagine you send an SMS with a reference number to check your exam result. The system expects a specific format (syntax). If your format is wrong, you get an error message (semantics — error handling). And if you don't get a reply within a certain time, you resend (timing).

### 🎯 Exam Important Points
- **Syntax** = Data format, signal levels.
- **Semantics** = Control information, error handling.
- **Timing** = Speed matching, synchronization, sequencing.
- In packet-switched networks, packets may not follow the same path — so sequencing is essential.
- Store-and-forward is the typical mechanism used to handle incoming and outgoing data streams.

### ⚠️ Common Confusions
- "Syntax" is NOT the content of the message — it is the structure/format.
- "Semantics" is NOT the data itself — it is about control logic and error handling.
- Sequencing is needed because in packet-switched networks, packets can arrive out of order.

---

## Concept 3: Protocol Interfaces — Service Interface and Peer-to-Peer Interface

### 📌 Concept Name
Two Interfaces of a Protocol Object

### 🧠 Simple Explanation

Every protocol has **two types of interfaces**:

**1. Service Interface**
This is the interface that a protocol exposes to the layer above it. It tells the upper layer: "Here are the services I can provide."

For example, the Transport layer provides a service interface to the Application layer — it says, "Give me your data, I will deliver it to the application on the other machine."

**2. Peer-to-Peer Interface**
This is the interface used to communicate with the **same layer on the remote machine**. This defines how messages are exchanged between peers — what format, what size, whether acknowledgement is expected, etc.

### 🛠 Real-world Example (from transcript idea)

The transcript uses the postal system analogy. You (the application) drop a letter at the post office counter (service interface). Then the post offices on both sides follow their own rules to carry your letter from IIT Kharagpur to Kolkata (peer-to-peer interface). At the destination, the post office gives the letter to the receiver through its service interface.

### 🎯 Exam Important Points
- Service interface = What services a protocol offers to the layer above.
- Peer-to-peer interface = How the same layer on two different machines communicates.
- The implementation includes prose, pseudo-code, and state transition diagrams.
- Interoperability is possible when two or more implementations follow the specification accurately.

### ⚠️ Common Confusions
- Service interface is vertical (between layers in the same machine). Peer-to-peer interface is horizontal (between same layers on different machines).

---

## Concept 4: Protocol Hierarchy and Layered Architecture

### 📌 Concept Name
Layered Architecture and Intermediate Devices

### 🧠 Simple Explanation

Protocols are arranged in a **hierarchy** — meaning they are stacked on top of each other. This is why we call it a **protocol stack**.

The beauty of this layered design is that **intermediate networking devices only need to understand up to the layer they operate on**:

| Device | Operates up to | What it does |
|--------|---------------|--------------|
| Hub / Repeater | Layer 1 (Physical) | Just regenerates and forwards the signal |
| Switch / Bridge | Layer 2 (Data Link) | Looks at frames and MAC addresses |
| Router | Layer 3 (Network) | Looks at packets and IP addresses for routing |
| End Host (Computer) | All layers (up to Layer 7 or Layer 5) | Runs complete application stack |

So a router opens the packet only up to the network layer. Everything above is just a "payload" to the router — it does not look inside.

### 🎯 Exam Important Points
- Devices are categorized as Layer 1, Layer 2, Layer 3 devices, etc.
- A Layer N device opens the packet only up to Layer N. The rest is payload.
- Hub/Repeater = Layer 1, Switch = Layer 2, Router = Layer 3.
- This layered design allows interoperability between heterogeneous devices.

### ⚠️ Common Confusions
- A "Layer 1 device" does NOT mean it only has Layer 1 — it means it only processes up to the physical layer (it does not look at MAC addresses or IP addresses).
- Students sometimes think a router looks at everything — it does NOT. It only looks up to the network layer.

---

## Concept 5: Encapsulation

### 📌 Concept Name
Encapsulation in the Protocol Stack

### 🧠 Simple Explanation

Encapsulation is one of the most important ideas in networking.

When data travels **down the protocol stack** at the sender's side, each layer **wraps** the data from the upper layer with its own header (and sometimes a trailer). This wrapping is called **encapsulation**.

Here is how it works step by step:

1. **Application Layer** generates the data (e.g., an email message).
2. **Transport Layer** takes that data, adds a Transport header (like TCP header) → this becomes a **segment** (or message).
3. **Network Layer** takes the segment, adds an IP header → this becomes a **packet**.
4. **Data Link Layer** takes the packet, adds a Data Link header (MAC address info) → this becomes a **frame**.
5. **Physical Layer** converts the frame into **raw bits** and transmits over the wire.

At the **receiver's side**, the process is reversed — each layer removes its header and passes the data upward. This is called **de-encapsulation** (or extraction).

The transcript says: *"High-level messages are encapsulated inside of low-level messages."*

### 🎯 Exam Important Points
- Data at each layer has a different name:
  - Application Layer → **Data / Message**
  - Transport Layer → **Segment (TCP) / Datagram (UDP)** or sometimes just **Message**
  - Network Layer → **Packet**
  - Data Link Layer → **Frame**
  - Physical Layer → **Bits**
- Encapsulation happens at the sender (going down). De-encapsulation happens at the receiver (going up).
- Intermediate devices only de-encapsulate up to their operating layer.

### ⚠️ Common Confusions
- Encapsulation does NOT change the original data — it only adds headers around it.
- A router removes the Data Link and Physical headers, reads the Network header for routing, and then re-encapsulates with new Data Link headers for the next hop.

---

## Concept 6: OSI Model — Quick Revisit

### 📌 Concept Name
OSI (Open System Interconnection) Model — 7 Layers

### 🧠 Simple Explanation

The **OSI model** has **7 layers**. This lecture briefly revisits it to compare with the TCP/IP model.

The 7 layers from bottom to top:

| Layer Number | Name | What it does |
|:---:|------|-------------|
| 1 | Physical | Transmits raw bits over the communication link |
| 2 | Data Link | Collects bits into frames; hop-to-hop delivery |
| 3 | Network | Routes packets from source to destination |
| 4 | Transport | Process-to-process communication |
| 5 | Session | Maintains sessions, ties transport streams together |
| 6 | Presentation | Handles data format exchange between peers |
| 7 | Application | End-user applications (FTP, HTTP, email, etc.) |

At intermediate hops (routers), only layers 1–3 are present. Layers 4–7 exist only on the end hosts.

### 🎯 Exam Important Points
- OSI = 7 layers. TCP/IP = 4 or 5 layers (depending on reference).
- The lower 3 layers (Physical, Data Link, Network) are implemented in **all network nodes** (including routers).
- Transport and above are only on **end hosts**.
- The layers above the network layer are considered a "payload" at intermediate nodes.

### ⚠️ Common Confusions
- OSI is a **reference model** — it is a theoretical framework. TCP/IP is the **practical implementation** used on the internet.
- Session and Presentation layers are part of OSI but are NOT separate layers in TCP/IP.

---

## Concept 7: Protocol Layers — Typical Functionalities

### 📌 Concept Name
What Each Layer Does (Detailed)

### 🧠 Simple Explanation

Let us go through each layer's functionality as described in the transcript:

### Layer 1: Physical Layer
- Handles **transmission of raw bits** over a communication link.
- Converts digital data into analog signals (or appropriate signal form) for transmission.
- Its job: get the bits from one end to the other end in an error-free manner so they can be reconstructed.
- It does NOT care about what the upper layers are doing.

### Layer 2: Data Link Layer
- Collects a **stream of bits into a larger aggregate called a frame**.
- So: bits → **frames**.
- Implemented by the **Network Interface Card (NIC)** along with the device driver in the OS.
- The NIC is the hardware card that connects your computer to the network (wired via RJ-45 or wireless via Wi-Fi).
- The NIC converts data to the appropriate signal level for the transmission medium (copper cable, fiber, wireless).
- Maintains **hop-to-hop connectivity** — meaning it is concerned only with getting the frame from one device to the very next device.
- Each NIC has a **MAC address (hardware address)** — this is the address used at the data link layer.

### Layer 3: Network Layer
- Handles **routing among nodes** within the network.
- Responsible for finding the **path (route)** from source to destination.
- The unit of data at this layer is called a **packet**.
- The naming convention: bits → frames → **packets**.
- Even though the network layer finds the overall path, for each hop, it has to come down to the data link layer (for hop-to-hop delivery) and then to the physical layer (for actual transmission).
- The lower 3 layers are typically implemented in **all network nodes**.

### Layer 4: Transport Layer
- Implements **process-to-process communication**.
- The network layer gets data from one machine to another. The transport layer goes further — it gets data from one **specific process (application)** on one machine to a **specific process** on another machine.
- The unit of data is called a **message**.
- Transport and higher layers run **only on end hosts**, not on intermediate switches or routers.

### Layer 5: Session Layer (OSI only)
- Provides a **namespace** that ties together different transport streams that are part of a single application.
- Think of it as maintaining a "session" — a name that links related communication together.

### Layer 6: Presentation Layer (OSI only)
- Concerned about the **format of data exchange** between peers.
- Decides what format the data should be in so both sides understand it.

### Layer 7: Application Layer
- Where the **end user interacts**.
- Standardized application types that use the network stack: like file transfer (FTP), web browsing (HTTP), email, remote login, etc.

### 🎯 Exam Important Points
- Physical Layer = raw bits. Data Link Layer = frames. Network Layer = packets. Transport Layer = messages.
- Data Link Layer uses **MAC address** (hardware address).
- Network Layer handles **routing** (finding the path).
- Transport Layer handles **process-to-process** communication (using port numbers).
- Lower 3 layers → present in ALL network nodes. Transport and above → only on END HOSTS.
- NIC (Network Interface Card) + device driver in OS → implement the data link layer.

### ⚠️ Common Confusions
- Network layer = machine to machine (using IP). Transport layer = process to process (using ports). Don't mix them.
- "Hop-to-hop" (data link) is NOT the same as "end-to-end" (transport). Hop-to-hop means just the next device. End-to-end means source to final destination.

---

## Concept 8: TCP/IP Protocol Stack vs. OSI

### 📌 Concept Name
TCP/IP Protocol Stack — 4 (or 5) Layer Model

### 🧠 Simple Explanation

The **TCP/IP protocol stack** is the real-world protocol stack used on the internet. Compared to the OSI model:

| OSI Model (7 Layers) | TCP/IP Model |
|---|---|
| Application | Application |
| Presentation | (merged into Application) |
| Session | (merged into Application) |
| Transport | Transport |
| Network | Internetwork (IP) |
| Data Link | Network Interface |
| Physical | (and Hardware) |

In some references, TCP/IP is shown as **4 layers** (Network Interface and Hardware are combined). In other references, it is shown as **5 layers** (Data Link and Physical are separated). Both are correct — the transcript says the course will use them interchangeably.

**Key protocols at each layer:**

| TCP/IP Layer | Predominant Protocols |
|---|---|
| Application | FTP, HTTP, SMTP, Telnet, DNS, etc. |
| Transport | **TCP** (connection-oriented, reliable) and **UDP** (connectionless, unreliable, best-effort) |
| Internetwork | **IP** (the most important), **ICMP**, **IGMP**, **ARP**, **RARP** |
| Network Interface | IEEE 802.2, 802.3 (Ethernet), X.25, ATM, FDDI, SNA |

### 🎯 Exam Important Points
- TCP/IP has **4 or 5 layers** (not 7 like OSI).
- OSI's Session and Presentation layers are merged into the Application layer in TCP/IP.
- The dominant protocol at the Internetwork layer is **IP**.
- Companion protocols at the Internetwork layer: ICMP, IGMP, ARP, RARP.
- ARP and RARP help interface with the lower layers (data link layer).

### ⚠️ Common Confusions
- TCP/IP is NOT a 7-layer model. It is 4 or 5 layers.
- ICMP is NOT a transport layer protocol — it works alongside IP at the network/internetwork layer.
- ARP/RARP work between the network layer and the data link layer.

---

## Concept 9: TCP vs. UDP

### 📌 Concept Name
Two Main Transport Layer Protocols — TCP and UDP

### 🧠 Simple Explanation

The Transport layer has two major protocols:

**TCP (Transmission Control Protocol)**
- **Connection-oriented**: A connection is established before data transfer begins.
- **Reliable**: It ensures data arrives correctly — provides error recovery, duplicate suppression, congestion control, and flow control.
- Used when you cannot afford to lose data (file transfer, web pages, emails).

**UDP (User Datagram Protocol)**
- **Connectionless**: No connection is established. Data is just sent.
- **Unreliable** / **Best-effort**: No guarantee that data will arrive. No error recovery, no flow control.
- If you need reliability with UDP, the upper layer (application) must handle it.
- Used for applications that need **fast transport** and can **tolerate some data loss** (e.g., video streaming, online gaming, DNS queries).

### 🎯 Exam Important Points
- TCP = connection-oriented, reliable, error recovery, flow control, congestion control, duplicate suppression.
- UDP = connectionless, unreliable, best-effort, fast.
- If UDP is used and reliability is needed, it must be handled at the application layer.
- Multiple applications can operate simultaneously (concurrent applications) using different port numbers.

### ⚠️ Common Confusions
- "Unreliable" does NOT mean UDP is bad. It simply means UDP does not provide built-in reliability — it is faster because of this.
- TCP is not always better. For real-time applications where speed matters more than perfect delivery, UDP is preferred.

---

## Concept 10: The IP Protocol (Internetwork Layer)

### 📌 Concept Name
IP — The Most Important Protocol at the Network Layer

### 🧠 Simple Explanation

The **IP (Internet Protocol)** is the most important protocol at the internetwork (network) layer. Its main job is to **route packets from one network to another**.

Key characteristics of IP:
- It is **connectionless** — each packet is handled independently.
- It provides **best-effort service** — no guarantee of delivery.
- It does NOT provide reliability, flow control, or error recovery.
- If those functions are needed, they must be provided by **higher layers** (like TCP at the transport layer).

Other protocols at this layer:
- **ICMP** (Internet Control Message Protocol) — companion protocol to IP.
- **IGMP** (Internet Group Management Protocol).
- **ARP** (Address Resolution Protocol) — maps IP addresses to MAC addresses.
- **RARP** (Reverse ARP) — maps MAC addresses to IP addresses.

### 🎯 Exam Important Points
- IP is connectionless and best-effort — no reliability, no flow control, no error recovery.
- If reliability is needed, it is provided by TCP at the transport layer.
- ARP and RARP help bridge the gap between IP addresses (network layer) and MAC addresses (data link layer).
- IP is the "narrow waist" of the internet — everything goes through IP.

### ⚠️ Common Confusions
- IP does NOT guarantee delivery. It just tries its best (best-effort). TCP adds the reliability on top of IP.
- ARP is NOT a transport protocol. It works at the boundary of network and data link layers.

---

## Concept 11: Network Interface Layer (Data Link + Physical)

### 📌 Concept Name
Network Interface Layer Protocols

### 🧠 Simple Explanation

This is the lowest layer of the TCP/IP stack. It combines the **Data Link Layer** and the **Physical Layer**.

Its job is to take frames from the network layer and push them as **bits** into the physical communication medium (cable, fiber, wireless).

Predominant protocols here include: **IEEE 802.2, IEEE 802.3 (Ethernet), X.25, ATM, FDDI, SNA**.

There is an underlying communication path (the actual wire, fiber, or radio wave) that carries the bits from one point to another.

### 🎯 Exam Important Points
- IEEE 802.3 = Ethernet (very important for exams).
- This layer deals with frames and bits.
- The physical medium (copper, fiber, wireless) lies beneath this layer.

---

## Concept 12: The Internet Architecture and the Hourglass Model

### 📌 Concept Name
Hourglass Shape of the Internet Architecture

### 🧠 Simple Explanation

The internet architecture, as defined by **IETF**, does NOT imply **strict layering**. Applications are free to bypass the transport layer and directly use IP if needed.

The transcript describes the overall shape of the internet protocol stack as an **hourglass**:
- **Wide at the top** — many applications (HTTP, FTP, SMTP, DNS, etc.)
- **Narrow in the middle** — everything funnels through IP
- **Wide at the bottom** — many different network technologies (Ethernet, Wi-Fi, fiber, etc.)

This means IP is the single protocol that everything must go through, making it the most critical protocol in the entire stack.

For a new protocol to be officially included in the internet architecture, there needs to be a **protocol specification** and at least **one (preferably two) working implementations**.

### 🎯 Exam Important Points
- Internet architecture = hourglass shape.
- IP is at the "narrow waist" — all traffic passes through IP.
- IETF is not very strict about layering — applications can bypass transport and directly use IP.
- New protocol inclusion requires: specification + at least 1–2 implementations.

### ⚠️ Common Confusions
- "Not strict layering" does NOT mean layers are unimportant. It means the architecture allows flexibility.

---

## Concept 13: Application Programming Interface (API) and Sockets

### 📌 Concept Name
Network API and Socket

### 🧠 Simple Explanation

The higher layers of the protocol stack are implemented in **software** as part of the **operating system**. The interface that applications use to access network services is called the **Network API (Application Programming Interface)**.

The API allows applications to send and receive data over the network.

An application is identified by two things:
1. **IP address** — which machine the application is running on.
2. **Port number** — which specific application/process on that machine.

The combination of IP address + Port number gives us a **socket**. A socket is the endpoint of communication. It allows two applications to connect and exchange data.

The transcript mentions that socket programming will be covered in detail in later lectures.

### 🎯 Exam Important Points
- Network protocols in higher layers are implemented in software as part of the OS.
- Network API = the interface applications use to access network services.
- Socket = IP address + Port number → identifies a specific application endpoint.
- Socket allows application-to-application communication.

### ⚠️ Common Confusions
- A socket is NOT just an IP address. It is IP address + Port number.
- API is not a protocol — it is an interface for programmers to use protocol services.

---

## Concept 14: How Communication Works End-to-End (Big Picture)

### 📌 Concept Name
End-to-End Communication Through the TCP/IP Stack

### 🧠 Simple Explanation

Let us put it all together. When two computers communicate across the internet, here is what happens:

**At the Sender:**
1. The **Application** (e.g., FTP client) generates data.
2. The **Transport layer** (TCP or UDP) takes the data and adds transport header → segment/datagram.
3. The **IP layer** adds IP header and determines the route → packet.
4. The **Data Link layer** adds MAC addresses → frame.
5. The **Physical layer** converts to bits and transmits.

**At each Router (intermediate hop):**
- Opens packet only up to the Network layer (Layer 3).
- Reads the destination IP, decides the next hop.
- Re-encapsulates with new Data Link headers for the next hop.
- Sends it to the physical layer for transmission.

**At the Receiver:**
- The process reverses — each layer strips its header and passes data upward.
- Finally, the Application (e.g., FTP server) receives the original data.

The key point: **Peer layers understand each other.** FTP client understands FTP server. TCP understands TCP. IP understands IP. The communication between peer layers is transparent to other layers.

### 🎯 Exam Important Points
- Communication is peer-to-peer at each layer — application talks to application, transport talks to transport, etc.
- Intermediate devices (routers) only operate up to the network layer.
- The process is transparent — each layer does its job without worrying about what other layers do.
- Transport Service Access Point (TSAP) is used to access transport layer services.

---

## Concept 15: What is TSAP and Network Access?

### 📌 Concept Name
Transport Service Access Point (TSAP)

### 🧠 Simple Explanation

The transcript briefly mentions that applications connect through a **Transport Service Access Point (TSAP)** to access the transport layer, and there is a **Network Access** point to connect to the network.

In simple terms:
- TSAP is the point where the application "plugs into" the transport layer. The port number acts as a TSAP.
- Network Access is the point where the transport layer connects to the underlying network (IP layer and below).

This creates the full chain: Application → TSAP → Transport → Network Access → IP → Data Link → Physical → Communication medium.

### 🎯 Exam Important Points
- TSAP = port number (identifies the application at the transport layer).
- This creates a full pipeline from application to physical medium.

---

## Summary Table — Lecture 4 at a Glance

| Concept | Key Point |
|---|---|
| Protocol | Set of rules for communication; guided by IETF |
| Protocol Elements | Syntax, Semantics, Timing |
| Protocol Interfaces | Service Interface (vertical) and Peer-to-Peer Interface (horizontal) |
| Layered Architecture | Each device operates only up to its designated layer |
| Encapsulation | Each layer adds its own header as data moves down the stack |
| OSI Model | 7 layers: Physical, Data Link, Network, Transport, Session, Presentation, Application |
| TCP/IP Model | 4 or 5 layers; Session and Presentation merged into Application |
| Physical Layer | Transmits raw bits |
| Data Link Layer | Frames; MAC address; hop-to-hop; NIC |
| Network Layer | Packets; routing; IP address |
| Transport Layer | Messages; process-to-process; port numbers; TCP or UDP |
| TCP | Connection-oriented, reliable, flow control, congestion control |
| UDP | Connectionless, unreliable, best-effort, fast |
| IP | Connectionless, best-effort; most important protocol at network layer |
| Companion Protocols | ICMP, IGMP, ARP, RARP |
| Hourglass Model | Wide top (apps), narrow middle (IP), wide bottom (networks) |
| Socket | IP address + Port number = communication endpoint |
| Network API | Software interface to access protocol services |

---

## 10 MCQs — Strictly from Lecture 4

---

**Q1. What are the three key elements of a protocol?**

A) Syntax, Semantics, Timing
B) Syntax, Security, Sequencing
C) Format, Encryption, Routing
D) Encapsulation, Decapsulation, Routing

**Answer: A**
**Explanation:** The transcript clearly states that the three key elements of any protocol are Syntax (data format, signal levels), Semantics (control information, error handling), and Timing (speed matching, sequencing, synchronization).

---

**Q2. Which organization guides the standardization of internet protocols?**

A) IEEE
B) ISO
C) IETF
D) ITU

**Answer: C**
**Explanation:** The transcript says standardization is guided by IETF (Internet Engineering Task Force). IEEE handles LAN standards (like Ethernet 802.3), but overall internet protocol standardization is IETF's role.

---

**Q3. What does the Data Link Layer collect bits into?**

A) Packets
B) Segments
C) Frames
D) Messages

**Answer: C**
**Explanation:** The transcript states that the data link layer "collects a stream of bits into a larger aggregate called frame." So the data unit at the data link layer is a frame.

---

**Q4. Up to which layer does a router operate?**

A) Physical Layer
B) Data Link Layer
C) Network Layer
D) Transport Layer

**Answer: C**
**Explanation:** The transcript says a router has a network layer and can look at packets at the level of the network layer. Everything above the network layer is payload to the router.

---

**Q5. Which of the following is a connectionless, unreliable, best-effort transport protocol?**

A) TCP
B) IP
C) UDP
D) FTP

**Answer: C**
**Explanation:** The transcript describes UDP as "connectionless, unreliable, best-effort service." TCP is connection-oriented and reliable. IP is a network layer protocol, not transport. FTP is an application layer protocol.

---

**Q6. What is the shape of the internet protocol architecture described in the transcript?**

A) Diamond
B) Pyramid
C) Hourglass
D) Rectangle

**Answer: C**
**Explanation:** The transcript describes the internet architecture as "hourglass" — wide at the top (many applications), narrow in the middle (IP), and wide at the bottom (many network technologies).

---

**Q7. A socket is a combination of which two things?**

A) MAC address + Port number
B) IP address + Port number
C) IP address + MAC address
D) Protocol + Port number

**Answer: B**
**Explanation:** The transcript says an application is identified by the IP address where it is running and the port number. This combination is called a socket.

---

**Q8. Which layer is responsible for process-to-process communication?**

A) Network Layer
B) Data Link Layer
C) Transport Layer
D) Application Layer

**Answer: C**
**Explanation:** The transcript clearly states that "the transport layer implements process-to-process communication." The network layer handles routing between networks, not between processes.

---

**Q9. Which of the following is NOT a protocol at the Internetwork (IP) layer?**

A) ICMP
B) ARP
C) RARP
D) TCP

**Answer: D**
**Explanation:** The transcript lists IP, ICMP, IGMP, ARP, and RARP as protocols at the internetwork layer. TCP is a transport layer protocol, not an internetwork/network layer protocol.

---

**Q10. What type of address is associated with the Data Link Layer?**

A) IP address
B) Port number
C) MAC address (hardware address)
D) Socket address

**Answer: C**
**Explanation:** The transcript says the data link layer has a "hardware address or MAC address." IP addresses are used at the network layer, port numbers at the transport layer, and socket addresses are a combination of IP + port.

---

*End of Lecture 4 — Complete Notes and MCQs*
*Next: Lecture 5 will go deeper into individual layers and their protocols.*
