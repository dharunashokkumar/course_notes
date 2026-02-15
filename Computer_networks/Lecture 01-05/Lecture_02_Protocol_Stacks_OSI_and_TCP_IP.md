# Lecture 02: Protocol Stacks – OSI and TCP/IP

**Course:** Computer Networks and Internet Protocol  
**Instructor:** Prof. Soumya Kanti Ghosh, IIT Kharagpur  

---

## Topics Covered in This Lecture

1. Brief History of Internet  
2. What is a Protocol?  
3. Protocol Specification  
4. OSI Model (7 Layers)  
5. TCP/IP Model (5 Layers or 4 Layers)  
6. Comparison of OSI and TCP/IP  
7. Protocols at Different Layers of TCP/IP  
8. Encapsulation (Payload and Header Concept)  
9. LAN and WAN  
10. Network Devices: NIC, Repeater, Hub, Bridge, Switch, Router  
11. Broadcast Domain and Collision Domain  
12. Hierarchical Network Design (Core, Distribution, Access)  

---

## Concept 1: Brief History of the Internet

📌 **Concept Name:** Evolution of Internet / History of Networking

🧠 **Simple Explanation:**

The Internet did not appear suddenly. It grew step by step over many decades. Here is the timeline as described in the transcript:

**1836 – Telegraph:** The very first long-distance electrical communication. Morse Code was used, which is a series of dots and dashes. The transcript draws an analogy — dots and dashes are somewhat similar to today's binary system of 0s and 1s.

**1858–1866 – Transatlantic Cable:** Cables were laid under the ocean (submarine cables) connecting continents. Even today, submarine cables form the major backbone of global data communication.

**Telephone Lines:** These revolutionized how people connect. Many early computer networks used existing telephone lines as a physical layer to send data.

**~1950 – ARPA (Advanced Research Projects Agency):** The U.S. formed ARPA under the Department of Defense. Around the same time, the USSR launched Sputnik. There were parallel efforts driven partly by this competition.

**1962 – ARPANET:** The goal was to create a network that would allow resilient and reliable connectivity during extreme situations (like war). This was a key motivation.

**1960s – Packet Switching Developed:** Data is split into small packets. These packets may take different routes to reach the destination. At the destination, all packets are reassembled. This was a very important development.

**1969 – ARPANET Commissioned by DoD:** Four major US universities participated — UCLA, Stanford Research Institute, UCSB, and University of Utah. This gave the network an open research flavor beyond just military use.

**1971 – First Email:** The first email program was developed. The first message was the first row of the QWERTY keyboard. At that time, ARPANET had about 15 nodes and 23 hosts.

**1973 – Global Networking:** Connections reached England and Norway — networking was crossing countries and continents.

**1974 – TCP (Transmission Control Protocol):** The evolution of TCP and other protocols began.

**1977 – Email Grows:** More than 100 hosts were communicating via email, with a steep increase afterward.

**1979 – Newsgroups:** Online discussion groups formed.

**1982 – TCP/IP Proposed for ARPANET:** A major milestone protocol stack.

**1983 – Name Servers Developed:** Since remembering IP addresses in numbers was difficult, name servers were created to map names to IP addresses.

**1984 – DNS (Domain Name System):** DNS came into play. Number of hosts crossed 1,000.

**1987 – 30,000+ Hosts.**

**1989 – WWW (World Wide Web):** The concept of "www" was coined by Tim Berners-Lee.

**1990 – First Search Engine:** Number of hosts exceeded 300,000. Around 1,000 newsgroups. ARPANET officially ceased to exist and moved to a distributed development mode.

**1991 – Gopher:** A text-based, menu-driven interface for accessing Internet resources.

**1992 – Multimedia & "Surfing the Net":** The term "surfing the net" was coined.

**1993 – WWW Revolution Begins:** Number of hosts crossed millions. Mosaic web browser was launched. After this, there was phenomenal growth in applications and content.

**Other Milestones:**
- 1994 – Hotmail, W3C (World Wide Web Consortium) founded
- 1995 – JAVA source code released
- 1996 – ICQ application (from Israel)
- 1998 – Google founded

**Web Explosion Numbers (from transcript):**
- 1984/94 → ~3.2 million
- 1995 → 6.4 million
- 1997 → 19.5 million
- 2001 → 110 million hosts, over 30 million websites
- Growth is exponential / expansive

🎯 **Exam Important Points:**
- ARPANET was commissioned in 1969 with 4 US universities
- Packet switching was developed in the 1960s
- TCP/IP was proposed for ARPANET in 1982
- DNS came in 1984
- WWW was coined by Tim Berners-Lee in 1989
- First email was in 1971
- ARPA was under the U.S. Department of Defense

⚠️ **Common Confusions:**
- ARPANET ≠ Internet. ARPANET was the predecessor that eventually evolved into the Internet.
- DNS and Name Servers are related but DNS specifically came in 1984.
- WWW (1989) is not the same as the first email (1971) — WWW is about web pages, email is a separate application.

---

## Concept 2: The Need for Networks and Applications

📌 **Concept Name:** Why Networks Are So Important

🧠 **Simple Explanation:**

As per the transcript, today we have a huge volume of applications on the Internet. Some applications are sensitive to errors (they need accuracy), some are sensitive to time (they need speed), and some need to handle large amounts of data.

These applications span everything from day-to-day life to scientific applications. With the rise of services like cloud computing and high-performance computing, the network backbone plays an extremely important role.

The transcript makes a strong point: "Any interruption of the network will not only make it difficult to communicate, but several industrial processes will come to a standstill." This tells us how critical networks are.

Also, the design of information systems is moving towards a service-oriented architecture, where the network is a major component.

🎯 **Exam Important Points:**
- Different applications have different requirements — some need accuracy, some need speed, some need more bandwidth
- Network interruption can halt industrial processes
- Cloud and high-performance computing depend heavily on networks

---

## Concept 3: Protocol Stack Recap (5 Layers)

📌 **Concept Name:** The Five-Layer Protocol Stack

🧠 **Simple Explanation:**

The transcript recaps the five layers of the protocol stack (from bottom to top):

1. **Physical Layer** — Handles the physical communication (wired, wireless) between two nodes
2. **Data Link Layer** — Handles framing, filtering at MAC layer level
3. **Network Layer** — Handles packet delivery and routing (unreliable delivery — reliability is not guaranteed at this layer)
4. **Transport Layer** — Provides process-to-process communication
5. **Application Layer** — Where the end-user applications operate

**Key Idea — Not all devices need all layers.** A Layer 2 switch only needs up to Layer 2. A router (Layer 3 device) needs up to Layer 3. But any higher-layer device also has all the properties of the lower layers. For example, a router also does data link layer filtering and physical layer communication.

**Popular Protocols at Each Layer (from transcript):**
- Application Layer: HTTP, FTP, SMTP, and various web services that piggyback on these
- Transport Layer: TCP, UDP, RTP (connection-oriented, connectionless, real-time)
- Network Layer: IPv4, IPv6, MPLS
- Data Link Layer: Ethernet, Wi-Fi, Bluetooth, UMTS, LTE
- Physical Layer: Physical connectivity, communication technologies

**Cross-Layer Protocols:** Some protocols exist between two layers (they interface between layers). Examples: DNS (Domain Name System), SNMP (Simple Network Management Protocol), ARP (Address Resolution Protocol), DHCP (Dynamic Host Configuration Protocol).

🎯 **Exam Important Points:**
- 5 layers: Physical → Data Link → Network → Transport → Application
- Network layer provides unreliable delivery
- Transport layer provides both reliable and unreliable delivery
- HTTP, FTP, SMTP are application layer protocols
- TCP, UDP are transport layer protocols
- IPv4, IPv6 are network layer protocols
- Ethernet is a data link layer protocol
- DNS, ARP, DHCP are cross-layer protocols

⚠️ **Common Confusions:**
- "Network layer is unreliable" does NOT mean it does not work. It means it does not guarantee delivery — packets may be lost, and the network layer will not try to fix that on its own.
- Transport layer sits ABOVE the unreliable network layer but can still provide reliable service (using TCP).

---

## Concept 4: What is a Protocol?

📌 **Concept Name:** Definition of Protocol

🧠 **Simple Explanation:**

As defined in the transcript:

> A protocol is a **controlled sequence of messages** that is exchanged between two or more systems to accomplish a given task.

When you do FTP communication, SSH, HTTP, DNS resolution, or DHCP, you are exchanging a controlled set of messages between two or more parties to accomplish a specific task. That task could be downloading a document, resolving an IP address, configuring a network address, and so on.

🛠 **Real-world Analogy (from transcript context):** When you type a website address in your browser, your browser (HTTP client) sends a request to the HTTP server. This exchange of requests and responses follows a defined protocol (HTTP).

🎯 **Exam Important Points:**
- Protocol = controlled sequence of messages between 2 or more systems to accomplish a task
- This is a core definition — remember it exactly

---

## Concept 5: Protocol Specification

📌 **Concept Name:** What is a Protocol Specification?

🧠 **Simple Explanation:**

The transcript says:

> Protocol specifications define the **sequence** together with the **format or layout** of the messages that are exchanged.

So, a protocol specification tells you two things:
1. **What messages** to send, in **what order** (the sequence)
2. **What is the format** of each message — what fields are there, what is the size, how the message is structured

Because both sides know the specification, the receiver can properly read and understand (decipher) the message. For example, when a DHCP packet arrives at a DHCP receiver, the receiver knows exactly how to open it and read the fields because both sides follow the same specification.

This is what makes **inter-operation** possible. If I say "I am following IEEE protocol X," then the other end only needs to know the specification — no separate explanation is needed.

The transcript also notes: In distributed, loosely-coupled, and autonomous systems, protocols play a very important role in making communication possible.

🎯 **Exam Important Points:**
- Protocol specification = sequence + format/layout of messages
- It enables inter-operation between different devices
- Both sender and receiver follow the same specification to communicate
- Important in distributed, loosely-coupled, autonomous systems

---

## Concept 6: OSI Model (7 Layers)

📌 **Concept Name:** OSI (Open System Interconnection) Model

🧠 **Simple Explanation:**

The OSI model was a major effort to define networking protocols in a specific way. It has **7 layers** (compared to TCP/IP's 5 layers).

The 7 layers from bottom to top:

| Layer # | Name | Function (from transcript) |
|---------|------|---------------------------|
| 1 | Physical | Transmission of binary data over a media |
| 2 | Data Link | Transfer of units of information, framing, and error checking |
| 3 | Network | Delivery of packets of information including routing (unreliable — reliability NOT guaranteed) |
| 4 | Transport | Provision of end-to-end reliable AND unreliable delivery |
| 5 | Session | Establishes and maintains a session |
| 6 | Presentation | Data formatting and encryption |
| 7 | Application | Network applications like file transfer, terminal emulation, etc. |

**Important Point from Transcript:** Transport layer sits over the network layer, yet it can provide reliable service even though the network layer below it is unreliable. It supports both reliable and unreliable protocols.

**Not all devices need all layers:** A Layer 2 switch is enabled up to Layer 2 only. A Layer 3 switch is enabled up to Layer 3. Some devices have all layers.

**Encapsulation:** The application layer data becomes a payload for the presentation layer. The presentation layer output becomes a payload for the session layer, and so on. Finally, everything is carried over the physical layer to the other end.

🎯 **Exam Important Points:**
- OSI = 7 layers (Physical, Data Link, Network, Transport, Session, Presentation, Application)
- OSI stands for Open System Interconnection
- Session layer → establishes and maintains sessions
- Presentation layer → data formatting and encryption
- Network layer → unreliable delivery
- Transport layer → both reliable and unreliable
- Each layer's output becomes the payload of the layer below it (encapsulation)

⚠️ **Common Confusions:**
- OSI has 7 layers, TCP/IP has 5 (or sometimes 4). Don't mix up the count.
- Session and Presentation layers exist in OSI but are merged into other layers in TCP/IP.

---

## Concept 7: TCP/IP Model

📌 **Concept Name:** TCP/IP Protocol Suite

🧠 **Simple Explanation:**

TCP/IP is one of the most prominent protocol suites. It has been used for a long time and is the dominant standard for inter-networking.

As per the transcript: "TCP/IP presents a set of public standards that specify how packets of information are exchanged between computers of one or more networks." It is not limited to one network — it can work between any two systems across any two networks.

**TCP/IP Layers:**
- In most descriptions, TCP/IP is a **5-layer stack**: Physical, Data Link, Network, Transport, Application
- In some places, it is considered a **4-layer stack** where Physical and Data Link are combined into one layer

**How TCP/IP maps to OSI:**
- TCP/IP's Transport layer covers a bit of OSI's Session layer + Transport layer
- TCP/IP's Application layer covers OSI's Presentation + Application + a bit of Session
- Nothing is left out — the functionality is merged

🎯 **Exam Important Points:**
- TCP/IP is the dominant standard for inter-networking
- TCP/IP = 5 layers (or sometimes 4 layers where physical + data link = one layer)
- TCP/IP is a set of public standards
- TCP/IP works across multiple networks, not just one

---

## Concept 8: Protocols at Different Layers of TCP/IP

📌 **Concept Name:** Protocols Supported by TCP/IP Layers

🧠 **Simple Explanation (from transcript):**

| Layer | Protocols Mentioned |
|-------|-------------------|
| Physical (Layer 1) | Fiber optics, UTP, Coax, Microwave, Satellite, STP |
| Data Link (Layer 2) | Ethernet (IEEE 802.3), X.25, Token Ring, Frame Relay |
| Network (Layer 3) | IPv4, IPv6 (Networking in TCP/IP is over IP) |
| Transport (Layer 4) | TCP, UDP, ICMP |
| Application (Layer 5) | HTTP, FTP, SMTP, and a big bunch of other protocols |

There are also **cross-layer (intermediate) protocols** that work between different layers: DNS, SNMP, ARP, DHCP.

🎯 **Exam Important Points:**
- In TCP/IP, the network layer uses IP (IPv4 or IPv6)
- Ethernet follows IEEE 802.3 standard
- ICMP is listed at the transport level in this transcript
- Cross-layer protocols: DNS, ARP, DHCP, SNMP

---

## Concept 9: Encapsulation (Payload Concept)

📌 **Concept Name:** How Data Moves Down the Layers (Encapsulation)

🧠 **Simple Explanation:**

When data is sent from one device to another, it passes through each layer from top to bottom. At each layer, the data from the upper layer becomes the **payload** (the cargo) for the current layer. The current layer adds its own **header** to this payload.

Step by step:
1. Application layer produces data → this becomes payload for Transport layer
2. Transport layer adds its header → this whole thing becomes payload for Network layer
3. Network layer adds its header → becomes payload for Data Link layer
4. Data Link layer adds its header → becomes payload for Physical layer
5. Physical layer transmits everything through the underlying physical channel

At the receiving end, the reverse happens — each layer removes its header and passes the payload up to the next layer.

🛠 **Think of it like putting a letter in envelopes:** Your letter (data) goes into a small envelope (transport header). That goes into a medium envelope (network header). That goes into a big envelope (data link header). Finally, the post office (physical layer) delivers the big envelope.

🎯 **Exam Important Points:**
- Each layer's output = payload + header of that layer
- The output of one layer becomes the payload of the layer below
- This process is called encapsulation
- At physical layer, the entire data is transmitted through the physical channel

---

## Concept 10: LAN and WAN

📌 **Concept Name:** Local Area Network (LAN) vs Wide Area Network (WAN)

🧠 **Simple Explanation:**

**LAN (Local Area Network):**
A network that covers a small geographic area. The transcript describes a typical LAN having clients, servers, and network devices (repeaters, hubs, transceivers, NICs, bridges, switches, routers).

Example from transcript: An FTP server and FTP client, or an HTTP server and HTTP client (your browser is the HTTP client; the website's server is the HTTP server).

**WAN (Wide Area Network):**
As per the transcript: "A WAN is a data communication network covering a large geographic span." Unlike LAN, a WAN connection is generally rented from a service provider. WANs connect various sites at different geographic locations so that information can be exchanged.

**Key Difference:** The way of handling may differ between LAN and WAN, but the devices and the way protocols work remain the same.

🎯 **Exam Important Points:**
- LAN = small area; WAN = large geographic area
- WAN connections are typically rented from a service provider
- Protocols and devices work the same way in both LAN and WAN

---

## Concept 11: Network Devices and Their Layers

📌 **Concept Name:** Network Devices at Different OSI Layers

🧠 **Simple Explanation:**

The transcript describes several network devices and which layer they operate at:

### Layer 1 Devices (Physical Layer)

**Repeater:**
- If two computers are connected using a cable, the maximum typical distance is about 100 meters.
- Beyond that, the signal weakens.
- A repeater amplifies the signal so data can travel farther.
- It is a Layer 1 device — it only deals with the physical signal.

**Hub (Multi-port Repeater):**
- If you want to connect more than two devices, you need a hub.
- A hub is basically a multi-port repeater.
- It is also a Layer 1 device.
- **Problem with hubs:** Hubs share bandwidth between all attached devices. If it is a 10 Mbps hub with 8 ports, the effective bandwidth per device in the worst case is 10/8 Mbps.
- Hubs cannot filter traffic.
- Most LANs using hubs work on broadcast topology — every device sees every other device's traffic.
- This causes many collisions.

### Layer 2 Devices (Data Link Layer)

**NIC (Network Interface Card):**
- Every laptop, PC, etc., has a NIC.
- It is a Layer 2 device.
- It has a **MAC address** (also called hardware address) — a unique address given by the manufacturer (OEM).
- To create the simplest LAN (connect two computers), you connect them with a **crossover cable** — where the transmitter (TX) of one goes to the receiver (RX) of the other, and vice versa.

**Bridge:**
- A bridge filters traffic based on MAC address.
- Since each NIC has a unique MAC address, the bridge can decide which traffic goes where.
- When you use bridges, traffic is **localized** — it does not cross to other segments unnecessarily, saving bandwidth.

**Switch (Layer 2 Switch):**
- A multi-port bridge is called a **switch** (specifically, a Layer 2 switch).
- Each port on a switch is its own collision domain.
- Switches break up collision domains.

### Layer 3 Devices (Network Layer)

**Router:**
- If two networks are **different**, a router is needed.
- Routers filter traffic based on **IP address** (not MAC address).
- The IP address tells the router which LAN segment a packet belongs to and where to send it.
- Routers not only divide collision domains but also divide **broadcast domains**.
- Routers help in inter-networking (connecting different networks).

### Key Principle from Transcript:

> "Any higher layer device has all the properties of the lower layers."

So a router (Layer 3) can also do Layer 2 (data link) filtering and Layer 1 (physical layer) communication.

🎯 **Exam Important Points:**
- Repeater = Layer 1, amplifies signal
- Hub = Layer 1, multi-port repeater, shares bandwidth, cannot filter traffic
- NIC = Layer 2, has unique MAC address
- Bridge = Layer 2, filters traffic based on MAC address
- Switch (Layer 2) = multi-port bridge, each port is its own collision domain
- Router = Layer 3, filters based on IP address, breaks both collision and broadcast domains
- Higher layer devices have all lower layer properties
- Two computers → crossover cable; more than two → hub or switch

⚠️ **Common Confusions:**
- Hub vs Switch: Hub is Layer 1, shares bandwidth, all devices see all traffic. Switch is Layer 2, filters based on MAC, each port is a separate collision domain.
- Bridge vs Switch: A switch is essentially a multi-port bridge.
- Switch vs Router: Switch works on MAC address (Layer 2), Router works on IP address (Layer 3).
- Hub shares bandwidth; switch does NOT share bandwidth in the same way.

---

## Concept 12: Collision Domain

📌 **Concept Name:** What is a Collision Domain?

🧠 **Simple Explanation:**

From the transcript:

> A collision domain is a network scenario in which one particular device sends a packet to the network segment, forcing other devices in the same segment to pay attention to it. At the same time, if a different device also sends, there will be a **collision**, which causes **loss of data**, **re-transmission**, and **loss of bandwidth**.

Collision domains are typically found at **Layer 1 (hubs)**. A hub represents **one collision domain** — all ports share the same collision domain.

A **switch** breaks up collision domains: **each port on a switch is its own collision domain**.

🎯 **Exam Important Points:**
- Collision domain = a segment where a collision can happen if two devices send at the same time
- Hub = one collision domain (all ports)
- Switch = each port is its own collision domain
- Collision causes data loss and re-transmission, wasting bandwidth

---

## Concept 13: Broadcast Domain

📌 **Concept Name:** What is a Broadcast Domain?

🧠 **Simple Explanation:**

From the transcript:

> A broadcast domain is the set of all devices on a network segment that hear all the broadcasts sent to that segment.

**Why breaking up broadcast domains is important:** When a host or server sends a network broadcast, **every device** in that broadcast domain must read and process that broadcast (it may reject or accept it based on whether it is meant for that device). This wastes processing power and bandwidth.

**Routers break up broadcast domains by default.** When a router interface receives a broadcast, it **discards** the broadcast without forwarding it to the other network. Routers also break up collision domains.

**Switches do NOT break up broadcast domains** — they only break up collision domains.

🎯 **Exam Important Points:**
- Broadcast domain = all devices that hear a broadcast on a segment
- Routers break both broadcast domains AND collision domains
- Switches break only collision domains, NOT broadcast domains
- When a router receives a broadcast, it discards it (does not forward)

⚠️ **Common Confusions:**
- Switches break collision domains but NOT broadcast domains
- Routers break BOTH collision and broadcast domains
- This is a very common exam question — do not mix them up

---

## Concept 14: Switches vs Routers (Summary from Transcript)

📌 **Concept Name:** Role of Switches and Routers

🧠 **Simple Explanation:**

The transcript makes a clear distinction:

**Switches:**
- NOT used to create inter-networking (that is the router's job)
- Used to add functionality to the LAN
- Break up collision domains
- Switch frames from one port to another within a switched network

**Routers:**
- Used for inter-networking (connecting different networks)
- Filter traffic based on IP address
- Break up both broadcast domains and collision domains
- Layer 3 devices

🎯 **Exam Important Points:**
- Switches = LAN functionality, collision domain separation
- Routers = inter-networking, broadcast + collision domain separation
- Switches switch frames; routers route packets

---

## Concept 15: Hierarchical Network Design

📌 **Concept Name:** Core, Distribution, and Access Layers

🧠 **Simple Explanation:**

The transcript briefly mentions the hierarchical design of a network:

1. **Core Layer** — The innermost part of the network. It is very fast.
2. **Distribution Layer** — Responsible for policy (deciding what traffic goes where).
3. **Access Layer** — The "end mile" (or last mile) solution, where end users connect.

🎯 **Exam Important Points:**
- Core = fast backbone
- Distribution = policy enforcement
- Access = end-user connection (last mile)
- The transcript mentions this briefly — it may appear as a concept question

---

## Concept 16: Summary of Layer Functions (from Transcript's Closing)

📌 **Concept Name:** What Each Layer Does — Final Summary

🧠 **Simple Explanation (directly from transcript's summary):**

- **Physical Layer:** Physical transmission of data
- **Data Link Layer:** Filtering at the MAC layer level; divides collision domains
- **Network Layer (Router):** Layer 3 device that connects devices on different networks; helps in inter-networking
- **Transport Layer:** Connects two processes on machines in the internetwork — provides process-to-process communication
- **Application Layer:** Where the end user uses different applications for inter-networking

Every layer processes data and adds its own header (encapsulation — making the payload for the next layer).

The course will follow a **top-down approach** — starting from Application layer going down to Transport and beyond.

🎯 **Exam Important Points:**
- Physical → physical transmission
- Data Link → MAC filtering, collision domain division
- Network → inter-networking, routing
- Transport → process-to-process communication
- Application → end-user applications
- Course approach = top-down (application first)

---

---

# 10 MCQs from Lecture 2

---

**Q1.** ARPANET was commissioned in which year?

(A) 1962  
(B) 1969  
(C) 1971  
(D) 1982  

**Answer: (B) 1969**

*Explanation:* As per the transcript, ARPANET was commissioned by DoD for research in 1969 with four major US universities: UCLA, Stanford Research Institute, UCSB, and University of Utah.

---

**Q2.** The concept of WWW (World Wide Web) was coined by:

(A) Vint Cerf  
(B) Robert Kahn  
(C) Tim Berners-Lee  
(D) Dennis Ritchie  

**Answer: (C) Tim Berners-Lee**

*Explanation:* The transcript states that in 1989, the "WWW" concept was coined by Tim Berners-Lee.

---

**Q3.** Which of the following is a cross-layer protocol mentioned in the transcript?

(A) HTTP  
(B) TCP  
(C) DNS  
(D) Ethernet  

**Answer: (C) DNS**

*Explanation:* The transcript mentions DNS, SNMP, ARP, and DHCP as cross-layer protocols that exist between two layers and interface between them.

---

**Q4.** How many layers does the OSI model have?

(A) 4  
(B) 5  
(C) 6  
(D) 7  

**Answer: (D) 7**

*Explanation:* The OSI model has 7 layers: Physical, Data Link, Network, Transport, Session, Presentation, and Application.

---

**Q5.** Which layer in the OSI model is responsible for data formatting and encryption?

(A) Session  
(B) Presentation  
(C) Transport  
(D) Application  

**Answer: (B) Presentation**

*Explanation:* The transcript clearly states: "Presentation is data formatting and encryption."

---

**Q6.** A hub with 10 Mbps bandwidth and 8 ports will have an effective worst-case bandwidth per device of:

(A) 10 Mbps  
(B) 8 Mbps  
(C) 1.25 Mbps  
(D) 80 Mbps  

**Answer: (C) 1.25 Mbps**

*Explanation:* The transcript says hubs share bandwidth between all attached devices. With a 10 Mbps hub and 8 ports, worst case = 10/8 = 1.25 Mbps per device.

---

**Q7.** Which device breaks up broadcast domains by default?

(A) Hub  
(B) Switch  
(C) Bridge  
(D) Router  

**Answer: (D) Router**

*Explanation:* The transcript states: "Routers by default break up broadcast domain." When a router receives a broadcast, it discards it without forwarding. Switches and hubs do NOT break broadcast domains.

---

**Q8.** What type of cable is needed to connect two computers directly using their NICs?

(A) Straight-through cable  
(B) Crossover cable  
(C) Coaxial cable  
(D) Fiber optic cable  

**Answer: (B) Crossover cable**

*Explanation:* The transcript says to connect two computers with NIC cards, you need a crossover cable where the TX of one goes to the RX of the other and vice versa.

---

**Q9.** Which of the following is TRUE about the Network Layer as described in the transcript?

(A) It provides reliable delivery  
(B) It provides unreliable delivery — reliability is not guaranteed  
(C) It establishes sessions  
(D) It handles data formatting  

**Answer: (B) It provides unreliable delivery — reliability is not guaranteed**

*Explanation:* The transcript says: "Network layer... it is not a reliable layer. It delivers packets in an unreliable way, means reliability is not guaranteed."

---

**Q10.** In the hierarchical network design mentioned in the transcript, which layer is described as the "end mile" solution?

(A) Core  
(B) Distribution  
(C) Access  
(D) Transport  

**Answer: (C) Access**

*Explanation:* The transcript describes three layers: Core (very fast), Distribution (policy), and Access (end mile type of solution).

---

*End of Lecture 2 Complete Notes*
