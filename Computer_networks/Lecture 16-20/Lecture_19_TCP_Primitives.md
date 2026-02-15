# Lecture 19: Transmission Control Protocol – I (Primitives)

**Course:** Computer Networks and Internet Protocol  
**Instructor:** Prof. Sandip Chakraborty, IIT Kharagpur  

---

## Overview of This Lecture

This lecture introduces the **Transmission Control Protocol (TCP)** — the most widely used transport layer protocol on the internet. More than **80% of internet traffic** uses TCP. The lecture covers TCP's purpose, its service model, socket concept, byte stream nature, TCP header structure, segment formation, and the challenges in TCP design.

---

## Concept 1: Why TCP Exists — Reliable Service Over Unreliable Network

### 📌 Concept Name
Purpose of TCP

### 🧠 Simple Explanation
The network layer (IP layer) is **unreliable**. Routers in the middle of the network can drop packets when their buffers overflow. The network layer does **not** take care of these lost packets. So, if we want reliable data delivery, the **transport layer** must handle it. TCP is that protocol — it provides **reliable end-to-end byte streaming** over this unreliable network.

Also, the underlying network is **heterogeneous** — meaning different parts of the path may use different technologies. For example, when you access Facebook from your mobile phone:

- **First hop:** Wireless (mobile to cell tower) — uses wireless protocols
- **Second hop:** Wired (cell tower to mobile switching center) — uses high-speed Ethernet
- **Third hop:** Optical fiber cable (connecting continents, e.g., Asia to USA)

Each of these links has different properties — different packet loss rates, different delays, different retransmission behaviors. TCP is designed to **handle all these different challenges** and still deliver data reliably.

### 🎯 Exam Important Points
- TCP provides **reliable** service over an **unreliable** internetwork
- Network layer (IP) is **unreliable** — it does not handle packet loss
- TCP must handle **heterogeneous** underlying networks (wireless, wired, optical)
- More than **80%** of internet traffic uses TCP

### ⚠️ Common Confusions
- Students sometimes think the network layer handles reliability — it does NOT. That is TCP's job.
- "Unreliable internetwork" does not mean the network is broken; it means IP does not guarantee delivery.

---

## Concept 2: History of TCP (RFCs)

### 📌 Concept Name
TCP Standards and Evolution

### 🧠 Simple Explanation
TCP has a long history. It did not stay the same since it was first created. Here is the timeline:

- **RFC 793 (September 1981):** The original/base TCP specification. RFC stands for **Request for Comments**, which is a standard document published by **IETF (Internet Engineering Task Force)** — a global body for protocol standardization.
- **RFC 1122:** Clarifications and bug fixes to TCP.
- **RFC 1323:** Extensions for **high-performance** TCP.
- **RFC 2018:** Introduced **TCP SACK (Selective Acknowledgement)** — which uses Selective Repeat protocol instead of just Go-Back-N for flow control.
- **RFC 2581:** Defines the **TCP congestion control** algorithm.
- **RFC 3168:** Introduces **Explicit Congestion Notification (ECN)**.

TCP has seen many amendments since 1981.

### 🎯 Exam Important Points
- Base TCP → **RFC 793** (September 1981)
- RFC stands for **Request for Comments**
- IETF = **Internet Engineering Task Force**
- TCP SACK → **RFC 2018** (Selective Acknowledgement using Selective Repeat)
- TCP Congestion Control → **RFC 2581**
- Explicit Congestion Notification → **RFC 3168**
- Standard TCP uses **Go-Back-N ARQ** for flow control; SACK uses **Selective Repeat**

### ⚠️ Common Confusions
- Don't confuse SACK (Selective Acknowledgement, RFC 2018) with general TCP ACK. SACK is an extension.
- The standard/default TCP uses Go-Back-N, not Selective Repeat.

---

## Concept 3: TCP Service Model — Full Duplex and Point-to-Point

### 📌 Concept Name
TCP Service Model

### 🧠 Simple Explanation
All TCP connections have two important properties:

1. **Full Duplex:** Both Host A and Host B can send data to each other **at the same time**. It is a two-way simultaneous communication.
2. **Point-to-Point:** TCP is designed for communication between exactly **2 end hosts** (one sender and one receiver). It was **NOT designed** for multicasting (one to many) or broadcasting (one to all).

### 🎯 Exam Important Points
- TCP = **Full Duplex** + **Point-to-Point**
- Full Duplex → both sides can send data simultaneously
- Point-to-Point → only between **two** hosts
- TCP does **NOT** support multicasting or broadcasting

### ⚠️ Common Confusions
- "Full duplex" does NOT mean one side sends while other waits. Both send at the same time.
- If a question asks about multicasting/broadcasting — TCP is NOT the answer.

---

## Concept 4: TCP Socket — The Logical Pipe

### 📌 Concept Name
TCP Socket and Its 6 Parameters

### 🧠 Simple Explanation
In Unix-based systems, TCP uses the concept of a **socket** to define an end-to-end connection. A socket is basically a **logical pipe** between two hosts through which data flows. Remember the "pipe" concept from the generic transport layer discussion — in TCP, that pipe is called a **socket**.

A socket is uniquely identified by **6 parameters (6-tuple)**:

1. **Source IP address**
2. **Source Port number**
3. **Source Initial Sequence Number**
4. **Destination IP address**
5. **Destination Port number**
6. **Destination Initial Sequence Number**

**How data flows through a socket:**

- Suppose Host B wants to send data to Host A.
- Host B uses the **write** system call to write data into the socket.
- This data travels through the protocol stack layers.
- It arrives at Host A's transport layer.
- Host A uses the **read** system call to read data from the transport layer buffer.
- The **transport layer** handles reliable delivery.
- The **network layer** handles delivering the packet to the correct IP address.

### 🎯 Exam Important Points
- Socket = **logical pipe** for TCP connection
- Socket is uniquely identified by **6 parameters** (source IP, source port, source ISN, destination IP, destination port, destination ISN)
- Data is written using **write** system call and read using **read** system call
- Transport layer → handles **reliability**
- Network layer → handles **delivery to correct IP**

### ⚠️ Common Confusions
- Socket is not just IP + Port. It has **6 parameters** including initial sequence numbers.
- "Pipe" and "Socket" refer to the same concept in different contexts (generic vs TCP).

---

## Concept 5: Internet Daemon (inetd)

### 📌 Concept Name
inetd — Internet Daemon Process

### 🧠 Simple Explanation
In Unix-based systems, there is a single daemon process called **inetd** (internet daemon) that runs all the time and listens on different **well-known ports**.

You don't need to keep a separate socket open for every service all the time. Instead, inetd handles it:

- inetd keeps listening on well-known ports.
- When the **first incoming connection** arrives, inetd **forks** — that means it creates a **child process** with a new process ID.
- This child process starts the corresponding daemon for that service.

**Examples:**
- For HTTP (web): inetd listens on **port 80**. When a connection request comes, it starts the **httpd** (HTTP daemon) process.
- For FTP (file transfer): The **ftpd** starts at **port 21**.

### 🎯 Exam Important Points
- **inetd** = internet daemon; runs on well-known ports
- On first incoming connection, inetd **forks** (creates child process)
- HTTP daemon (httpd) → **port 80**
- FTP daemon (ftpd) → **port 21**
- Fork = creating a child process with a new process ID

### ⚠️ Common Confusions
- inetd is not a specific application protocol. It is a **manager** that listens and then starts the right daemon.
- The daemon starts only when a connection request arrives — it does not run permanently from the beginning.

---

## Concept 6: TCP is a Byte Stream, NOT a Message Stream

### 📌 Concept Name
Byte Stream Nature of TCP

### 🧠 Simple Explanation
This is a very important concept. In TCP, **every byte is identified by a unique sequence number**. TCP does not think in terms of "messages" — it thinks in terms of **bytes**.

**Key point: Message boundaries are NOT preserved end-to-end.**

What does this mean? Let's look at an example from the transcript:

- Host B sends data to Host A in 3 segments:
  - Segment 1: bytes 101 to 200 (100 bytes)
  - Segment 2: bytes 201 to 250 (50 bytes)
  - Segment 3: bytes 251 to 400 (150 bytes)

- Suppose Segment 1 is received correctly, but Segments 2 and 3 are **lost**.

- Now, Host B needs to retransmit bytes 201 to 400.

- But during retransmission, TCP does NOT have to send the same 2 segments (50 bytes + 150 bytes). Instead, it might retransmit:
  - New Segment A: bytes 201 to 300 (100 bytes)
  - New Segment B: bytes 301 to 400 (100 bytes)

See what happened? The original segmentation (50 + 150) was **not preserved**. TCP only cares about **which bytes** need to be retransmitted, not the original segment boundaries.

### 🎯 Exam Important Points
- TCP = **byte stream** protocol, NOT message stream
- Every byte has a **unique sequence number**
- **Message boundaries are NOT preserved** end-to-end
- During retransmission, segment sizes can change
- TCP cares about **bytes**, not segments

### ⚠️ Common Confusions
- Don't think TCP retransmits the "same segment." It retransmits the **same bytes**, but possibly in different segment sizes.
- "Byte stream" means the unit of identification is the **byte**, not the message.

---

## Concept 7: Write and Read Calls — Sender and Receiver Buffers

### 📌 Concept Name
Data Transfer Through Buffers (Write/Read Calls)

### 🧠 Simple Explanation
Here is how the application interacts with TCP:

**At the Sender side:**
- The application uses the **write** system call to put data into the **sender buffer** (transport layer buffer).
- The TCP entity reads from this buffer and creates segments.

**At the Receiver side:**
- TCP receives the segments, reassembles the bytes, and puts them in the **receiver buffer**.
- The receiving application uses the **read** system call to read data from the receiver buffer.

**Important example from the transcript:**
- Suppose the sending application writes **four 512-byte blocks** to the socket.
- TCP may deliver this as:
  - Four 512-byte segments, OR
  - Two 1024-byte segments, OR
  - One 2048-byte segment, OR
  - Some other combination

The receiver has **no way to detect** the chunk size that the sender used when writing. The receiver might read in **1024-byte chunks** even though the sender wrote in **512-byte chunks**.

### 🎯 Exam Important Points
- Application writes data to **sender buffer** using write call
- TCP creates segments from the buffer — segment sizes can vary
- Receiver reads from **receiver buffer** using read call
- **Receiver does NOT know** what chunk size the sender used
- TCP can combine multiple write calls into one segment OR split one write call into multiple segments

### ⚠️ Common Confusions
- A single write call does NOT mean a single segment. TCP decides segment sizes independently.
- The receiver's read size can be completely different from sender's write size.

---

## Concept 8: TCP Header Structure

### 📌 Concept Name
TCP Segment Header Format

### 🧠 Simple Explanation
The TCP header has a **fixed 20-byte** part plus optional fields. Here are all the fields:

**1. Source Port (16 bits):** Identifies the sending application.

**2. Destination Port (16 bits):** Identifies the receiving application.

**3. Sequence Number (32 bits):** Identifies the byte number. Since TCP is a byte stream, every byte has a sequence number. This tells the receiver which byte this segment starts from.

**4. Acknowledgement Number (32 bits):** Tells the sender which byte the receiver is expecting next. This is a **cumulative acknowledgement** — it means all bytes before this number have been received correctly.

Example: If ACK number = 31245, it means the receiver has correctly received all bytes up to 31244 and is now expecting byte 31245.

**5. Header Length:** Tells how long the TCP header is.

**6. Flag Bits:** Special bits that indicate the type of message:
- **SYN** bit: Set to 1 for **connection initiation** (starting a connection)
- **FIN** bit: Set to 1 for **connection closure** (ending a connection)
- **ACK** bit: Set to 1 when the segment carries an **acknowledgement**

**7. Window Size (16 bits):** The **receiver advertised window size**. It tells the sender how much free buffer space the receiver currently has. This is used for **flow control** (sliding window protocol / dynamic buffer management).

- Window size = 0 means receiver buffer is full — sender must stop sending.

**8. Checksum:** Used to check the correctness of received data.

**9. Urgent Pointer:** If the urgent bit is set, this pointer indicates urgent data. Normally, TCP uses a FIFO (First In, First Out) queue — data that comes first is sent first. But if you set the urgent pointer (through socket programming), that data **bypasses the queue** and is sent immediately, with the urgent bit set to 1.

**10. Options (variable):** Optional fields.

**11. Data:** The actual payload from the upper layer (application data).

### 🎯 Exam Important Points
- TCP header = **fixed 20 bytes** + optional part
- Sequence Number = **32 bits**
- Acknowledgement Number = **32 bits**
- Window Size = **16 bits**
- Source Port and Destination Port = **16 bits each**
- SYN = connection initiation, FIN = connection closure, ACK = acknowledgement
- Window Size → used for **flow control** (receiver's free buffer space)
- Urgent Pointer → allows data to **bypass FIFO queue**
- Acknowledgement number = **cumulative** (all bytes before it are received)

### ⚠️ Common Confusions
- Sequence number identifies **bytes**, not packets. The professor specifically corrected this in the lecture.
- ACK number is the **next expected byte**, not the last received byte.
- Window size of 0 = receiver buffer full, sender must **stall** (stop sending).

---

## Concept 9: TCP Segments — Formation and Size

### 📌 Concept Name
How TCP Segments Are Created

### 🧠 Simple Explanation
A TCP segment = **fixed 20-byte header** + optional part + **0 or more bytes of data**.

- If it is a control message (like SYN or FIN), there may be **no data** — just the header.
- If it is a data message, there will be additional application data.

**How is segment size determined?**

TCP can combine data from several write calls into one segment, or split data from one write call into multiple segments. The segment size is restricted by **two parameters**:

1. **IP Payload limit:** The amount of data that can fit inside an IP fragment = **65,515 bytes** maximum.
2. **Maximum Transmission Unit (MTU):** Each link in the network has a maximum amount of data it can carry in one go. Different technologies have different MTUs:
   - Wi-Fi has one MTU
   - Ethernet has another MTU
   - Optical fiber has another MTU

**Path MTU Discovery:**
TCP uses a mechanism called **Path MTU Discovery** (part of **ICMP — Internet Control Message Protocol** at the network layer) to find the **smallest MTU** along the entire path from source to destination.

Example: If the path has links with MTUs of 512 bytes, 1KB, 1KB, and 256 bytes, then the effective MTU for the entire path = **256 bytes** (the smallest one). TCP should not send segments bigger than this.

This **Maximum Segment Size (MSS)** is set up during **connection establishment**.

### 🎯 Exam Important Points
- Segment = 20-byte fixed header + optional + data
- SYN and FIN messages have **no data** (only header)
- Segment size limited by: **IP payload (65,515 bytes)** and **MTU of links**
- **Path MTU Discovery** uses **ICMP** to find smallest MTU on path
- MSS (Maximum Segment Size) is set during **connection establishment**
- Different links have different MTUs based on technology

### ⚠️ Common Confusions
- MSS is not the same as MTU. MSS is the maximum segment size for TCP; MTU is the maximum transmission unit of a link.
- Path MTU = minimum MTU along the entire path, not the maximum.

---

## Concept 10: Sender Window — Congestion Window and Receiver Window

### 📌 Concept Name
Dynamic Sender Window Size

### 🧠 Simple Explanation
The application writes data into the TCP **sender buffer** using write calls. TCP then creates segments and sends them based on a **dynamic window**.

The **sender window size** is determined as:

**Sender Window = minimum(Congestion Window, Receiver Advertised Window)**

- **Congestion Window:** This is determined by the **congestion control** algorithm. The sender starts from a low rate and gradually increases the sending rate using **additive increase**. The window size represents how many bytes can be sent simultaneously. If window = 1, you send 1 byte; if window = 4, you send 4 bytes simultaneously. When congestion is detected, the window drops back to a small value.

- **Receiver Advertised Window:** The receiver tells the sender how much free buffer space it has (this comes in the Window Size field of the TCP header).

The sender takes the **minimum** of these two, because:
- You should not send faster than the **network** can handle (congestion window)
- You should not send faster than the **receiver** can accept (receiver window)

### 🎯 Exam Important Points
- Sender Window = **min(Congestion Window, Receiver Advertised Window)**
- Congestion Window → controlled by **congestion control** algorithm (additive increase)
- Receiver Window → comes from **Window Size** field in TCP header
- Sender window is **dynamically updated**
- When congestion is detected → window drops to small/minimum value

### ⚠️ Common Confusions
- The sender window is NOT just the receiver window. It is the **minimum** of congestion window and receiver window.
- Additive increase does not mean the window increases forever — it drops when congestion is detected.

---

## Concept 11: Segment Creation Algorithm

### 📌 Concept Name
How TCP Decides Segment Size Dynamically

### 🧠 Simple Explanation
After receiving an ACK, the sender checks the window. Here is the logic:

- If the **receiver advertised window size < MSS** (Maximum Segment Size): Create a single segment with whatever data fits in the receiver window.
  
- If the **receiver advertised window size ≥ MSS**: You can create multiple MSS-sized segments.
  - Example: Receiver window = 2048 bytes, MSS = 1024 bytes → You can create **2 segments of 1024 bytes** each.

**Special case:** If the sender buffer has very little data (say only 10 bytes), TCP does **NOT** wait until it accumulates MSS-worth of data. It sends whatever is available. Why? Because waiting would cause **unnecessary delay**.

So segment sizes are dynamically adapted based on:
1. Receiver advertised window
2. Maximum segment size (MSS)
3. Amount of data in the sender buffer

### 🎯 Exam Important Points
- After receiving ACK, sender checks window size
- If receiver window < MSS → make one small segment
- If receiver window ≥ MSS → make multiple MSS-sized segments
- TCP does **NOT wait** to fill up MSS if buffer has little data → avoids unnecessary delay
- Segment size is **dynamic** — changes based on conditions

### ⚠️ Common Confusions
- TCP does not always send MSS-sized segments. Small segments are allowed when buffer has less data.
- The segment size decision happens **after every ACK**, not just once.

---

## Concept 12: Challenges in TCP Design

### 📌 Concept Name
Key Challenges in TCP

### 🧠 Simple Explanation
Because segments are created dynamically, TCP faces several challenges:

**Challenge 1: Retransmission does NOT guarantee the same segments**
- As we saw earlier, original segments of 50 bytes + 150 bytes may be retransmitted as 100 bytes + 100 bytes.
- Retransmission may contain additional data, less data, or rearranged segments.

**Challenge 2: Out-of-order segments**
- TCP does not control the path — the **network layer** decides the routing.
- For different packets, the network layer may choose **different paths** (due to load balancing or routing protocols).
- Because of this, segments may arrive at the receiver **out of order**.

**Challenge 3: Handling out-of-order segments wisely**
- If we use Go-Back-N and throw away out-of-order segments (since sender will retransmit everything), that is **wasteful**.
- Better approach: **Keep the out-of-order segments in the buffer.**

**Example from the transcript:**
- Receiver has received bytes 100–120 and bytes 151–500, but bytes 121–150 are missing.
- When the sender retransmits (say bytes 121–160 in one segment), the receiver only needs to fill in the gap (bytes 121–150).
- Now the receiver has bytes 100–500 continuously. It can send a **cumulative ACK** saying "received up to byte 500."
- The sender can then stop retransmitting and move forward to send from byte 501 onwards.

This optimization saves network resources.

### 🎯 Exam Important Points
- Retransmission may NOT preserve original segment boundaries
- Segments can arrive **out of order** because network layer may use different paths
- Keeping out-of-order segments in buffer is better than discarding them (optimization)
- **Cumulative acknowledgement** allows the sender to know all bytes received so far
- This reduces unnecessary retransmissions and saves network resources

### ⚠️ Common Confusions
- Go-Back-N would discard out-of-order segments, but TCP can still keep them in buffer for optimization.
- Out-of-order arrival is NOT an error — it is normal because of different routing paths.

---

## Concept 13: Window Size Field and Flow Control

### 📌 Concept Name
Window Size for Flow Control

### 🧠 Simple Explanation
The **Window Size** field in the TCP header is used for the **flow control algorithm**. TCP uses a **variable-size sliding window protocol** (also called dynamic buffering).

- The window size tells the sender: **"This is how many bytes I (the receiver) can currently accept."**
- It is based on the **free buffer space** at the receiver.
- **Window size = 0** means the receiver has **no free buffer space**. The sender must **stall** (stop sending) until the receiver advertises a non-zero window.

The **final TCP acknowledgement** is a combination of:
1. **Acknowledgement number** (which bytes have been received)
2. **Advertised window size** (how much more the receiver can accept)

Based on both of these, the sender tunes its parameters for what to send next.

### 🎯 Exam Important Points
- Window Size field = **16 bits** in TCP header
- Used for **flow control** via variable-size **sliding window protocol**
- Window size indicates **receiver's free buffer space**
- Window size = 0 → receiver buffer full → sender must **stall**
- TCP ACK = Acknowledgement Number + Advertised Window Size → together they control sender behavior

### ⚠️ Common Confusions
- Window size is set by the **receiver**, not the sender.
- Window size = 0 does not mean connection is closed. It means "wait, my buffer is full."

---

## Summary Table of Key Numbers from This Lecture

| Item | Value |
|------|-------|
| TCP traffic share on internet | More than 80% |
| Base TCP RFC | RFC 793 (September 1981) |
| TCP header (fixed part) | 20 bytes |
| Sequence Number | 32 bits |
| Acknowledgement Number | 32 bits |
| Window Size field | 16 bits |
| Source/Destination Port | 16 bits each |
| Max IP payload | 65,515 bytes |
| HTTP port | 80 |
| FTP port | 21 |
| Socket parameters (tuple) | 6 |

---

## 10 MCQs — Strictly From Lecture 19

---

**Q1.** TCP was designed to provide reliable service over what kind of network?

A) Reliable network  
B) Unreliable internetwork  
C) Only wireless network  
D) Only wired network  

**Answer: B**  
**Explanation:** The transcript clearly states TCP was designed to provide "reliable end-to-end byte streaming over an unreliable internetwork." The IP-based network layer is unreliable because routers can drop packets due to buffer overflow.

---

**Q2.** How many parameters uniquely identify a TCP socket?

A) 2  
B) 4  
C) 5  
D) 6  

**Answer: D**  
**Explanation:** A TCP socket is identified by 6 parameters (6-tuple): source IP, source port, source initial sequence number, destination IP, destination port, and destination initial sequence number.

---

**Q3.** Which of the following is TRUE about TCP connections?

A) TCP supports multicasting  
B) TCP is half-duplex and point-to-point  
C) TCP is full-duplex and point-to-point  
D) TCP supports broadcasting  

**Answer: C**  
**Explanation:** The transcript states "all TCP connections they have full duplex and point to point." TCP was NOT designed for multicasting or broadcasting.

---

**Q4.** In TCP, the acknowledgement number 31245 means:

A) Byte 31245 has been received  
B) All bytes up to 31245 have been received  
C) All bytes up to 31244 have been received and byte 31245 is expected next  
D) 31245 segments have been acknowledged  

**Answer: C**  
**Explanation:** TCP uses cumulative acknowledgement. The acknowledgement number contains the next expected byte. So ACK = 31245 means all bytes up to 31244 are correctly received, and byte 31245 is expected next.

---

**Q5.** The fixed part of a TCP header is:

A) 10 bytes  
B) 16 bytes  
C) 20 bytes  
D) 32 bytes  

**Answer: C**  
**Explanation:** The transcript states "a segment consists of a fixed 20-byte header plus an optional part."

---

**Q6.** Which RFC introduced TCP Selective Acknowledgement (SACK)?

A) RFC 793  
B) RFC 1323  
C) RFC 2018  
D) RFC 2581  

**Answer: C**  
**Explanation:** The transcript states RFC 2018 uses selective acknowledgement (TCP SACK), which uses selective repeat protocol for flow control.

---

**Q7.** What does a TCP window size of 0 indicate?

A) Connection is closed  
B) Receiver has no free buffer space  
C) Sender has no data to send  
D) Network is congested  

**Answer: B**  
**Explanation:** The transcript says "window size 0 means, the receiver does not have a sufficient buffer space. So, the sender should stall transmitting further data."

---

**Q8.** TCP determines the Maximum Segment Size (MSS) using which mechanism?

A) ARP Discovery  
B) DNS Resolution  
C) Path MTU Discovery (via ICMP)  
D) DHCP Configuration  

**Answer: C**  
**Explanation:** The transcript explains that TCP uses "path MTU discovery" which is part of ICMP (Internet Control Message Protocol) to find the MTU of all links in the path, and MSS is set during connection establishment.

---

**Q9.** If the sending application writes four 512-byte blocks to the TCP socket, how will TCP deliver them?

A) Always as four 512-byte segments  
B) Always as one 2048-byte segment  
C) The delivery segment sizes can vary — TCP may combine or split the data  
D) Always as two 1024-byte segments  

**Answer: C**  
**Explanation:** The transcript states this data "may be delivered as four 512 bytes chunks… two 1024 bytes segments or one 2048 byte segments or in some other way." The receiver has no way to detect the sender's original write sizes.

---

**Q10.** In TCP, why can segments arrive out of order at the receiver?

A) Because TCP uses different sequence numbers  
B) Because the sender sends them out of order  
C) Because the network layer may route different packets on different paths  
D) Because the receiver buffer is full  

**Answer: C**  
**Explanation:** The transcript explains that "TCP does not determine the path, the network layer is determining the path. And the network layer it may happen that for one segment… it decides one path for another packet it decides another path. Because of this load balancing or many other mechanism in the routing protocol."

---

## What Else Is Covered in Upcoming Lectures

The transcript ends by saying: "In the next class, we will go to the details of this **sliding window based flow control algorithm** which is adopted as part of TCP."

So **Lecture 20** will cover the **TCP sliding window flow control** in detail.

---

*End of Lecture 19 Notes*
