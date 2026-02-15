# Lecture 12: Transport Layer — II (Connection Establishment)

**Course:** Computer Networks and Internet Protocol  
**Instructor:** Prof. Sandip Chakraborty, IIT Kharagpur

---

## What This Lecture Covers (Overview)

In the previous lecture (Lecture 11), we learned that the transport layer provides end-to-end services on top of the unreliable network layer. Starting from this lecture, we go into the **details** of those services. The **first service** discussed is **Connection Establishment** — how two devices set up a logical connection before exchanging data.

---

## Concept 1: Why Connection Establishment Is Needed

### 📌 Concept Name
**Connection Establishment — The Logical Pipe**

### 🧠 Simple Explanation
Before two devices (client and server) start sending actual data, they first need to check:

- Is the other end **alive**?
- Is the other end **ready to receive** messages?

Think of it like making a phone call. You say "Hello" and wait for the other person to say "Hello" back. Only after both sides confirm, you start talking. This "Hello — Hello" process creates a **logical connection** (a logical pipe) between the two ends.

In a **telephone network** (circuit switching), this is easy because the path is dedicated and reliable — your "Hello" always reaches the other side.

But in a **packet switching network** (like the Internet), this is **much harder** because:

- Packets can be **lost** (dropped from router buffers when network is congested)
- Packets can be **delayed** (stuck in queues at routers)
- Packets can be **corrupted**
- Packets can be **duplicated** (because of retransmission after timeout)

### 🛠 Real-world Example (from transcript)
Think of a **road junction**. Cars come from multiple roads into one junction. If the junction has limited capacity, there is a traffic jam (congestion). Cars slow down. Similarly, in a network, a router receives packets from many links. If its buffer is full, packets slow down or get dropped.

### 🎯 Exam Important Points
- Connection establishment creates a **logical pipe** between two ends.
- The network layer provides **unreliable datagram delivery** — packets can be lost, delayed, corrupted, or duplicated.
- The transport layer must handle all these problems while establishing a connection.
- Circuit switching (telephone) = reliable delivery, so connection is easy.
- Packet switching (Internet) = unreliable delivery, so connection establishment is **non-trivial**.

### ⚠️ Common Confusions
- Connection establishment does NOT mean a physical wire is set up. It is a **logical** agreement between two ends.
- Duplicates happen because of **retransmission** — the sender thinks the packet was lost (due to timeout), but it was actually just delayed.

### 📝 Possible NPTEL-style Questions
- Why is connection establishment harder in packet switching than circuit switching?
- What are the four problems that can happen to packets in a packet switching network?

---

## Concept 2: Naive (Two-Way Handshake) Protocol — And Why It Fails

### 📌 Concept Name
**Two-Way Handshake — A Simple But Flawed Approach**

### 🧠 Simple Explanation
The simplest way to establish a connection:

1. The **server** is in a **LISTEN** state (waiting for connections).
2. The **client** sends a **Connection Request** message.
3. The **server** receives it and sends back a **Connection Acknowledgement** message.

This is called a **two-way handshake**. It works like the "Hello — Hello" in a phone call.

**But does this work in a packet switching network?** The answer is **NO, not reliably.**

The reason is: because of packet loss, delay, and duplication, the server might receive **two copies** of a connection request. The server then faces a big problem — it cannot tell whether the second request is:

- A **new connection request** (maybe the client crashed and restarted), OR
- A **delayed duplicate** of the old connection request.

### 🛠 Real-world Example (from transcript)
Imagine the client sends Connection Request 1. Then the client **crashes** and restarts. Now the client sends Connection Request 2 (a genuinely new request). But the server does not know about the crash. It receives both requests and cannot figure out which is old and which is new.

### 🎯 Exam Important Points
- Two-way handshake: Client sends request → Server sends acknowledgement.
- This **does NOT work well** in packet switching networks because of **delayed duplicates**.
- The server cannot distinguish between a **new request** and a **delayed duplicate**.
- The client crash scenario makes this problem even worse — the server has no way to know the client crashed.

### ⚠️ Common Confusions
- Two-way handshake is NOT the same as TCP's three-way handshake. Two-way is the naive/simple version that **fails**.
- The problem is not just about packet loss — it is specifically about **delayed duplicates** creating confusion.

### 📝 Possible NPTEL-style Questions
- Why does a two-way handshake fail in a packet switching network?
- What confusion does a delayed duplicate create for the server?

---

## Concept 3: The Core Problem — Delayed Duplicates

### 📌 Concept Name
**Delayed Duplicates — The Major Challenge**

### 🧠 Simple Explanation
A **delayed duplicate** is when an old packet (that the sender already retransmitted because it thought the packet was lost) finally arrives at the receiver — **late**.

Here is what happens step by step:

1. Client sends a packet.
2. The packet gets **stuck** in a congested queue at some router.
3. The client **times out** (does not get acknowledgement in time).
4. The client thinks the packet is **lost** and **retransmits** it.
5. Now the retransmitted packet reaches the server.
6. **Later**, the original stuck packet also reaches the server.
7. The server now has **two copies** (a duplicate) of the same packet.

This is the **biggest challenge** in connection establishment in packet switching networks.

### 🎯 Exam Important Points
- Delayed duplicates happen because of **retransmission after timeout**.
- The original packet was NOT lost — it was just **delayed** in the network.
- A major challenge is to develop protocols that can **handle delayed duplicates** correctly.
- There is always a debate: **Protocol Correctness vs. Protocol Performance**.
  - If you want 100% correctness (handle all delayed duplicates), the protocol becomes complex and slow.
  - If you want good performance, you may compromise on correctness (accept some failure scenarios).

### ⚠️ Common Confusions
- Delayed duplicate is NOT the same as a corrupted packet. A delayed duplicate is a perfectly valid packet — it just arrived too late.
- The problem is not just receiving extra data — it is about the server **misinterpreting** what the duplicate means.

### 📝 Possible NPTEL-style Questions
- What is a delayed duplicate in a packet switching network?
- Why is handling delayed duplicates described as "an eternal debate" in protocol design?

---

## Concept 4: Solution 1 — Throwaway Transport Address (Port Numbers)

### 📌 Concept Name
**Solution 1: Use Throwaway Port Numbers**

### 🧠 Simple Explanation
**Idea:** Every time a connection crashes and restarts, use a **different port number**. This way, the delayed duplicate (which was sent to the old port) will arrive at the old port number, and the transport layer will **discard** it because no application is listening on that old port anymore.

**How it works (example from transcript):**

- Application A1 starts a connection on **port 8080**.
- Application A1 crashes.
- Application A1 restarts and now uses **port 8082**.
- If a delayed duplicate reply comes on port 8080, the transport layer cannot deliver it (no app on 8080) — so it is **correctly discarded**.
- If a reply comes on port 8082, it is the **correct** new reply.

**Why this solution is NOT feasible:**

- We have a **finite number** of port numbers.
- You cannot throw away a port number after using it once — you would need **infinite port numbers**, which is impossible.
- Many applications need to send data, so ports are a limited and shared resource.

### 🎯 Exam Important Points
- Solution 1 = Never reuse a port number once it has been used.
- This prevents delayed duplicates from reaching the wrong process.
- **Not feasible** because port numbers are **finite** (we cannot have infinite ports).

### ⚠️ Common Confusions
- This solution theoretically works but is **practically impossible** due to the finite number of ports.

### 📝 Possible NPTEL-style Questions
- Why is the throwaway transport address approach not feasible for handling delayed duplicates?

---

## Concept 5: Solution 2 — Unique Connection Identifier

### 📌 Concept Name
**Solution 2: Give Each Connection a Unique Identifier**

### 🧠 Simple Explanation
**Idea:** The initiating party (client) generates a **unique identifier** for each connection and puts it in every segment/packet.

**Why this solution has problems:**

- You need to ensure the identifier is **globally unique** — no two connections should ever have the same ID.
- Designing an algorithm to generate such a unique ID is difficult.
- Even after a **system crash and recovery**, the system must NOT reuse an old identifier. This requires some kind of **hardware support** to remember what IDs were used before the crash.
- This adds significant **overhead**.

### 🎯 Exam Important Points
- Solution 2 = Unique identifier chosen by the initiating party, placed in every segment.
- Problem: Ensuring **global uniqueness** is very hard, especially after crashes.
- Requires hardware-level support to survive crashes — adds overhead.

### ⚠️ Common Confusions
- This is different from sequence numbers. This is about a **connection-level** unique ID, not per-packet sequence numbers.

### 📝 Possible NPTEL-style Questions
- What is the main problem with using a unique connection identifier to handle delayed duplicates?

---

## Concept 6: Solution 3 — Restrict Packet Lifetime (The Feasible Solution)

### 📌 Concept Name
**Solution 3: Kill Off Aged Packets — Restrict Packet Lifetime**

### 🧠 Simple Explanation
**The core idea:** If delayed duplicates are the problem, then **eliminate them**. How? By ensuring that every packet has a **maximum lifetime** in the network. After that lifetime expires, the packet is **automatically killed/dropped**.

If you can guarantee that old packets are dead before you start a new connection, then there will be no confusion between old and new packets.

**This makes it possible to design a feasible solution.**

### Three Ways to Restrict Packet Lifetime

**Way 1: Restricted Network Design**
- Design the network so that packets cannot loop forever.
- Put a bound on the maximum delay (including congestion delay).
- If a packet exceeds this time from its origin, it is dropped.

**Way 2: Hop Count in Each Packet (Most Feasible — Used in Today's Networks)**
- Every packet carries a **hop count** value (set to a maximum, e.g., 10).
- Every time the packet passes through a router (one hop), the hop count is **decremented by 1** (10 → 9 → 8 → ...).
- When the hop count reaches **0**, the router **drops the packet**.
- This ensures no packet travels forever in the network.

**Way 3: Timestamping Each Packet**
- Each packet carries a **timestamp** defining its lifetime.
- After the lifetime expires, the packet is dropped.
- **Problem:** This requires **time synchronization** across all routers, which is very difficult because of **clock drift** between different systems.

### 🎯 Exam Important Points
- Solution 3 (restrict packet lifetime) is the **most practical and feasible** approach.
- Three methods: Restricted network design, Hop count, Timestamping.
- **Hop count** is the most commonly used method in practice.
- Timestamping requires clock synchronization — **difficult in real networks** due to clock drift.
- The hop count approach is used in real networks (similar to TTL — Time to Live in IP).

### ⚠️ Common Confusions
- Hop count is NOT about time — it counts the **number of routers** the packet passes through, not seconds.
- Timestamping sounds better but is impractical because perfect clock synchronization across all routers is nearly impossible.

### 📝 Possible NPTEL-style Questions
- What are the three ways to restrict packet lifetime?
- Why is the hop count method the most feasible?
- Why is timestamping impractical for restricting packet lifetime?

---

## Concept 7: The Design Challenge — Packet AND Its Acknowledgements Must Be Dead

### 📌 Concept Name
**Design Challenge: Killing Both Packets and Their Acknowledgements**

### 🧠 Simple Explanation
It is NOT enough to just ensure the original packet is dead. You also need to ensure that all **acknowledgements** related to that packet are also dead.

**Why?** Consider this scenario:

1. Client sends a Connection Request (old).
2. Client **crashes** and restarts.
3. Client sends a **new** Connection Request.
4. But then the client receives a **reply** — and it cannot tell if this reply is for the **old request** or the **new request**.

If the reply to the old request is still alive in the network, the client gets confused. So, you must guarantee that when you start a new connection, both the old packet **and** all replies/acknowledgements to it are completely gone from the network.

### 🎯 Exam Important Points
- We need to guarantee that not only is the **packet** dead, but **all acknowledgements to it** are also dead.
- This is a critical design requirement for connection establishment protocols.
- If old acknowledgements survive, they can confuse the sender about which request the reply belongs to.

### ⚠️ Common Confusions
- Many students forget about the acknowledgement part — they think killing the original packet is enough. It is NOT.

### 📝 Possible NPTEL-style Questions
- Why must we ensure that both a packet and its acknowledgements are dead before reusing a sequence number?

---

## Concept 8: Maximum Packet Lifetime T and Virtual Clock (Sequence Numbers)

### 📌 Concept Name
**Maximum Packet Lifetime T and Sequence Numbers as Virtual Clock**

### 🧠 Simple Explanation
We define a **maximum packet lifetime T**. If we wait for T seconds after sending a packet, we can be **sure** that the packet and all its acknowledgements are gone from the network.

Now, instead of using a **physical clock** (which requires clock synchronization — very hard on the Internet), we use a **virtual clock**. This virtual clock works through **sequence numbers** generated based on clock ticks.

**Key principle:** Every packet is labeled with a **sequence number**, and that sequence number is **not reused within T seconds**. This ensures that at any given time, only **one packet** with a particular sequence number exists in the network.

**Example from transcript:**
- You send a packet with sequence number **125** and T = 1 minute.
- For the next 1 minute, you do NOT send any other packet with sequence number 125.
- After 1 minute, sequence number 125's packet is guaranteed to be dead.
- So, when the receiver gets a packet with sequence number 125, it knows this is the **only** such packet in the network — not a delayed duplicate.

### 🎯 Exam Important Points
- **T** = Maximum packet lifetime. After T seconds, all traces (packet + acknowledgements) are dead.
- **Virtual clock** = Sequence numbers generated based on clock ticks (avoids need for clock synchronization).
- A sequence number is **not reused within T seconds**.
- At most **one packet** with a given sequence number may be outstanding at any time.
- The **period T** and the **rate of packets per second** together determine the **size of the sequence number** field needed.

### ⚠️ Common Confusions
- Virtual clock ≠ physical clock. Virtual clock uses sequence numbers, not actual time.
- The rate of sending packets matters — if you send very fast, you need a larger sequence number space so you don't run out before T expires.

### 📝 Possible NPTEL-style Questions
- What is the maximum packet lifetime T and what does it guarantee?
- Why is a virtual clock (sequence number) used instead of a physical clock?
- What determines the size of the sequence number field?

---

## Concept 9: Tomlinson's Two Requirements for Sequence Numbers (1975)

### 📌 Concept Name
**Tomlinson's Two Requirements for Selecting Sequence Numbers**

### 🧠 Simple Explanation

Tomlinson published a landmark work in 1975 titled "Selecting Sequence Numbers" which defined two key requirements:

**Requirement R1: No Sequence Number Reuse**
- A particular sequence number must **never refer to more than one byte** at any given time.
- TCP uses **byte sequence numbers** (not packet sequence numbers). This means every individual **byte** in the data gets its own sequence number.

**How byte sequence numbering works (from transcript):**
- Say a packet has 100 bytes of data.
- The header has two fields: **Sequence Number** and **Length**.
- If sequence number = 500 and length = 100, then this packet contains bytes from **501 to 600**.
- The next packet might start at sequence number 601.

**Byte Sequence Number vs. Packet Sequence Number:**
- **Packet sequence number:** Each packet gets ONE sequence number (like packet #1, packet #2...).
- **Byte sequence number:** Each byte within a packet gets a sequence number (byte #500, byte #501...). TCP uses this approach.

**Requirement R2: Positive Synchronization of Sequence Numbers**
- The valid range of sequence numbers must be **positively synchronized** between sender and receiver whenever a connection is used.
- Once the initial sequence number is agreed upon during connection establishment, all subsequent packets follow that sequence.
- This synchronization is ensured by the **flow control mechanism** (discussed in later lectures).

### 🛠 Example from transcript
- Client sends a request with initial sequence number = **1000**.
- Server accepts and acknowledges initial sequence number 1000.
- Now the data transfer follows:
  - Packet 1: Sequence number = 1001, Length = 50 (bytes 1001–1050)
  - Packet 2: Sequence number = 1051, Length = 100 (bytes 1051–1150)
  - Packet 3: Sequence number = 1151, Length = 50 (bytes 1151–1200)
- The flow control algorithm manages this sequence.

### 🎯 Exam Important Points
- **R1:** A sequence number must never refer to more than one byte at any time (for byte sequence numbers).
- **R2:** The valid range of sequence numbers must be positively synchronized between sender and receiver.
- TCP uses **byte sequence numbers**, not packet sequence numbers.
- The header contains: **Sequence Number** + **Length** → together they identify which bytes are in the packet.
- The flow control algorithm ensures R2 — once the initial sequence number is set, all subsequent packets follow the sequence.
- Tomlinson, 1975, "Selecting Sequence Numbers" — remember this reference.

### ⚠️ Common Confusions
- Byte sequence number does NOT mean one separate header per byte. It means the **starting byte number** is in the header, and the length field tells how many bytes follow.
- R2 is handled by flow control, NOT by the connection establishment protocol directly.

### 📝 Possible NPTEL-style Questions
- What are Tomlinson's two requirements for selecting sequence numbers?
- What is the difference between byte sequence numbering and packet sequence numbering?
- If a packet has sequence number 500 and length 100, which bytes does it contain?

---

## Concept 10: Choosing the Initial Sequence Number — The Forbidden Zone

### 📌 Concept Name
**Initial Sequence Number Selection and the Forbidden Range**

### 🧠 Simple Explanation
The biggest challenge is: **How to choose the initial sequence number?**

Once the initial handshake is done and the initial sequence number is set, the subsequent sequence numbers are easy (handled by flow control). But choosing the **first** sequence number is the hard part.

**The Problem:**
- Connection 1 uses certain sequence numbers (say starting from some value).
- Those packets have a lifetime T in the network.
- If Connection 1 crashes and Connection 2 starts, the sequence numbers used by Connection 2 must **NOT overlap** with the sequence numbers still alive from Connection 1.

**If there is overlap**, a delayed duplicate from Connection 1 could be mistaken for a valid packet of Connection 2 — causing confusion.

**Two ways to avoid overlap:**

1. **Wait for sufficient time** — After Connection 1 ends/crashes, wait long enough (at least T duration) so that all packets from Connection 1 are dead. Then start Connection 2 with any sequence number.

2. **Use a high enough initial sequence number** — Choose an initial sequence number for Connection 2 that is far enough ahead from Connection 1's sequence numbers, so their ranges do not overlap.

**Forbidden Range:**
- The range of sequence numbers already used by a previous connection (and still potentially alive in the network) is called the **forbidden range**.
- Connection 2 must NOT use any sequence number that falls in the forbidden range of Connection 1.
- You must avoid **overlapping of forbidden zones** between two different connections.

### 🎯 Exam Important Points
- Choosing the **initial sequence number** is the critical challenge during connection establishment.
- Subsequent sequence numbers are handled by flow control — they are known once the initial is set.
- The **forbidden range** = sequence numbers used by a previous connection that may still be alive.
- Two solutions: (1) Wait for time T so old packets die, OR (2) Choose a high enough initial sequence number.
- The forbidden zones of two connections must **never overlap**.
- The detailed mechanism for selecting initial sequence numbers is covered in the **next lecture**.

### ⚠️ Common Confusions
- The forbidden range is NOT permanent — it expires after time T.
- You do NOT need to remember all old sequence numbers forever — just ensure no overlap within the lifetime T.

### 📝 Possible NPTEL-style Questions
- What is the forbidden range in the context of initial sequence number selection?
- How can you ensure that the sequence number zones of two connections do not overlap?
- Why is choosing the initial sequence number the most critical part of connection establishment?

---

## Summary of Lecture 12

| # | Topic | Key Takeaway |
|---|-------|-------------|
| 1 | Why connection establishment is needed | To confirm both ends are alive and ready; harder in packet switching due to unreliable delivery |
| 2 | Two-way handshake | Simple but fails because of delayed duplicates |
| 3 | Delayed duplicates | The core problem — old packets arriving late cause confusion |
| 4 | Solution 1: Throwaway ports | Works in theory, not feasible (finite ports) |
| 5 | Solution 2: Unique identifier | Hard to ensure global uniqueness, especially after crashes |
| 6 | Solution 3: Restrict packet lifetime | The feasible solution — hop count is most practical |
| 7 | Kill packets AND acknowledgements | Both must be dead before starting a new connection |
| 8 | Lifetime T and virtual clock | Sequence numbers as virtual clock; not reused within T |
| 9 | Tomlinson's requirements | R1: unique byte reference; R2: positive synchronization |
| 10 | Initial sequence number and forbidden zone | Must avoid overlapping forbidden ranges between connections |

---

## 10 MCQs — Strictly from Lecture 12

---

**Q1.** In a packet switching network, which of the following problems does NOT occur during packet delivery?

(A) Packet loss  
(B) Packet delay  
(C) Dedicated path for each packet  
(D) Packet duplication  

**Answer: (C)**  
**Explanation:** In a packet switching network, there is NO dedicated path. Packets can be lost, delayed, corrupted, or duplicated. Dedicated paths are a feature of circuit switching networks (like telephone networks).

---

**Q2.** Why does a two-way handshake fail in a packet switching network?

(A) Because the server cannot send acknowledgements  
(B) Because the server cannot distinguish between a new connection request and a delayed duplicate  
(C) Because the client always crashes  
(D) Because port numbers are not available  

**Answer: (B)**  
**Explanation:** The transcript explains that in a two-way handshake, if the server receives two connection request messages, it cannot determine whether the second one is a new request (e.g., after a client crash and restart) or a delayed duplicate of the first one.

---

**Q3.** Why is the "throwaway transport address (port number)" approach not feasible?

(A) Port numbers are too large  
(B) Port numbers are infinite  
(C) Port numbers are finite, so you cannot discard them after one use  
(D) Port numbers cannot be assigned to applications  

**Answer: (C)**  
**Explanation:** The transcript clearly states that we have a finite number of port numbers. If we throw away a port after each use, we would need infinite port numbers, which is not practically feasible.

---

**Q4.** Which of the following methods of restricting packet lifetime is the most feasible and commonly used?

(A) Timestamping each packet  
(B) Restricted network design  
(C) Putting a hop count in each packet  
(D) Using throwaway port numbers  

**Answer: (C)**  
**Explanation:** The transcript states that putting a hop count in each packet is the "most feasible implementation" and is used in today's networks. Timestamping requires clock synchronization, which is difficult.

---

**Q5.** In the hop count method, what happens when the hop count of a packet reaches 0?

(A) The packet is retransmitted  
(B) The packet is sent back to the sender  
(C) The router drops (discards) the packet  
(D) The packet is delivered to the destination  

**Answer: (C)**  
**Explanation:** As stated in the transcript, when the hop count becomes 0, the router that receives the packet simply drops it. This prevents the packet from travelling in the network forever.

---

**Q6.** Why is timestamping NOT practical for restricting packet lifetime in a network?

(A) Timestamps are too large to fit in a packet header  
(B) It requires time synchronization across all routers, which is difficult due to clock drift  
(C) Timestamps cannot be read by routers  
(D) Timestamps increase the packet size to more than the MTU  

**Answer: (B)**  
**Explanation:** The transcript explains that timestamping requires proper time synchronization among all devices, which is very difficult to achieve in real scenarios due to clock drift between systems.

---

**Q7.** What does "maximum packet lifetime T" guarantee?

(A) That the packet will reach the destination within T seconds  
(B) That after T seconds, the packet and all its acknowledgements are dead (gone from the network)  
(C) That the packet will be retransmitted T times  
(D) That the connection will last for T seconds  

**Answer: (B)**  
**Explanation:** The transcript defines T as the maximum packet lifetime, and states that if we wait T seconds after a packet is sent, we can be sure that all traces of it (the packet and its acknowledgements) are gone from the network.

---

**Q8.** TCP uses which type of sequence numbering?

(A) Packet sequence numbering  
(B) Byte sequence numbering  
(C) Frame sequence numbering  
(D) Segment sequence numbering  

**Answer: (B)**  
**Explanation:** The transcript explicitly states that TCP uses byte sequence numbers, where every individual byte has a sequence number, rather than packet sequence numbers where each packet gets one number.

---

**Q9.** If a TCP packet has sequence number 500 and length 100, which bytes does this packet contain?

(A) Bytes 400 to 500  
(B) Bytes 500 to 600  
(C) Bytes 501 to 600  
(D) Bytes 500 to 599  

**Answer: (C)**  
**Explanation:** The transcript gives this exact example — if the sequence number is 500 and the length is 100 bytes, the packet contains data from byte 501 to byte 600 (100 bytes total).

---

**Q10.** What is the "forbidden range" in the context of initial sequence number selection?

(A) The range of port numbers that cannot be used  
(B) The range of sequence numbers from a previous connection that may still be alive in the network and must not be reused  
(C) The range of IP addresses that are blocked  
(D) The range of hop count values that are invalid  

**Answer: (B)**  
**Explanation:** The transcript explains that the sequence numbers used by a previous connection, which may still exist in the network within the lifetime T, form the "forbidden range." A new connection must not use sequence numbers that fall in this range, to avoid confusion with delayed duplicates.

---

## What Comes Next — Lecture 13

The transcript ends by saying that in the **next lecture**, the detailed mechanism for **selecting the initial sequence number** will be discussed — specifically how to avoid the overlapping of forbidden zones between two different connections.

---

*These notes are strictly based on the Lecture 12 transcript of NPTEL Computer Networks and Internet Protocol by Prof. Sandip Chakraborty, IIT Kharagpur.*
