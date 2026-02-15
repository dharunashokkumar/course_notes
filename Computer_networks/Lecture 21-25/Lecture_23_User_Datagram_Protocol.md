# Lecture 23 — User Datagram Protocol (UDP)

**Course:** Computer Networks and Internet Protocol  
**Instructor:** Prof. Sandip Chakraborty, IIT Kharagpur  
**Topic:** User Datagram Protocol (UDP) and QUIC Protocol

---

## Topics Covered in This Lecture

1. Why UDP is needed — Overhead problems of TCP
2. What is UDP — Definition and basic idea
3. Features and Uses of UDP
4. UDP is Connectionless and Unreliable
5. UDP Header Structure (4 fields)
6. Checksum — What it is and how it works
7. Pseudo Header in Checksum Computation
8. Why Pseudo Header is used — Double Validation
9. IP Header Checksum vs TCP/UDP Checksum
10. Applications that use UDP (DNS, BOOTP/DHCP, TFTP, SNMP, QUIC)
11. QUIC Protocol — Full form and basic idea
12. QUIC vs TCP Protocol Stack Comparison
13. QUIC: 0-RTT Connection Establishment
14. QUIC Initial Handshake (1-RTT)
15. QUIC Final/Successful Handshake (0-RTT)
16. QUIC Repeat/Rejected Handshake (1-RTT again)
17. QUIC Multi-Stream and Head-of-Line Blocking Free
18. Head-of-Line Blocking Problem in HTTP/2 (SPDY)
19. How QUIC solves Head-of-Line Blocking
20. QUIC Packet Sequence Number (No Duplicate ACK problem)
21. QUIC Deployment — Google, YouTube, Chromium

---

## Concept 1: Why UDP is Needed — The Overhead Problem of TCP

📌 **Concept Name:** TCP Signaling Overhead

🧠 **Simple Explanation:**

In the previous lectures, we learned that TCP provides many great features like connection establishment, reliable data delivery, flow control, congestion control, and ordered packet delivery. But all these features come at a cost — they add a **significant amount of overhead** called **signaling overhead**.

What does "signaling overhead" mean? It means you are sending extra data/messages on the internet just to make the protocol work correctly, not to send actual user data.

Here are the main overheads of TCP:

**Overhead 1 — Connection Establishment and Closure:** For every TCP connection, you need a **three-way handshake** to establish the connection and **three messages** to close the connection. Now imagine you are browsing the web. Every time you send an HTTP request and get a response, if each one needs its own TCP connection, you are wasting 3 messages to open and 3 messages to close the connection. That is a lot of extra work for small data transfers.

**Overhead 2 — Flow Control and Congestion Control:** If TCP detects a packet loss, it needs to retransmit. This retransmission blocks the flow of existing and new data packets. Also, the congestion control algorithm forces TCP to start from a very low value of the congestion window (slow start phase) and gradually increase it. On a high-speed link, by the time the TCP flow reaches its operating point, a short flow (like an HTTP request-response) may already be finished! Then a new TCP connection starts again with slow start from scratch.

So, for many applications, we do not need or cannot tolerate this kind of overhead. What we really need is just to send data to the other end and let it be parsed. That is where UDP comes in.

🎯 **Exam Important Points:**
- TCP has signaling overhead due to connection establishment, connection release, flow control, and congestion control
- Three-way handshake for connection setup and three messages for closure is wasteful for short flows
- Slow start in TCP means starting from a low congestion window value every time a new connection is created
- For short flows like HTTP request-response, TCP connection may close before reaching optimal speed

⚠️ **Common Confusions:**
- Overhead does NOT mean TCP is bad — it just means TCP is not ideal for every application
- Signaling overhead = extra messages for protocol operation, NOT user data

---

## Concept 2: What is UDP — Definition and Basic Idea

📌 **Concept Name:** User Datagram Protocol (UDP)

🧠 **Simple Explanation:**

UDP stands for **User Datagram Protocol**. It is another transport layer protocol, like TCP, but it is much simpler. While TCP is widely used, many applications use UDP because they prefer speed over reliability.

The basic difference between TCP and UDP is: **UDP is a very simple protocol and it does NOT support the functionalities that TCP provides.** TCP supports connection establishment, reliable data delivery, flow control, congestion control, and ordered packet delivery. UDP provides **none** of these.

What does UDP actually do? It just provides **end-to-end packet delivery** — or in UDP terminology, **end-to-end datagram delivery**. That is all. It takes the data from the application and delivers it to the destination application. If packets are lost, UDP does not care.

A good example from the transcript: **DNS (Domain Name System)**. When you send a DNS request and expect a DNS response, you do not need a TCP connection for this. Using TCP for DNS would be a huge overhead. Instead, you simply send a DNS message using UDP. If the DNS server does not respond, the sender will have a timeout and send the request again. Simple and fast.

🎯 **Exam Important Points:**
- UDP = User Datagram Protocol
- UDP is a transport layer protocol
- UDP provides ONLY end-to-end datagram delivery
- UDP does NOT provide: connection establishment, reliable data delivery, flow control, congestion control, ordered packet delivery
- DNS uses UDP because it is a simple request-response — no need for TCP overhead

⚠️ **Common Confusions:**
- UDP is not "useless" — it is intentionally designed to be simple for applications that need speed
- The term "datagram" is used in UDP, whereas TCP uses "segment"

---

## Concept 3: Features and Uses of UDP

📌 **Concept Name:** UDP Features and Use Cases

🧠 **Simple Explanation:**

The transcript presents a clear table of UDP features and uses:

**Features of UDP:**

**Feature 1 — Simple Protocol:** UDP is sometimes said to be "not at all a transport layer protocol." It is just like a **wrapper on top of the IP layer**. Whatever services the IP layer provides, UDP simply forwards them to the application by bypassing typical transport layer functionality.

**Feature 2 — Fast:** Because you do not need to wait for the slow start phase, connection establishment, connection closure, flow control, or congestion control, the protocol is **very fast**. It works nicely when the network has a **low loss probability**.

**Uses of UDP:**

**Use 1 — Provide Performance:** You do not have any buffer like TCP. So you can deliver data faster when your link is good, or when you do not bother about packet loss.

**Use 2 — Short and Sweet:** UDP has **no overhead**. It is suitable for **short message transfers** like DNS (Domain Name System).

🎯 **Exam Important Points:**
- UDP is a wrapper on top of IP layer
- UDP is fast because there is no slow start, no connection setup, no flow control, no congestion control
- UDP provides better performance — no data holding in buffer like TCP
- UDP is suitable for short messages with no overhead

⚠️ **Common Confusions:**
- "Wrapper on top of IP" means UDP adds very little beyond what IP already provides
- "No buffer like TCP" means UDP does not hold data waiting for acknowledgments

---

## Concept 4: UDP is Connectionless and Unreliable

📌 **Concept Name:** Connectionless and Unreliable Nature of UDP

🧠 **Simple Explanation:**

The transcript clearly states: **UDP is connectionless and unreliable.**

**Connectionless** means: You just send data packets (datagrams) one after another. You do NOT first establish a connection. You do not even check whether the server is running or not. You simply send a packet.

**Unreliable** means: If a datagram gets lost in the network, the server does not take care of it, and UDP does not care about it either. There is **no acknowledgment mechanism** in UDP.

**No connection establishment** — You do not bother about whether the server is running or not.

**No reliability** — If a packet is lost, it is lost. UDP does not retransmit.

**No acknowledgment** — The sender gets no confirmation that the data was received.

Now, the important point from the transcript: **If the application cares about packet loss, the application itself will apply its own acknowledgment mechanism or its own procedure for handling or recovering from the loss.** So it is the application's responsibility, not UDP's responsibility.

🎯 **Exam Important Points:**
- UDP is connectionless — no connection establishment before data transfer
- UDP is unreliable — no guarantee of delivery, no retransmission
- UDP has no acknowledgment mechanism
- If reliability is needed, the application layer must handle it on its own

⚠️ **Common Confusions:**
- "Unreliable" does not mean UDP always loses packets — it means UDP does not have mechanisms to recover from loss
- Applications like DNS handle loss themselves (using timeouts and retransmission at the application level)

---

## Concept 5: UDP Header Structure

📌 **Concept Name:** UDP Header — 4 Fields

🧠 **Simple Explanation:**

Compared to the TCP header (which has many fields), the UDP header is **very simple**. It has only **four fields**:

**Field 1 — Source Port (16 bits):** The port number of the sender (client port).

**Field 2 — Destination Port (16 bits):** The port number of the receiver (server port).

**Field 3 — Length (16 bits):** Tells how much data is there in your UDP datagram. This is required to find out the size of the packet.

**Field 4 — Checksum (16 bits):** Used to check the correctness of the packet. Even though UDP does not provide reliability, the receiver still wants to know whether the datagram it received is correct or if something got corrupted (bits flipped) during transmission.

After the header comes the **Data** (the actual payload).

So the total UDP header is very small — just 4 fields, each of 16 bits, making the header **8 bytes** total.

🎯 **Exam Important Points:**
- UDP header has exactly 4 fields: Source Port, Destination Port, Length, Checksum
- Each field is 16 bits
- UDP header size = 8 bytes (compared to TCP's minimum 20 bytes)
- Checksum is for correctness checking, not for reliability
- The "Length" field indicates the total size of the UDP datagram (header + data)

⚠️ **Common Confusions:**
- Do not confuse UDP header (4 fields, 8 bytes) with TCP header (many fields, minimum 20 bytes)
- Checksum in UDP is for detecting corruption, NOT for guaranteeing delivery

📝 **Possible NPTEL-style Question:**
"How many fields are in the UDP header?" → Answer: 4 (Source Port, Destination Port, Length, Checksum)

---

## Concept 6: Checksum — What It Is and How It Works

📌 **Concept Name:** Internet Checksum in TCP/UDP

🧠 **Simple Explanation:**

The transcript explains checksum as a **function**. You provide a message as input to this function, and you get a fixed-size output value. This output value is called the **checksum** (denoted as C).

Mathematically: **f(M) = C**, where M is the message and C is the checksum.

Now, because the checksum is of **fixed length**, you can think of it as a **hash function**. But the internet checksum is **NOT a complicated cryptographic hash function**. Why? Because we do not need the "one-way property" that cryptographic hash functions have. We are just concerned about getting a **fixed-size message digest** out of the message. So the internet checksum computation is **fairly simple**.

**How internet checksum works (basic idea):** You divide the entire message into **fixed-size blocks**, and then make **one's complement addition** to compute the checksum.

**At the sender side:** The sender computes the checksum C from the message and puts it in the checksum field of the header. Then it sends the packet.

**At the receiver side:** The receiver takes the received data, applies the same function to compute the checksum. Then it compares: Does the checksum that came with the message match the checksum computed at the receiver side? If they match → the packet is correct. If they don't match → something got corrupted during transmission.

**Very important point from transcript:** Checksum is **NOT** for security or preventing attacks. It is only to ensure **packet integrity from network faults or system faults**. During data transmission, digital-to-analog and analog-to-digital conversion, modulation, encoding/decoding at the physical layer — due to sampling errors or quantization errors, some bits might get flipped (a 1 becomes 0, or a 0 becomes 1). The checksum helps detect these kinds of errors.

🎯 **Exam Important Points:**
- Checksum is a function: f(M) = C (message → fixed-size checksum value)
- Internet checksum uses one's complement addition on fixed-size blocks
- NOT a cryptographic hash — simpler and faster
- Purpose: detect errors from network faults (bit flips during transmission), NOT security attacks
- Sender computes checksum and puts it in the header; receiver recomputes and compares

⚠️ **Common Confusions:**
- Checksum ≠ Security mechanism. It only protects against accidental bit errors, not intentional attacks
- Internet checksum is simple (one's complement addition), not complex like SHA or MD5

---

## Concept 7: Pseudo Header in Checksum Computation

📌 **Concept Name:** Pseudo Header for TCP/UDP Checksum

🧠 **Simple Explanation:**

This is a very important concept. When TCP or UDP computes the checksum, the input to the checksum function is NOT just the TCP/UDP header + data. It also includes something called a **pseudo header**.

**What is the pseudo header?** The pseudo header contains certain fields taken from the **IP header**. The contents of the pseudo header are:

1. **Source IP address** (from IP header)
2. **Destination IP address** (from IP header)
3. **Protocol field** (from IP header)
4. **Reserved bits** — 8 reserved bits from the IP header

So the checksum is computed over: **Pseudo Header + TCP/UDP Header + Data**

**Critical point from transcript:** The pseudo header is **NOT transmitted** with the packet. It is used **only** for the computation of the checksum. Once the checksum is computed, the pseudo header is dropped. At the receiver end, the receiver will again **construct** the pseudo header (from the IP header of the received packet), compute the checksum, and compare it with the received checksum. Then the pseudo header is dropped again.

🎯 **Exam Important Points:**
- Pseudo header = Source IP + Destination IP + Protocol field + Reserved bits (all from IP header)
- Pseudo header is used ONLY for checksum computation, never transmitted
- Checksum input = Pseudo Header + TCP/UDP Header + Data
- Sender creates pseudo header → computes checksum → drops pseudo header → sends packet
- Receiver reconstructs pseudo header → computes checksum → compares → drops pseudo header

⚠️ **Common Confusions:**
- The pseudo header is NOT part of the UDP packet. It is NEVER sent over the network
- The pseudo header fields come from the IP header, not from the transport layer

---

## Concept 8: Why Pseudo Header is Used — Double Validation

📌 **Concept Name:** Purpose of Pseudo Header — End-to-End Integrity

🧠 **Simple Explanation:**

Why do we include a pseudo header in the checksum computation? The transcript explains this beautifully.

**Reason: Double Validation of Source IP, Destination IP, and Protocol.**

The IP header already has its own checksum field. But here is the problem: **the IP header checksum changes at every router.** Why? Because every router looks at the IP header, applies the routing mechanism, may change the IP header (like changing the TTL field), and then recomputes the IP header checksum. So the IP header checksum keeps changing hop by hop.

Now, there could be faults inside a router. If a router has some inconsistency or error, it might introduce an error in the source IP or destination IP fields. The IP header checksum would be recomputed at that router and would look fine — but the source/destination IP might be wrong.

**However**, the transport layer protocols (TCP/UDP) are **end-to-end protocols**. The TCP/UDP header **never gets changed at intermediate routers** — routers only look at the IP header. So by including the source IP, destination IP, and protocol field in the checksum computation of the transport layer, we are doing a **double check** — making sure these critical fields have not been corrupted during the entire journey from source to destination.

So the pseudo header provides **end-to-end integrity checking** for the source IP, destination IP, and protocol field — something the IP layer checksum alone cannot guarantee because it changes at every hop.

🎯 **Exam Important Points:**
- IP header checksum changes at every router (because routers modify IP header fields)
- TCP/UDP headers are NEVER changed at intermediate routers — they are end-to-end
- Pseudo header provides a double check that source IP, destination IP, and protocol have not been corrupted end-to-end
- This is the key reason pseudo header is included in TCP/UDP checksum computation

⚠️ **Common Confusions:**
- Even though IP already has a checksum, it is not reliable end-to-end because it is recomputed at each hop
- The pseudo header checksum does NOT replace the IP checksum — it provides an additional end-to-end check

---

## Concept 9: Applications That Use UDP

📌 **Concept Name:** UDP-Based Applications

🧠 **Simple Explanation:**

The transcript lists several applications that use UDP instead of TCP:

**1. DNS (Domain Name System):**
- Keyword: Domain
- It is a simple request-response messaging system. DNS is faster over UDP than TCP. You send a DNS query, you get a DNS response. No need for connection establishment.

**2. BOOTP/DHCP (Network Configuration Protocols):**
- Keyword: Network configuration
- These are short messaging protocols that help with faster configuration of network devices. Using UDP makes this faster.

**3. TFTP (Trivial File Transfer Protocol):**
- Keyword: File transfer
- A lightweight file transfer protocol to transfer small files. It is simpler and lighter than FTP (which uses TCP).

**4. SNMP (Simple Network Management Protocol):**
- Keyword: Network management
- A simple UDP protocol that easily cuts through congestion. In TCP, if there is congestion, TCP reduces the rate. But in UDP, if the packet comes to the buffer and is not dropped from that intermediate buffer, eventually it will get transmitted. That is why SNMP uses UDP — network management packets need to get through even during congestion.

**5. QUIC (Quick UDP Internet Connection):**
- Keyword: Advanced transport protocol
- Developed by Google. UDP provides direct access to IP, while TCP cannot do that as easily. QUIC uses UDP as its underlying transport and implements its own reliability, flow control, and congestion control at the application level.

🎯 **Exam Important Points:**
- DNS, BOOTP/DHCP, TFTP, SNMP, and QUIC all use UDP
- Each has a specific reason for choosing UDP over TCP
- SNMP uses UDP because it can cut through congestion (TCP would reduce rate during congestion)
- QUIC uses UDP to get direct access to IP and implements its own features on top

⚠️ **Common Confusions:**
- TFTP ≠ FTP. TFTP is lightweight and uses UDP; FTP is full-featured and uses TCP
- SNMP chooses UDP specifically because network management messages need to get through during congestion

---

## Concept 10: QUIC Protocol — Full Form and Basic Idea

📌 **Concept Name:** QUIC — Quick UDP Internet Connection

🧠 **Simple Explanation:**

QUIC stands for **Quick UDP Internet Connection**. It was developed by **Google** and the first detailed research paper of QUIC came from **Google in 2017 SIGCOMM** (a top networking conference).

**Why was QUIC created?** The idea behind QUIC is to overcome the shortcomings of TCP:
- The slow start phase problem
- The connection establishment overhead for every individual flow
- The overhead of separate security layers

**How does QUIC work?** QUIC uses UDP as the underlying transport layer protocol. With the help of UDP, QUIC directly sends packets via IP. But whatever additional facilities like flow control, congestion control, and reliability are needed — they are **implemented as part of the application** with secure binding.

So QUIC is basically an **advanced transport protocol** that sits between HTTP and UDP, providing features that TCP would provide, but doing it more efficiently.

🎯 **Exam Important Points:**
- QUIC = Quick UDP Internet Connection
- Developed by Google, published at SIGCOMM 2017
- Uses UDP as the underlying transport protocol
- Overcomes TCP shortcomings: slow start, connection establishment overhead
- Flow control, congestion control, reliability are implemented at the application level in QUIC

⚠️ **Common Confusions:**
- QUIC is NOT a replacement for UDP — it USES UDP
- QUIC provides reliability, but this reliability is implemented at the application level, not at the transport level

---

## Concept 11: QUIC vs TCP — Protocol Stack Comparison

📌 **Concept Name:** Application over TCP vs Application over QUIC

🧠 **Simple Explanation:**

The transcript shows a clear comparison of the protocol stacks:

**Traditional TCP Stack (for HTTPS):**
- IP layer (bottom)
- TCP (transport layer)
- Encryption layer (SSL/TLS — Secure Socket Layer / Transport Layer Security)
- HTTP (application layer on top)

So when you use HTTPS, you need: IP → TCP → SSL/TLS → HTTP. That is 4 layers, and TCP + SSL/TLS add significant overhead.

**QUIC Stack:**
- IP layer (bottom)
- UDP (transport layer)
- QUIC (sits between UDP and HTTP — handles encryption, reliability, flow control, congestion control)
- HTTP (application layer on top)

The key difference: In QUIC, the **encryption is built into the QUIC protocol itself**. Every QUIC packet is **end-to-end encrypted**. So you do **NOT** need a separate SSL/TLS layer. The security feature is embedded inside QUIC.

HTTP runs directly on top of QUIC, and QUIC runs on top of UDP, which accesses IP.

🎯 **Exam Important Points:**
- TCP stack: IP → TCP → SSL/TLS → HTTP
- QUIC stack: IP → UDP → QUIC → HTTP
- QUIC has built-in encryption — no need for separate SSL/TLS layer
- Every QUIC packet is end-to-end encrypted
- QUIC provides end-to-end security inherently

⚠️ **Common Confusions:**
- SSL/TLS is needed with TCP, but NOT with QUIC
- QUIC does not just replace TCP — it also replaces the encryption layer (SSL/TLS)

---

## Concept 12: QUIC 0-RTT Connection — Initial Handshake (1-RTT)

📌 **Concept Name:** QUIC Initial Handshake

🧠 **Simple Explanation:**

One of the most important features of QUIC is its ability to combine multiple short flows together and reduce connection establishment overhead. Let me explain the QUIC handshake process as described in the transcript.

**Initial Handshake (First time connecting to a server) — Requires 1-RTT:**

Since QUIC is an end-to-end encryption protocol, the client needs security credentials from the server. Here is what happens the very first time:

**Step 1:** The client sends an **Inchoate CHLO** (Client Hello). "Inchoate" means incomplete — the client does not yet have the server's security certificate.

**Step 2:** The server receives the Inchoate CHLO and finds out that the client does not have the required security certificate. So the server sends a **REJ** (Reject) message. Along with this reject message, the server sends the **security credential** to the client.

**Step 3:** Now the client has the security credential. It sends a **Complete CHLO** (Complete Client Hello). Because the client already received the REJ message (meaning the server is running and ready), the client can **immediately start sending encrypted requests** along with the Complete CHLO.

**Step 4:** The server sends a **SHLO** (Server Hello) and starts sending **encrypted responses**.

So the initial handshake requires **1-RTT** (1 Round Trip Time) before data can flow.

🎯 **Exam Important Points:**
- First connection to a server requires 1-RTT handshake
- Client sends Inchoate CHLO → Server sends REJ with security credentials → Client sends Complete CHLO + encrypted data → Server sends SHLO + encrypted responses
- CHLO = Client Hello, SHLO = Server Hello, REJ = Reject
- Inchoate CHLO = incomplete client hello (no credentials yet)
- After receiving REJ, client knows server is running and can start sending data

⚠️ **Common Confusions:**
- The initial handshake is 1-RTT, NOT 0-RTT
- 0-RTT is only possible for subsequent connections (not the first one)

---

## Concept 13: QUIC 0-RTT Connection — Successful Handshake (0-RTT)

📌 **Concept Name:** QUIC Successful 0-RTT Handshake

🧠 **Simple Explanation:**

This is the really powerful part of QUIC. Once the initial 1-RTT handshake has been completed and the connection has been established, **for subsequent connections between the same client and server, you do NOT need the 1-RTT handshake.** Instead, you can use a **0-RTT handshake**.

Why? Because:
- You already have the server's security credentials (received during the initial handshake)
- You already know the server is running (because it responded before)
- You already have the Complete Client CHLO ready

So what happens in 0-RTT:

**Step 1:** The client directly sends a **Complete CHLO + Encrypted Request** — no need to wait for any response first.

**Step 2:** The server sends a **SHLO** (Server Hello) and starts sending **encrypted responses**. It can even send **multiple responses or handle multiple requests simultaneously**.

This is called a **Successful 0-RTT Handshake** — the client starts sending encrypted data immediately without waiting for any round trip. This dramatically reduces latency.

🎯 **Exam Important Points:**
- 0-RTT handshake is possible for subsequent connections (after the first 1-RTT handshake is done)
- Client already has server credentials, so it can directly send Complete CHLO + encrypted request
- No waiting for server response before sending data
- Server responds with SHLO + encrypted responses
- 0-RTT means data starts flowing immediately — zero round trip delay for connection setup

⚠️ **Common Confusions:**
- 0-RTT is NOT available for the very first connection — it only works when the client already has the server's credentials
- Compare with TCP: TCP always needs at least 1-RTT for connection establishment (SYN → SYN-ACK → ACK)

---

## Concept 14: QUIC Repeat/Rejected Handshake (When Credentials Change)

📌 **Concept Name:** QUIC Rejected 1-RTT Handshake

🧠 **Simple Explanation:**

Sometimes, the server's security credential may have **changed** between connections. What happens in that case?

When the client sends its Complete CHLO (expecting a 0-RTT connection), the server finds that the client's credential is **outdated**. So the server sends a **REJ (Reject) message** with the **updated server credential**.

Now, with this updated credential, the client can reinitiate the connection and start sending the request again.

So, whenever the server credential gets changed, you go back to requiring a **1-RTT handshake**. But between credential changes, you can always use the **0-RTT handshake**.

**Summary of all three cases:**
- **Initial Handshake:** 1-RTT (first time ever connecting)
- **Successful Handshake:** 0-RTT (credentials still valid from previous connection)
- **Rejected Handshake:** 1-RTT (server credentials have changed — need to get updated credentials)

🎯 **Exam Important Points:**
- If server credentials change, 0-RTT fails and falls back to 1-RTT
- Server sends REJ with updated credentials when client's credentials are outdated
- Three scenarios: Initial (1-RTT), Successful (0-RTT), Rejected (1-RTT)
- Between credential changes, 0-RTT works perfectly

⚠️ **Common Confusions:**
- Rejected handshake does NOT mean the connection fails — it just requires 1-RTT instead of 0-RTT
- The client gets updated credentials and can then proceed normally

---

## Concept 15: QUIC Multi-Stream and Head-of-Line Blocking Problem

📌 **Concept Name:** Head-of-Line (HoL) Blocking and How QUIC Solves It

🧠 **Simple Explanation:**

This is another major feature of QUIC. Let me explain the problem first, then the solution.

**The Problem — How HTTP/1.1 works:**
In HTTP/1.1, you can have **multiple TCP streams** (TCP 1, TCP 2, TCP 3) in parallel. Each TCP stream can send multiple request-response messages between the client and server. But the problem is: every individual TCP stream needs its own **connection establishment** and goes through the **slow start phase**. This creates overhead.

**HTTP/2 (or SPDY) Solution:**
To solve this, HTTP/2 (earlier called SPDY by Google) **multiplexes multiple streams into a single TCP connection**. So instead of multiple separate TCP connections, all streams are combined into one TCP connection.

**The Head-of-Line (HoL) Blocking Problem in HTTP/2:**
But this creates a new problem! Since TCP guarantees in-order delivery, if TCP receives a single out-of-order packet, it puts that packet in the buffer and starts sending **duplicate acknowledgments**. It will NOT send any received packets to the application until the missing packet arrives. 

Now imagine multiple streams (Stream 1, Stream 2, Stream 3) are multiplexed over a single TCP connection. Say a packet from Stream 1 (red packet) gets lost. Even if packets from Stream 2 (blue packets) and Stream 3 (black packets) are arriving perfectly, **TCP will NOT deliver them to their respective streams** until the missing red packet from Stream 1 is received. So all streams get blocked because of one lost packet in one stream. This is called **Head-of-Line (HoL) Blocking**.

**QUIC's Solution:**
QUIC solves this by using a **UDP connection** instead of TCP. UDP does not have the problem of blocking due to reordering. UDP simply passes the packet to the streams. Then the streams themselves (as part of the QUIC protocol) take care of sending packets to the individual streams. QUIC maintains **stream-wise flow control** and a **stream-wise congestion control** algorithm.

So in QUIC, if a packet from Stream 1 is lost, Stream 2 and Stream 3 continue to receive their packets without being blocked. Only Stream 1 is affected.

🎯 **Exam Important Points:**
- HTTP/1.1: multiple separate TCP connections → connection setup overhead for each
- HTTP/2 (SPDY): multiplexes streams into single TCP connection → reduces connection overhead
- But HTTP/2 has Head-of-Line (HoL) Blocking — one lost packet blocks ALL streams
- HoL Blocking happens because TCP guarantees in-order delivery across the entire connection
- QUIC uses UDP → no HoL blocking because UDP does not enforce ordering
- In QUIC, each stream is independent — loss in one stream does not block other streams
- QUIC maintains stream-wise flow control and congestion control

⚠️ **Common Confusions:**
- HoL blocking is NOT a problem in HTTP/1.1 (because each stream has its own TCP connection) — it is a problem in HTTP/2 where streams share ONE TCP connection
- QUIC does not remove flow control/congestion control — it just implements them per-stream instead of per-connection

---

## Concept 16: QUIC Packet Sequence Numbers — No Duplicate ACK Problem

📌 **Concept Name:** QUIC Uses Packet Sequence Numbers

🧠 **Simple Explanation:**

Another interesting feature from the transcript: **TCP uses duplicate acknowledgments**, but QUIC does **NOT** use duplicate acknowledgments.

In TCP, the sequence numbers are byte-based (byte sequence numbers). If a packet is retransmitted, the retransmitted packet has the **same sequence number** as the original packet. This can cause the "duplicate sequence number" problem and confusion in congestion detection.

QUIC does things differently:
- QUIC is **NOT a stream-oriented protocol** (since it runs over UDP)
- QUIC uses **packet sequence numbers** (not byte sequence numbers) for simplicity
- For every packet — including **retransmitted packets** — QUIC assigns a **new sequence number**
- This means the original packet and the retransmitted packet have **different sequence numbers**
- This eliminates the problem of duplicate sequence numbers and the confusion caused by duplicate acknowledgments

🎯 **Exam Important Points:**
- TCP uses byte sequence numbers; QUIC uses packet sequence numbers
- In TCP, retransmitted packets have the SAME sequence number as the original
- In QUIC, retransmitted packets get a NEW sequence number
- QUIC does not use duplicate acknowledgments like TCP
- This avoids the duplicate sequence number problem

⚠️ **Common Confusions:**
- QUIC still provides reliable delivery — just in a different way than TCP
- New sequence number for retransmission ≠ no retransmission. QUIC does retransmit, but with a fresh sequence number

---

## Concept 17: QUIC Deployment and the Future of Internet

📌 **Concept Name:** QUIC Deployment — Google, YouTube, Chromium

🧠 **Simple Explanation:**

The transcript mentions that QUIC is gradually getting popular on the internet:

- Many Google services like **YouTube** and **Google Drive** have started using QUIC
- Google has already started deployment of QUIC
- Current versions of **Chromium-based browsers** have the implementation of QUIC
- Many recent applications, especially from Google, have started using QUIC

The transcript says: **QUIC is possibly the future protocol which is going to replace the standard TCP-based data delivery.** When that happens, **UDP will actually become more important than TCP** because QUIC runs on top of UDP.

So the future of the internet might be: more UDP (through QUIC) and less TCP.

🎯 **Exam Important Points:**
- QUIC is deployed in YouTube, Google Drive, and Chromium-based browsers
- QUIC may replace standard TCP-based data delivery in the future
- This would make UDP more important than TCP in the future
- QUIC was developed by Google and published at SIGCOMM 2017

⚠️ **Common Confusions:**
- QUIC replacing TCP doesn't mean TCP disappears — it means QUIC could become the preferred transport for many applications
- QUIC makes UDP more important because QUIC runs on top of UDP

---

## Summary Table: TCP vs UDP vs QUIC

| Feature | TCP | UDP | QUIC |
|---|---|---|---|
| Connection Establishment | Yes (3-way handshake) | No | Yes (0-RTT / 1-RTT) |
| Reliable Delivery | Yes | No | Yes (at application level) |
| Flow Control | Yes | No | Yes (stream-wise) |
| Congestion Control | Yes | No | Yes (stream-wise) |
| Ordered Delivery | Yes | No | Yes (per-stream) |
| Encryption | No (needs SSL/TLS) | No | Built-in (end-to-end) |
| Head-of-Line Blocking | Yes (in HTTP/2) | No | No |
| Header Size | Min 20 bytes | 8 bytes | Variable |
| Underlying Protocol | IP | IP | UDP → IP |
| Sequence Numbers | Byte-based | None | Packet-based (new for retransmission) |

---

## Summary Table: UDP-Based Applications

| Protocol | Keyword | Why UDP? |
|---|---|---|
| DNS | Domain | Simple request-response, faster than TCP |
| BOOTP/DHCP | Network configuration | Short messaging, faster device configuration |
| TFTP | File transfer | Lightweight, for small files |
| SNMP | Network management | Cuts through congestion (TCP would reduce rate) |
| QUIC | Advanced transport | Direct access to IP, implements own reliability |

---

## 10 MCQs — Strictly from Lecture 23 Transcript

### Q1. What does UDP stand for?

(A) Universal Data Protocol  
(B) User Datagram Protocol  
(C) Unified Datagram Processing  
(D) User Data Processing  

**Answer: (B)**  
**Explanation:** As clearly stated in the transcript, UDP stands for User Datagram Protocol. It is a transport layer protocol that provides end-to-end datagram delivery.

---

### Q2. How many fields are in the UDP header?

(A) 6  
(B) 8  
(C) 4  
(D) 2  

**Answer: (C)**  
**Explanation:** The transcript shows that the UDP header has exactly 4 fields: Source Port, Destination Port, Length, and Checksum. This is much simpler than the TCP header.

---

### Q3. Which of the following services does UDP NOT provide?

(A) End-to-end datagram delivery  
(B) Connection establishment  
(C) Both (A) and (B)  
(D) Only (A)  

**Answer: (B)**  
**Explanation:** UDP does NOT provide connection establishment. It only provides end-to-end datagram delivery. UDP also does not provide reliable delivery, flow control, congestion control, or ordered packet delivery.

---

### Q4. What is the purpose of the checksum field in UDP?

(A) To provide security against attacks  
(B) To detect errors caused by network faults during transmission  
(C) To guarantee reliable delivery  
(D) To establish a connection  

**Answer: (B)**  
**Explanation:** The transcript clearly states that checksum is NOT for security attacks. It is to ensure packet integrity from network faults or system faults — such as bit flips during digital-to-analog conversion, modulation, or encoding/decoding at the physical layer.

---

### Q5. What is the pseudo header used for in TCP/UDP checksum computation?

(A) It is transmitted along with the packet for security  
(B) It is used only for checksum computation and is NOT transmitted  
(C) It replaces the IP header  
(D) It is stored at intermediate routers  

**Answer: (B)**  
**Explanation:** The transcript repeatedly emphasizes that the pseudo header is NOT transmitted with the packet. It is created only for computing the checksum (containing Source IP, Destination IP, Protocol, Reserved bits from the IP header) and is dropped after checksum computation.

---

### Q6. What is the full form of QUIC?

(A) Quick Unified Internet Connection  
(B) Quick UDP Internet Connection  
(C) Queue-based UDP Internet Control  
(D) Quick Universal Internet Connector  

**Answer: (B)**  
**Explanation:** The transcript clearly states that QUIC stands for Quick UDP Internet Connection. It was developed by Google and published at SIGCOMM 2017.

---

### Q7. In QUIC, which handshake type is used when a client connects to a server for the very first time?

(A) 0-RTT Handshake  
(B) 1-RTT Handshake  
(C) 3-Way Handshake  
(D) No Handshake needed  

**Answer: (B)**  
**Explanation:** The transcript explains that the initial handshake (first time connecting) requires 1-RTT. The client sends an Inchoate CHLO, server responds with REJ and security credentials, then the client sends a Complete CHLO with encrypted data. 0-RTT is only possible for subsequent connections.

---

### Q8. What is Head-of-Line (HoL) Blocking as described in the transcript?

(A) When a client blocks the server from sending data  
(B) When one lost packet in a multiplexed TCP connection blocks all streams from receiving data  
(C) When UDP packets arrive out of order  
(D) When DNS response is delayed  

**Answer: (B)**  
**Explanation:** The transcript explains that in HTTP/2 (SPDY), multiple streams are multiplexed over a single TCP connection. If one packet from one stream is lost, TCP will not deliver any received packets (even from other streams) to the application until the missing packet arrives. This blocks all streams.

---

### Q9. How does QUIC handle retransmitted packets differently from TCP?

(A) QUIC does not retransmit lost packets  
(B) QUIC assigns a new sequence number to retransmitted packets  
(C) QUIC uses the same sequence number as TCP  
(D) QUIC uses byte sequence numbers for retransmission  

**Answer: (B)**  
**Explanation:** The transcript states that QUIC uses packet sequence numbers (not byte sequence numbers) and for every retransmitted packet, QUIC assigns a NEW sequence number. This avoids the duplicate sequence number problem and the issues caused by duplicate acknowledgments in TCP.

---

### Q10. Why does QUIC NOT need a separate SSL/TLS layer?

(A) QUIC does not provide any encryption  
(B) QUIC uses the IP layer for encryption  
(C) QUIC has encryption built into the protocol — every QUIC packet is end-to-end encrypted  
(D) QUIC uses UDP's encryption features  

**Answer: (C)**  
**Explanation:** The transcript clearly states that in QUIC, the encryption part is embedded inside the QUIC protocol itself. Every QUIC packet is end-to-end encrypted, providing end-to-end security. So you do not need a separate SSL or TLS layer like you do with TCP.

---

*End of Lecture 23 — User Datagram Protocol (UDP) and QUIC Protocol*
