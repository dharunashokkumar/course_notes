# Lecture 5: Application Layer – I
## Computer Networks and Internet Protocol (NPTEL)
### Prof. Soumya Kanti Ghosh, IIT Kharagpur

---

## Overview of This Lecture

This lecture introduces the **Application Layer** of the TCP/IP protocol stack. It is the first layer we study in detail. The lecture covers what the application layer does, how it connects to the transport layer, popular application layer protocols (DNS, FTP, TFTP, HTTP, SMTP, SNMP, Telnet), port numbers, and the concept of sockets.

---

## Concept 1: Application Layer in the TCP/IP Protocol Stack

### 📌 Concept Name
Position of Application Layer in the TCP/IP Stack

### 🧠 Simple Explanation
The TCP/IP protocol stack has these layers (from top to bottom):

1. **Application Layer** — (topmost) where user programs like web browsers, email clients, file transfer run
2. **Transport Layer** — provides either connection-oriented (TCP) or connectionless (UDP) service
3. **Internetworking Layer (Layer 3 / IP Layer)** — handles routing of packets
4. **Network Interface and Hardware** — includes Data Link Layer and Physical Layer at the bottom

The Application Layer is the **most important layer from the end user's perspective**. When you open a web page, send an email, or download a file — you are directly interacting with applications at this layer. You do not see or worry about the underlying layers.

The major application layer protocols mentioned are: **Telnet, FTP, SMTP, HTTP, and DNS**.

### 🛠 Real-world Example (from transcript)
When you type "www.iitkgp.ac.in" in your browser, you are using the HTTP application. You only care about seeing the web page. You do not care about the routers, IP addresses, or cables underneath — the application layer shields you from all that.

### 🎯 Exam Important Points
- Application layer is at the **top** of the TCP/IP stack.
- It directly interacts with the **end user/client**.
- Major protocols: Telnet, FTP, SMTP, HTTP, DNS.
- Application layer primarily talks to the **Transport Layer** below it.
- In some cases, an application can directly talk to the **IP layer** (skipping transport).

### ⚠️ Common Confusions
- The application layer is NOT the application itself (like Chrome or Outlook). It is the **protocol** used by the application (like HTTP, SMTP).
- Don't confuse "application layer" with "application software." The layer defines rules/protocols for communication.

---

## Concept 2: Application Layer — Basic Philosophy

### 📌 Concept Name
What the Application Layer Actually Does

### 🧠 Simple Explanation
The application layer is basically the **"programming interface"** to the entire networking system. It is a **user process** that cooperates with **another process** — usually on a **different host (computer)**. This is what we call the **Client-Server model**.

- When you use a browser, your browser is the **client**.
- The website you visit has a **server** running on the other end.
- So: HTTP Client (your browser) ↔ HTTP Server (website's server)
- Similarly: FTP Client ↔ FTP Server, Telnet Client ↔ Telnet Server

The application layer does NOT have to communicate with a process on a different network. It can also be on the **same network**, but typically it is across networks.

### 🎯 Exam Important Points
- Application layer = programming interface to networking.
- It is a **user process cooperating with another process** (usually on a different host).
- Follows the **Client-Server model**.
- Popular examples: Telnet, SMTP, SNMP, FTP, HTTP, DNS.

### ⚠️ Common Confusions
- Client and server do NOT have to be on different networks. They can be on the same machine or same network. But usually, they are on different hosts.

---

## Concept 3: Port Numbers and Process Identification

### 📌 Concept Name
How to Identify a Process — IP Address + Port Number

### 🧠 Simple Explanation
To communicate across a network, you need to identify TWO things:

1. **Which machine?** → Identified by the **IP address**
2. **Which process on that machine?** → Identified by the **Port number**

So, **IP address + Port number** together uniquely identify a running process on a specific machine. This is a **transport layer concept** but the application layer uses it.

**Well-known port numbers** (from the transcript):
- **Telnet** → Port **23**
- **FTP** → Port **21** (control) and Port **20** (data) — FTP uses two ports!
- **HTTP** → Port **80**

You can also define your **own custom port** where a server is running. The client can connect through any available port.

### 🛠 Real-world Example (from transcript)
When you visit "www.iitkgp.ac.in", your browser connects to the IP address of that server on **port 80** (the standard HTTP port). Your computer automatically assigns a port for the client side.

### 🎯 Exam Important Points
- **IP address** identifies the machine.
- **Port number** identifies the process within the machine.
- **IP + Port = Unique process identification** on the network.
- FTP uses **two ports**: 21 (control) and 20 (data).
- HTTP uses port **80**.
- Telnet uses port **23**.
- Client port is usually **automatically assigned** by the operating system.

### ⚠️ Common Confusions
- Don't say "IP address identifies the process." IP identifies the **machine**, port identifies the **process**.
- FTP is special — it uses TWO ports (21 and 22 are mentioned in transcript as control and data). Most other protocols use one port.

---

## Concept 4: Sockets

### 📌 Concept Name
Socket — The Communication Endpoint

### 🧠 Simple Explanation
A **socket** is the interface that is established between the source (client) and destination (server) for communication. Think of a socket as a **communication endpoint** — it is the point through which data goes in and out.

To establish a socket connection, you need **5 things** (as per the transcript):

1. **IP address of the server** (where to connect)
2. **Port number of the server** (which process on that server)
3. **IP address of the client** (where the request is coming from)
4. **Port number of the client** (automatically given by the OS if not specified)
5. **Protocol** (TCP or UDP — which rules to follow)

Once a socket is established, a **communication path** exists between client and server. You can then do file transfer (FTP), chat, web browsing (HTTP), or any application.

The term **socket programming** refers to writing programs that use sockets to communicate over the network.

### 🎯 Exam Important Points
- Socket = communication endpoint / interface between source and destination.
- **5 things needed** for a socket connection: Server IP, Server Port, Client IP, Client Port, Protocol.
- Client port is automatically assigned by the system if not manually set.
- Socket programming allows writing programs for network communication.

### ⚠️ Common Confusions
- A socket is NOT just an IP address or just a port. It is the **combination** — the endpoint of communication.
- Socket is an **interface**, not a physical thing.

---

## Concept 5: Data Encapsulation Across Layers

### 📌 Concept Name
How Data Moves Down the Protocol Stack (Encapsulation)

### 🧠 Simple Explanation
When you send data (say an HTTP request), here is what happens layer by layer:

1. **Application Layer**: Creates HTTP data + HTTP header → This becomes the "application data"
2. **Transport Layer**: Takes the application data as a **payload**, adds its own Transport header (TCP/UDP header)
3. **IP Layer (Network Layer)**: Takes the transport data as payload, adds **IP header**
4. **Data Link Layer**: Takes the IP data as payload, adds its own header
5. **Physical Layer**: Transmits the actual bits over the wire/wireless

At the **receiving end**, the process is reversed — each layer **removes (extracts) its header** and passes the data up.

**Key point from transcript**: If the data reaches a **router**, it only gets extracted up to the **Network Layer (Layer 3)**. If it reaches the **destination end system**, it gets extracted all the way up to the **Application Layer**.

### 🎯 Exam Important Points
- Data from each layer becomes a **payload** for the layer below.
- Each layer adds its own **header**.
- Router extracts data only up to **Layer 3 (IP/Network layer)**.
- End system (destination) extracts data up to the **Application Layer**.
- This supports **interoperability** — you do not care what routers are in between.

### ⚠️ Common Confusions
- A router does NOT look at application layer data. It only works up to Layer 3.
- Encapsulation = adding headers going down. De-encapsulation = removing headers going up.

---

## Concept 6: TCP vs UDP at the Application Layer

### 📌 Concept Name
Connection-Oriented (TCP) vs Connectionless (UDP) Service

### 🧠 Simple Explanation
Applications have different needs. Some need **reliable, ordered delivery**. Some just need **fast, simple delivery**.

**TCP (Transmission Control Protocol)** — Connection-Oriented:
- Establishes a logical connection between client and server.
- Provides: **Connection establishment, reliable data transfer, flow control, congestion control, ordered packet delivery**.
- Used for: HTTP, Email (SMTP), File Transfer (FTP).

**UDP (User Datagram Protocol)** — Connectionless:
- No connection setup. Just send the data.
- Used for: **DNS**, **SNMP** (network management).
- Simpler, faster, but no guarantee of delivery.

**Important from transcript**: TCP runs on top of IP. IP itself is a **connectionless, best-effort protocol** — it does NOT guarantee packet delivery. TCP adds reliability ON TOP of this unreliable IP layer. How TCP achieves reliability over unreliable IP is covered in later lectures.

### 🎯 Exam Important Points
- TCP = connection-oriented, reliable, ordered delivery.
- UDP = connectionless, no guarantee, simpler.
- **DNS** uses **UDP**.
- **HTTP, SMTP, FTP** use **TCP**.
- IP is connectionless and best-effort (no delivery guarantee).
- TCP provides reliability OVER the unreliable IP layer.
- Other protocols mentioned: ICMP, IGMP, OSPF, RSVP — these sit between Transport and IP.

### ⚠️ Common Confusions
- Don't assume UDP is "bad." For some use cases (like DNS lookups), UDP is the right choice — faster and sufficient.
- IP is NOT reliable. TCP adds the reliability on top of IP.

---

## Concept 7: Layers and Their Control (Software, Kernel, Hardware)

### 📌 Concept Name
What Controls Each Layer

### 🧠 Simple Explanation
Different layers are handled by different parts of the system:

- **Application Layer**: Controlled by **user software** (your programs/applications).
- **Transport and Network (IP) Layer**: Controlled by **software and kernel** of the operating system. When you configure TCP/IP properties in Windows or Linux, you are setting up these layers.
- **Data Link Layer (partially) and Physical Layer**: Controlled by **firmware, device drivers, and hardware**.
  - You need a **Network Interface Card (NIC)** for physical connectivity (e.g., when you plug in an RJ-45 cable).
  - For wireless, you need a **wireless interface card** with appropriate **device drivers** from the OS.

### 🛠 Real-world Example (from transcript)
When you plug an RJ-45 cable into your laptop, the NIC card handles the physical connection. The device driver (part of your OS) manages communication with the NIC. The OS kernel manages TCP/IP. Your browser (application) sits on top.

### 🎯 Exam Important Points
- Application layer → User software
- Transport + IP → OS software and kernel
- Data Link + Physical → Firmware, device drivers, hardware (NIC)
- NIC (Network Interface Card) is needed for physical connectivity.
- Device drivers are needed for the OS to communicate with network hardware.

---

## Concept 8: Responsibilities of the Application Layer

### 📌 Concept Name
Four Key Responsibilities of the Application Layer

### 🧠 Simple Explanation
The transcript lists these as the typical responsibilities:

1. **Identifying and establishing availability of intended communication partners** — When you type a URL, the application layer needs to find the server and check if it is available.

2. **Synchronizing cooperating applications** — If multiple applications are working together (like in a chat server with request-response pattern), the application layer manages their coordination and synchronization.

3. **Establishing agreement on procedures for error recovery** — If an error happens, there should be a defined procedure to recover from it and report it.

4. **Controlling data integrity** — Ensuring the data is correct and not corrupted during communication.

### 🎯 Exam Important Points
- Four responsibilities: (1) Identify communication partner, (2) Synchronize cooperating apps, (3) Error recovery procedures, (4) Data integrity control.
- These are the **basic** responsibilities; specific applications may have additional ones.

---

## Concept 9: DNS (Domain Name System)

### 📌 Concept Name
DNS — Translating Names to IP Addresses

### 🧠 Simple Explanation
**DNS = Domain Name System**. Its major job is to **translate a domain name (like www.iitkgp.ac.in) into an IP address**.

Why is this needed? Because routers and the IP layer work with **IP addresses**, not names. When you type a website name, something must convert that name into an IP address. That "something" is the **DNS server**.

**How it works (step by step)**:
1. You type "www.iitkgp.ac.in" in your browser.
2. Your system has a **DNS client** that sends a request to a **DNS server**.
3. The DNS server **resolves** the name and sends back the IP address.
4. Now your system uses that IP address for the rest of the communication.

**How does your system know the DNS server?**
- It is configured during **network setup** (manually by admin, or in TCP/IP settings).
- Or it is **automatically assigned via DHCP** (Dynamic Host Configuration Protocol) when you connect.

**Domain hierarchy** (from the transcript):
- There are more than **200 top-level domains (TLDs)** on the internet.
- Examples of TLDs: **.in** (India), **.us** (US), **.edu** (educational), **.com** (company), **.net** (network services).
- In "iitkgp.ac.in":
  - **.in** = Top Level Domain (TLD) — represents India
  - **.ac** = Sub-domain under .in — represents academics
  - **iitkgp** = Sub-domain under .ac — represents IIT Kharagpur

### 🎯 Exam Important Points
- DNS translates **domain names → IP addresses**.
- DNS uses **UDP** (mentioned earlier in the lecture).
- Your system has a DNS **client**; it contacts a DNS **server** to resolve names.
- DNS server address comes from: manual configuration, network admin, or **DHCP**.
- More than **200 TLDs** exist.
- TLD examples: .in, .us, .edu, .com, .net
- Domain is hierarchical: TLD → sub-domain → sub-domain (e.g., .in → .ac → iitkgp)

### ⚠️ Common Confusions
- DNS does NOT route packets. It only resolves names to IPs. After that, routing takes over.
- DNS uses **UDP**, not TCP (for standard queries).

---

## Concept 10: FTP and TFTP

### 📌 Concept Name
FTP (File Transfer Protocol) vs TFTP (Trivial File Transfer Protocol)

### 🧠 Simple Explanation

**FTP (File Transfer Protocol)**:
- It is a **reliable, connection-oriented** service.
- Uses **TCP** to transfer files.
- Used when you need guaranteed, reliable file delivery.
- Example use: Transferring router images (like iOS images for Cisco routers).
- Requires more resources because of connection setup and reliability mechanisms.

**TFTP (Trivial File Transfer Protocol)**:
- It is a **connectionless** service.
- Uses **UDP** to transfer files.
- Designed to be **small and easy to implement**.
- Less overhead/payload compared to FTP.
- If transfer fails, you simply **retransmit** — not a big deal because the files are typically small.

### 🎯 Exam Important Points
- FTP = reliable, uses **TCP**, connection-oriented.
- TFTP = simple, uses **UDP**, connectionless.
- FTP is for cases where reliability is critical.
- TFTP is for cases where simplicity matters and files are small — if failure occurs, just resend.
- TFTP = "small and easy to implement" (as per transcript).

### ⚠️ Common Confusions
- Don't mix up FTP and TFTP. FTP = TCP (reliable). TFTP = UDP (simple, unreliable).
- FTP uses TWO ports (21 for control, 20 for data). TFTP does not need this complexity.

---

## Concept 11: HTTP (Hypertext Transfer Protocol)

### 📌 Concept Name
HTTP — The Most Predominant Application Layer Protocol

### 🧠 Simple Explanation
HTTP is described in the transcript as the **predominant protocol** on the internet. It is the most widely used application layer protocol.

**How HTTP works** (basic):
- The HTTP **client** (your browser) sends an **HTTP request**.
- The HTTP **server** (the website server) responds with an **HTTP response message**.
- This request-response cycle is how web pages are loaded.

**Special importance of HTTP** (from transcript):
- HTTP is extensively used.
- Most **routers and firewalls** allow HTTP traffic to pass through. If a firewall allows anything at all, HTTP is usually the first thing allowed.
- Because of this, many other services (like web services) **piggyback on HTTP** — they use HTTP as a carrier because it is almost always allowed.

### 🎯 Exam Important Points
- HTTP = Hypertext Transfer Protocol.
- Works on **request-response** model.
- Uses **TCP** (connection-oriented).
- Standard port: **80**.
- Most widely used protocol.
- **Routers and firewalls** usually allow HTTP traffic first.
- Other services piggyback on HTTP because of its widespread acceptance.

---

## Concept 12: SMTP (Simple Mail Transfer Protocol)

### 📌 Concept Name
SMTP — Email Communication Protocol

### 🧠 Simple Explanation
**SMTP** stands for **Simple Mail Transfer Protocol**. It is used by email servers to **send and receive mails** between each other.

SMTP is the **mail transport protocol** — it handles the actual sending of email from one server to another.

The transcript also briefly mentions that there are other protocols for the front-end (client side) of email, like **POP3** (Post Office Protocol version 3), which are used to retrieve emails from the server.

### 🎯 Exam Important Points
- SMTP = protocol for sending/receiving emails between servers.
- It is a **mail transport protocol**.
- Uses **TCP** (connection-oriented).
- POP3 is a front-end protocol for email retrieval (briefly mentioned).

---

## Concept 13: SNMP (Simple Network Management Protocol)

### 📌 Concept Name
SNMP — Network Management Protocol

### 🧠 Simple Explanation
**SNMP** = Simple Network Management Protocol. It is an **application layer protocol** that helps exchange **management information** between network devices.

How it works:
- There are **SNMP agents** on network devices that report the status of the network.
- There is a **Network Management System (NMS)** that collects this SNMP data and uses it for network management.

SNMP is **not directly used by end users**. It works behind the scenes to manage and monitor the network.

### 🎯 Exam Important Points
- SNMP = application layer protocol for network management.
- Facilitates exchange of **management information** between devices.
- Uses **SNMP agents** to report status.
- Uses **UDP** (mentioned earlier when discussing UDP applications).
- Not directly used by users — used for network management.

---

## Concept 14: Telnet

### 📌 Concept Name
Telnet — Remote Login Protocol

### 🧠 Simple Explanation
**Telnet** allows you to **connect to a remote system** and execute commands from the command line, as if you were sitting right in front of that remote computer.

How it works:
- The **Telnet client** makes a **TCP connection** to the **Telnet server** on the remote machine.
- Once connected, you can execute commands on the remote machine.
- The server may run user applications, access control mechanisms, or server control functions.
- You need **credentials** (IP, login, password) to connect — there is an access control check.

Telnet uses **port 23**.

### 🎯 Exam Important Points
- Telnet = remote login to another system.
- Uses **TCP** (connection-oriented).
- Port: **23**.
- Client-server model: Telnet client → Telnet server.
- Requires **credentials** (login and password) for access.
- Can run user applications, server control, and access control on the remote end.

---

## Concept 15: Server Daemon and How Servers Listen

### 📌 Concept Name
Server Daemon — Always Listening for Requests

### 🧠 Simple Explanation
When a server is running (like an HTTP server or Telnet server), it runs a special process called a **daemon**. The daemon is **always active** and **always listening** on a specific port for incoming client requests.

For example:
- The HTTP server daemon (called **"httpd"**) listens on **port 80** all the time.
- It constantly checks: "Is there any request from a client?"
- When a client request arrives, the daemon takes it and — if it is a **concurrent server** — it **creates a child process (fork)** to handle that specific request, while the main daemon continues listening for new requests.

### 🛠 Real-world Example (from transcript)
The www.iitkgp.ac.in web server has httpd running on port 80. When your browser sends a request, httpd receives it, forks a child process to serve your request, and continues listening for other clients.

### 🎯 Exam Important Points
- A server runs a **daemon** process that is always listening on a specific port.
- HTTP daemon = **httpd**, listens on port 80.
- FTP daemon = **ftpd**.
- When a request comes, a **concurrent server forks a child process** to serve it.
- The main daemon keeps listening for more requests.

---

## Concept 16: Device Independence and Interoperability

### 📌 Concept Name
The Beauty of TCP/IP — Device Independence and Protocol-Based Communication

### 🧠 Simple Explanation
The transcript highlights a very important property of the TCP/IP networking model:

- **Device Independence**: You can have different types of devices — wired, wireless, fiber, any hardware — and they can all communicate as long as they follow the **same protocol**.
- **You are NOT bothered about the underlying network**: The communication path can be fiber, wireless, wired — anything. Your application does not care.
- **Interoperability**: Different systems, different operating systems, different hardware — all can communicate because everyone follows the same set of **protocol rules**.
- **No centralized control**: There is no single entity controlling everything. You develop your application, follow the protocols, and it works.
- **The underlying network is hidden**: When you type a URL, you don't see the routers, switches, or cables in between. The protocol stack handles everything.

This is what the transcript calls the **"ubiquitousness"** of network-level communication — it works everywhere, across all kinds of devices and networks.

### 🎯 Exam Important Points
- TCP/IP supports **device independence** and **interoperability**.
- Different device types can communicate if they follow the **protocol**.
- Communication path (fiber, wireless, wired) is hidden from the application.
- **No centralized control** — everyone follows protocol guidelines.
- This is why the Internet is so popular — it is **heterogeneous** and works everywhere.
- Authorities exist for protocol guidelines, but what you develop at your end is your own choice.

---

## Concept 17: Companion Protocols (ICMP, IGMP, OSPF, RSVP)

### 📌 Concept Name
Protocols Between Transport and IP Layer

### 🧠 Simple Explanation
The transcript briefly mentions some companion protocols that sit **between the Transport layer and the IP layer**:

- **ICMP** — Internet Control Message Protocol
- **IGMP** — Internet Group Management Protocol
- **OSPF** — Open Shortest Path First (a routing protocol)
- **RSVP** — Resource Reservation Protocol

These are not application layer protocols but are part of the overall protocol stack. The transcript mentions them as a "bunch of protocols" in the stack but does not explain them in detail in this lecture.

### 🎯 Exam Important Points
- ICMP, IGMP, OSPF, RSVP sit between Transport and IP.
- They are part of the protocol stack but NOT application layer protocols.
- The transcript does not provide further details about these in Lecture 5.

---

## Summary Table: Application Layer Protocols from Lecture 5

| Protocol | Full Form | Uses TCP/UDP | Port | Purpose |
|----------|-----------|-------------|------|---------|
| HTTP | Hypertext Transfer Protocol | TCP | 80 | Web browsing |
| FTP | File Transfer Protocol | TCP | 21 (control), 20 (data) | File transfer (reliable) |
| TFTP | Trivial File Transfer Protocol | UDP | — | File transfer (simple, small files) |
| SMTP | Simple Mail Transfer Protocol | TCP | — | Sending/receiving email between servers |
| DNS | Domain Name System | UDP | — | Name to IP resolution |
| SNMP | Simple Network Management Protocol | UDP | — | Network management |
| Telnet | Telnet | TCP | 23 | Remote login |

---

## 5 Key Things a Socket Connection Needs

1. Server IP address
2. Server Port number
3. Client IP address
4. Client Port number (auto-assigned by OS)
5. Protocol (TCP or UDP)

---

# 📝 10 MCQs from Lecture 5

---

### Q1. Which layer of the TCP/IP protocol stack is most directly visible to the end user?

A) Transport Layer
B) Network Layer
C) Application Layer
D) Data Link Layer

**Answer: C) Application Layer**
**Explanation:** The transcript says the application layer has a "direct connection or manifestation to the end user perspective." When you open a web page or send email, you interact with the application layer.

---

### Q2. How is a specific process on a machine identified in a network?

A) By IP address alone
B) By MAC address
C) By IP address + Port number
D) By hostname alone

**Answer: C) IP address + Port number**
**Explanation:** The transcript says "How do I identify a system? By an IP address. How do I identify a process in the system? By IP plus a port number."

---

### Q3. FTP uses which type of transport service?

A) Connectionless (UDP)
B) Connection-oriented (TCP)
C) Both TCP and UDP
D) Neither TCP nor UDP

**Answer: B) Connection-oriented (TCP)**
**Explanation:** The transcript says "FTP is a reliable connection-oriented service that uses TCP to transfer files."

---

### Q4. What is the primary function of DNS?

A) Transfer files between hosts
B) Manage network devices
C) Translate domain names to IP addresses
D) Provide remote login access

**Answer: C) Translate domain names to IP addresses**
**Explanation:** The transcript says DNS's "major job is to translate name to IP." It resolves domain names so that routing can happen using IP addresses.

---

### Q5. Which protocol uses UDP and is designed to be "small and easy to implement"?

A) FTP
B) HTTP
C) TFTP
D) SMTP

**Answer: C) TFTP**
**Explanation:** The transcript says "TFTP is designated for small and easy to implement" and uses UDP (connectionless service).

---

### Q6. What is the standard port number for HTTP?

A) 21
B) 23
C) 25
D) 80

**Answer: D) 80**
**Explanation:** The transcript mentions multiple times that the HTTP server listens on "port 80" as the standard port.

---

### Q7. In the domain name "iitkgp.ac.in", what is the Top Level Domain (TLD)?

A) iitkgp
B) ac
C) in
D) www

**Answer: C) in**
**Explanation:** The transcript says ".in" is the top level domain representing India. Below that, ".ac" is a sub-domain for academics, and "iitkgp" is a further sub-domain.

---

### Q8. What does the server daemon (like httpd) do?

A) It shuts down the server after each request
B) It listens continuously on a specific port for client requests
C) It translates domain names to IP
D) It assigns IP addresses to clients

**Answer: B) It listens continuously on a specific port for client requests**
**Explanation:** The transcript says the daemon is "always alive" and "always listening" on a port. When a client request comes, it forks a child process to serve it while continuing to listen.

---

### Q9. Which of the following is NOT a responsibility of the application layer as described in the transcript?

A) Identifying intended communication partners
B) Synchronizing cooperating applications
C) Routing packets through intermediate routers
D) Controlling data integrity

**Answer: C) Routing packets through intermediate routers**
**Explanation:** Routing is a function of the Network/IP layer, not the application layer. The transcript lists the application layer's responsibilities as: identifying partners, synchronizing apps, error recovery procedures, and data integrity control.

---

### Q10. How many items are needed to establish a socket connection according to the transcript?

A) 3 (IP, port, protocol)
B) 4 (server IP, server port, client IP, client port)
C) 5 (server IP, server port, client IP, client port, protocol)
D) 2 (IP and port)

**Answer: C) 5 (server IP, server port, client IP, client port, protocol)**
**Explanation:** The transcript says you need: IP of the server, port of the server, IP of the client, port of the client, and the protocol. "If we know this 5 step allow me to connect to the things."

---

# What Else Is Coming in Next Lectures

The transcript mentions that in subsequent lectures:
- More application layer protocols will be covered in greater detail.
- The course will then move to the **Transport Layer**, then **Network Layer**, then **Data Link Layer**, and so on.
- Socket programming will be discussed in more detail later.
- How TCP achieves reliability over unreliable IP will be covered later.
- Web services that piggyback on HTTP may be covered if time permits.

---

*End of Lecture 5 Notes — Application Layer I*
