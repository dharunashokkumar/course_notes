# Lecture 14 — Transport Layer – IV (Reliability & Flow Control)

**Course:** Computer Networks and Internet Protocol  
**Instructor:** Prof. Sandip Chakraborthy, IIT Kharagpur  

---

## Topics Covered in This Lecture

1. Flow Control and Reliable Data Delivery — Introduction  
2. Why Network Layer is Unreliable  
3. Ensuring Reliability at the Transport Layer  
4. Why Flow Control and Error Control Exist at BOTH Data Link Layer and Transport Layer  
5. Hop-by-Hop vs End-to-End Flow Control — With Example  
6. End-to-End Arguments in System Design (Reference Paper)  
7. Stop and Wait Flow Control — Error Free Channel  
8. Stop and Wait Flow Control — Noisy Channel (with Sequence Numbers)  
9. Automatic Repeat Request (ARQ)  
10. Stop and Wait ARQ — Sender Side State Transition Diagram  
11. Problems with Stop and Wait  
12. Piggybacking  
13. Introduction to Sliding Window Protocol (Pipelined Protocol)  

---

## Concept 1: Flow Control and Reliable Data Delivery — Introduction

📌 **Concept Name:** Flow Control & Reliability in Transport Layer

🧠 **Simple Explanation:**

In the previous lectures, you learned about connection establishment and connection termination in the transport layer. Now, this lecture talks about the **second important service** of the transport layer — **Flow Control** and **Reliability**.

Imagine two computers are connected end-to-end. After they set up a connection, one computer (sender) starts sending data to the other (receiver). Now, two problems can happen:

- **Problem 1 (Flow Control):** The sender might send data too fast, faster than the receiver can handle. This is bad. So, the sender should always know how much data the receiver can accept and should adjust its speed. This is called **flow control**.

- **Problem 2 (Reliability):** The lower layers (like the network layer) are unreliable. They might lose some data on the way. The transport layer must make sure that **every piece of data eventually reaches the receiver**, even if some data gets lost in between.

In TCP, data is sent as a **sequence of bytes**. In some other transport protocols, it can be a **sequence of packets or segments**.

🛠 **Real-world Example:**

Think of a teacher (sender) dictating notes to a student (receiver). If the teacher speaks too fast, the student cannot write everything — that's a flow control problem. If some words are not heard properly, the student asks the teacher to repeat — that's a reliability problem.

🎯 **Exam Important Points:**
- Flow control ensures the sender does not send more data than the receiver can handle.
- Reliability ensures all data eventually reaches the receiver.
- In TCP, data is sent as a sequence of bytes.
- Flow control and reliability are implemented together in the transport layer.

⚠️ **Common Confusions:**
- Flow control is NOT about how fast the network can carry data. It is about how fast the **receiver** can accept data.
- Reliability is NOT guaranteed by the network layer. The transport layer adds reliability on top.

---

## Concept 2: Why the Network Layer is Unreliable

📌 **Concept Name:** Unreliable Nature of Network Layer

🧠 **Simple Explanation:**

The network layer's job is to forward packets from the source to the destination. But the network layer **does not guarantee** that every packet will arrive at the destination. It only makes a **"best effort"** — meaning it tries its best to deliver, but some packets might be lost.

Why can packets get lost? Because in a **packet switching network**, the intermediate routers have **buffers (temporary storage)**. When too much data comes in, these buffers get full and start **dropping packets**. This is called **buffer overflow**.

So, the transport layer's job is to **detect** whether data reached the other end correctly, and if not, to **retransmit** the lost data so that eventually the receiver gets everything.

🛠 **Real-world Example:**

Imagine you are sending letters through the post office. The post office (network layer) tries to deliver your letter, but sometimes a letter gets lost. Now, if you need to make sure the letter is received, you would ask the receiver to send you a confirmation. If you don't get confirmation, you send the letter again. This is exactly what the transport layer does.

🎯 **Exam Important Points:**
- Network layer provides **best effort delivery** — no guarantee.
- Buffer overflow at intermediate routers can cause **data loss**.
- The transport layer detects lost data and **retransmits** it.
- The basic idea: sense the channel → detect if data was lost → retransmit if needed.

⚠️ **Common Confusions:**
- The network layer does NOT intentionally drop data. It drops data only when buffers are full.
- The transport layer does NOT change how the network layer works. It adds its own mechanism on top.

---

## Concept 3: Ensuring Reliability at the Transport Layer

📌 **Concept Name:** Reliable Data Transfer Protocol (rdt) and Unreliable Data Transfer (udt)

🧠 **Simple Explanation:**

The application layer always wants **reliable delivery**. If you are downloading a file, you need every chunk of data — if even one chunk is missing, the file is corrupted.

But the network layer gives you an **unreliable channel**. So how do you build reliability on top of an unreliable channel? That's the job of the transport layer.

Here is how it works using hypothetical functions:

- At the **interface between network layer and transport layer**, there is an unreliable send function called **`udt_send()`**. This is unreliable — data might get lost.
- At the **interface between transport layer and application layer**, there is a reliable send function. The transport layer converts the unreliable delivery into reliable delivery.

On the **sender side**: The transport layer takes data from the application, adds a sequence number and checksum, and sends it via the unreliable channel using `udt_send()`.

On the **receiver side**: The transport layer receives data, checks if it arrived correctly, and then delivers it up to the application layer.

For applications where reliability is NOT important (like live video streaming), we use **UDP** which does NOT provide reliability. But for applications where reliability IS important (like file transfer), we use **TCP** which provides reliability.

🎯 **Exam Important Points:**
- `udt_send()` = unreliable data transfer at the network layer.
- `rdt_send()` = reliable data transfer provided by the transport layer.
- Transport layer implements reliability mechanisms at both sender and receiver.
- UDP = no reliability. TCP = with reliability.
- The transport layer adds sequence numbers and checksums to ensure reliability.

⚠️ **Common Confusions:**
- `udt_send()` and `rdt_send()` are hypothetical function names used to explain the concept — they are not actual system calls.
- Reliability is added at the transport layer, NOT by changing the network layer.

---

## Concept 4: Why Flow Control and Error Control Exist at BOTH Data Link Layer and Transport Layer

📌 **Concept Name:** Flow Control and Error Control at Two Layers

🧠 **Simple Explanation:**

You might wonder — if the data link layer already has flow control and error control, why do we need them again at the transport layer?

The answer is:

- **At the data link layer:** Flow control and error control work **hop-by-hop**. That means they operate between two directly connected devices (e.g., between your computer and the first router, or between two neighboring routers).

- **At the transport layer:** Flow control and error control work **end-to-end**. That means they operate between the original sender and the final receiver.

The key distinction from the transcript:

> **Flow control and error control at the transport layer are ESSENTIAL.**  
> **Flow control and error control at the data link layer IMPROVE PERFORMANCE.**

So even if the data link layer has flow control, we still NEED it at the transport layer. And even if the transport layer has flow control, the data link layer's flow control helps improve overall network performance.

🎯 **Exam Important Points:**
- Data link layer = **hop-by-hop** flow control and error control.
- Transport layer = **end-to-end** flow control and error control.
- Transport layer flow control is **essential** (mandatory).
- Data link layer flow control **improves performance** (helpful but not sufficient alone).
- Both layers need their own flow control — one cannot fully replace the other.

⚠️ **Common Confusions:**
- Don't think "if data link layer already does it, transport layer doesn't need it." Both are needed for different reasons.
- Hop-by-hop means between each pair of directly connected devices. End-to-end means between the original source and the final destination.

---

## Concept 5: Hop-by-Hop vs End-to-End Flow Control — With Example

📌 **Concept Name:** Why Hop-by-Hop Flow Control Alone is Not Sufficient

🧠 **Simple Explanation:**

Let's understand this with the example from the transcript.

**Scenario:** Source S sends data to Destination D through routers R1, R2, R3.

| Link | Speed |
|------|-------|
| S → R1 | 10 Mbps |
| R1 → R2 | 5 Mbps |
| R2 → R3 | 3 Mbps |
| R3 → D | 1 Mbps |

**What is the effective end-to-end rate?** → **1 Mbps** (the bottleneck is the slowest link).

**What happens with ONLY hop-by-hop flow control (no end-to-end)?**

- S sees the link to R1 is 10 Mbps, so it sends data at 10 Mbps.
- R1 can only forward to R2 at 5 Mbps. So R1's buffer starts filling up.
- Similarly, R2 can only forward at 3 Mbps, and R3 can only forward at 1 Mbps.
- Eventually, buffers overflow and data is **lost**.
- Because S does not know the receiver D can only accept at 1 Mbps, a huge amount of data (9 Mbps worth) gets accumulated in router buffers and eventually dropped.

**Conclusion:** Hop-by-hop flow control alone is NOT sufficient. We need end-to-end (transport layer) flow control so S knows the overall capacity and adjusts its rate.

**What happens with ONLY end-to-end flow control (no hop-by-hop)?**

Now imagine a router has **multiple incoming links** from different sources. Even if one source sends at only 1 Mbps, other sources might send at 2 Mbps and 5 Mbps through the same router. If the outgoing link from that router is only 3 Mbps but total incoming is 8 Mbps, the buffer will overflow. So hop-by-hop flow control at the data link layer is also needed to reduce congestion at each router.

**Final Conclusion:**
- End-to-end flow control at transport layer = **essential**.
- Hop-by-hop flow control at data link layer = **improves performance**, but cannot completely eliminate data loss alone.

🎯 **Exam Important Points:**
- The effective rate of an end-to-end path = the rate of the **slowest (bottleneck) link**.
- Without end-to-end flow control, the sender might send too fast → causes buffer overflow → data loss.
- Without hop-by-hop flow control, intermediate routers can get congested when multiple flows converge.
- **Both** are needed: transport layer (essential) + data link layer (performance improvement).

⚠️ **Common Confusions:**
- The effective rate is NOT the sum of all link speeds. It is the **minimum** link speed on the path.
- Even with hop-by-hop flow control, you cannot eliminate all data loss — the system needs time to converge and estimate ideal rates.

---

## Concept 6: End-to-End Arguments in System Design

📌 **Concept Name:** End-to-End Arguments (Reference Paper)

🧠 **Simple Explanation:**

The transcript mentions a very important and fundamental paper in computer networking:

**"End-to-End Arguments in System Design"** by J.H. Saltzer, D.P. Reed, and D.D. Clark from MIT.

This paper explains **why we need end-to-end protocols in the internet** even when hop-by-hop protocols already exist. The professor suggests reading this paper for a deeper understanding of the principles behind the TCP/IP protocol stack.

🎯 **Exam Important Points:**
- This is a landmark paper in networking.
- It justifies the need for end-to-end mechanisms at the transport layer.
- Authors: Saltzer, Reed, and Clark (MIT).

---

## Concept 7: Stop and Wait Flow Control — Error Free Channel

📌 **Concept Name:** Stop and Wait Protocol (Error Free Channel)

🧠 **Simple Explanation:**

This is the **simplest flow control algorithm**.

**How it works:**

1. The sender sends **one frame (packet)** to the receiver.
2. The sender then **stops** and **waits** for an acknowledgement (ACK) from the receiver.
3. Once the receiver gets the frame, it sends back an ACK.
4. Only after the sender receives the ACK, it sends the **next frame**.
5. This process repeats: send one frame → wait for ACK → send next frame → wait for ACK...

**In an error-free channel:**
- There is no data loss.
- The receiver will always receive the frame and will always send back the ACK.
- The sender will always receive the ACK.
- So, the protocol works perfectly — just slowly.

🛠 **Real-world Example:**

Imagine you are passing boxes to your friend across a river using a small boat. You put one box on the boat, send it. You wait until your friend signals "I got it!" Then you send the next box. You never send the second box until the first one is confirmed received.

🎯 **Exam Important Points:**
- Stop and Wait = send one frame, wait for ACK, then send next frame.
- In error-free channel, this always works correctly.
- Only **one frame** can be outstanding (in transit) at a time.
- Very simple but very slow (wastes network capacity).

⚠️ **Common Confusions:**
- "Stop and Wait" doesn't mean the receiver stops. The **sender** stops and waits after sending one frame.

---

## Concept 8: Stop and Wait Flow Control — Noisy Channel (with Sequence Numbers)

📌 **Concept Name:** Stop and Wait in a Noisy Channel + Sequence Numbers

🧠 **Simple Explanation:**

In a real network, the channel is **noisy** — frames can get **lost**. So, we need to handle this.

**Key additions for noisy channel:**

**1. Sequence Numbers:**
Every frame is tagged with a **sequence number** to uniquely identify it. In Stop and Wait, we only need **two sequence numbers: 0 and 1** (alternating). This is because only one frame is outstanding at a time.

- Frame 0 is sent → receiver sends ACK 1 (meaning "I got frame 0, now send frame 1").
- Frame 1 is sent → receiver sends ACK 0 (meaning "I got frame 1, now send frame 0").

**2. Timeout:**
If a frame is lost, the receiver never gets it, so it never sends an ACK. The sender waits for a **timeout period**. If no ACK arrives before the timeout, the sender **retransmits** the same frame.

**3. Handling Lost ACK:**
If the ACK is lost (not the frame), the sender times out and resends the same frame. The receiver gets a **duplicate frame**. The receiver uses the sequence number to detect the duplicate and **discards** it, then sends the ACK again.

**Why only 2-bit (0 and 1) sequence numbers?**
Because in Stop and Wait, only one frame is in transit at a time. So you only need to distinguish between "current frame" and "next frame". Sequence numbers just alternate: 0, 1, 0, 1, 0, 1...

🛠 **Real-world Example:**

You send a letter numbered "1" to your friend. Your friend sends back "Got letter 1, send letter 2." If you don't get the reply within a certain time, you assume the letter got lost and send letter "1" again. If your friend already got letter 1 and gets it again, they just ignore the duplicate and resend the confirmation.

🎯 **Exam Important Points:**
- Noisy channel → frames or ACKs can be lost.
- **Sequence numbers** are used to identify each frame uniquely.
- In Stop and Wait, only **2 sequence numbers (0 and 1)** are needed — this is a 1-bit sequence number.
- **Timeout mechanism** handles lost frames/ACKs — if ACK not received before timeout, retransmit.
- Receiver discards duplicate frames using sequence numbers.
- ACK number indicates the **next expected frame** (ACK 1 means "I got frame 0, send frame 1").

⚠️ **Common Confusions:**
- A "2-bit sequence number" in the transcript means we use values 0 and 1 — this is actually a **1-bit** field (2 possible values). The professor calls it "2 bit" to refer to the two values.
- The ACK number is NOT the number of the frame received. It is the number of the **next expected frame**.

---

## Concept 9: Automatic Repeat Request (ARQ)

📌 **Concept Name:** Automatic Repeat Request (ARQ)

🧠 **Simple Explanation:**

When you combine:
- Flow control (stop and wait)
- Sequence numbers
- Timeout and retransmission
- Working over a noisy channel

...you get what is called **Automatic Repeat Request (ARQ)**.

ARQ is the general name for this class of algorithms where the sender automatically retransmits frames when it detects (via timeout or negative acknowledgement) that a frame was lost or corrupted.

The Stop and Wait protocol in a noisy channel is actually called **Stop and Wait ARQ**.

🎯 **Exam Important Points:**
- ARQ = flow control + reliability mechanism combined.
- Stop and Wait with sequence numbers + timeout = **Stop and Wait ARQ**.
- ARQ ensures reliability by retransmitting lost or corrupted frames.
- There are different versions/variants of ARQ algorithms (discussed in later lectures).

⚠️ **Common Confusions:**
- ARQ is not a single protocol — it is a **class** of protocols. Stop and Wait ARQ is one type. Others include Go-Back-N ARQ and Selective Repeat ARQ (covered later).

---

## Concept 10: Stop and Wait ARQ — Sender Side State Transition Diagram

📌 **Concept Name:** Sender Side Implementation of Stop and Wait ARQ

🧠 **Simple Explanation:**

The transcript describes the sender's behavior using a **state transition diagram** with 4 states:

**State 1: "Wait for call 0 from above"**
- The sender is idle, waiting for the application layer to give it data.
- When data arrives, the sender creates a packet with sequence number 0, adds a checksum, sends it via `udt_send()`, and starts a timer.
- Moves to State 2.

**State 2: "Wait for ACK 0"**
- The sender is waiting for acknowledgement for frame 0.
- **If ACK received and it is not corrupted and it is ACK for frame 0:** Stop the timer. Move to State 3 (wait for call 1 from above).
- **If ACK is corrupted or wrong ACK received:** Stay in the same state, keep waiting.
- **If timeout occurs:** Retransmit the packet, restart the timer, stay in the same state.

**State 3: "Wait for call 1 from above"**
- Now the sender waits for the next data from the application layer.
- When data arrives, the sender creates a packet with sequence number 1, adds checksum, sends via `udt_send()`, starts timer.
- Moves to State 4.

**State 4: "Wait for ACK 1"**
- Same as State 2 but for frame 1.
- **If correct ACK 1 received:** Stop timer. Move back to State 1.
- **If corrupted/wrong ACK:** Stay here.
- **If timeout:** Retransmit, restart timer.

The cycle repeats: State 1 → State 2 → State 3 → State 4 → State 1 → ...

🎯 **Exam Important Points:**
- 4 states in the sender: Wait for call 0, Wait for ACK 0, Wait for call 1, Wait for ACK 1.
- On sending a packet: append sequence number + checksum → `udt_send()` → start timer.
- On correct ACK: stop timer → move to next state.
- On timeout: retransmit same packet → restart timer → stay in same state.
- On corrupted ACK: do nothing, stay in same state (keep waiting).

⚠️ **Common Confusions:**
- "Call from above" means the application layer gives data to the transport layer. It does NOT mean a phone call.
- The checksum is used later for error detection — ensuring the data is not corrupted.

---

## Concept 11: Problems with Stop and Wait

📌 **Concept Name:** Limitations of Stop and Wait Protocol

🧠 **Simple Explanation:**

Stop and Wait has a major problem: **it wastes network resources**.

**Problem 1: Must wait for ACK before sending next packet.**
In Stop and Wait, you send one packet, then sit idle waiting for ACK. During this waiting time, the network link is not being used at all. This is very inefficient, especially for high-speed or long-distance networks.

**Problem 2: Bidirectional connections need two instances.**
If both A and B want to send data to each other simultaneously (bidirectional), you need **two separate instances** of Stop and Wait — one for A→B and one for B→A. This wastes even more resources.

🎯 **Exam Important Points:**
- Stop and Wait is very **inefficient** — sender is idle while waiting for ACK.
- For bidirectional connections, two separate instances of Stop and Wait are needed → even more waste.
- Only ONE packet can be in transit at a time — very low throughput.

---

## Concept 12: Piggybacking

📌 **Concept Name:** Piggybacking

🧠 **Simple Explanation:**

Piggybacking is a **partial solution** to the bidirectional problem.

**Idea:** When B needs to send a data frame back to A, instead of sending the data and the ACK separately, B **attaches (piggybacks) the ACK along with the data frame**.

So, in one packet from B to A, you have both:
- The data B wants to send to A
- The acknowledgement for the frame A previously sent to B

This is more efficient than having two completely separate Stop and Wait instances. But even with piggybacking, the fundamental problem remains — **you still have to wait for acknowledgement before sending the next packet**.

🛠 **Real-world Example:**

Imagine you and your friend are exchanging letters. Instead of sending a separate "I got your letter" confirmation and a separate new letter, your friend writes both in the same envelope: "Got your letter #1. By the way, here is my letter #5." This saves one envelope (one transmission).

🎯 **Exam Important Points:**
- Piggybacking = sending data + ACK together in the same frame.
- It reduces overhead by combining data and acknowledgement.
- It helps in bidirectional communication.
- But it does NOT solve the fundamental problem of Stop and Wait (still must wait for ACK before sending next packet).

⚠️ **Common Confusions:**
- Piggybacking does NOT mean sending multiple data packets at once. It only means attaching an ACK to a data packet going in the reverse direction.

---

## Concept 13: Introduction to Sliding Window Protocol (Pipelined Protocol)

📌 **Concept Name:** Sliding Window Protocol / Pipelined Protocol

🧠 **Simple Explanation:**

To solve the inefficiency of Stop and Wait, we use a class of protocols called **Sliding Window Protocols** (also called **Pipelined Protocols**).

**Key difference from Stop and Wait:**

- In Stop and Wait: You send **one** packet, wait for ACK, then send the next.
- In Sliding Window: You can send **multiple** packets without waiting for the ACK of each one. While packets are going to the receiver, acknowledgements are coming back in parallel.

**How it works (broad idea):**
- The sender has a "window" of packets it is allowed to send without waiting for ACK.
- It sends all packets in the window one after another (like a pipeline).
- As ACKs come back, the window "slides" forward, allowing new packets to be sent.
- This way, the network is being used much more efficiently — no idle time.

The professor mentions that the details of sliding window protocols will be covered in the **next lecture**.

🛠 **Real-world Example:**

Instead of sending one box on a boat and waiting for confirmation before sending the next, you now have a conveyor belt. You put box 1, box 2, box 3 all on the belt continuously. While box 3 is still on its way, you get confirmation for box 1 and you put box 4 on the belt. The belt never stops.

🎯 **Exam Important Points:**
- Sliding Window = Pipelined Protocol.
- Multiple frames can be sent **without waiting for individual ACKs**.
- Much more efficient than Stop and Wait.
- The sender sends a **sequence of packets** and receives a **sequence of acknowledgements** in parallel.
- The network resources are utilized much better.
- Details will be covered in the next lecture.

⚠️ **Common Confusions:**
- Sliding Window does NOT mean infinite packets can be sent. There is a **window size** limit — the sender can only have a certain number of unacknowledged packets at a time.
- This was only introduced in this lecture. The detailed working (Go-Back-N, Selective Repeat) comes later.

---

## Summary Table of Lecture 14

| Concept | Key Point |
|---------|-----------|
| Flow Control | Sender should not send faster than receiver can handle |
| Reliability | All data must eventually reach the receiver |
| Network Layer | Unreliable, best-effort delivery, can lose packets |
| Transport Layer vs Data Link Layer | Transport = end-to-end (essential), Data Link = hop-by-hop (improves performance) |
| Effective Rate | Minimum link speed on the path (bottleneck link) |
| Stop and Wait (Error Free) | Send one frame → wait for ACK → send next |
| Stop and Wait (Noisy Channel) | Add sequence numbers (0 and 1), timeout, retransmission |
| ARQ | Automatic Repeat Request — class of reliability protocols |
| Stop and Wait ARQ States | 4 states: Wait call 0 → Wait ACK 0 → Wait call 1 → Wait ACK 1 |
| Piggybacking | Attach ACK along with data in reverse direction |
| Sliding Window | Send multiple frames without waiting for each ACK — pipelined |

---

## 10 MCQs — Strictly from Lecture 14

### Q1. What is the primary purpose of flow control at the transport layer?

A) To increase the speed of the network  
B) To ensure the sender does not send more data than the receiver can handle  
C) To encrypt the data before sending  
D) To find the shortest path to the destination  

**Answer: B**  
**Explanation:** Flow control ensures the sender adjusts its rate so it does not overshoot the rate at which the receiver can receive. This is directly stated in the transcript.

---

### Q2. The network layer provides which type of delivery?

A) Guaranteed delivery  
B) Reliable delivery  
C) Best-effort delivery  
D) Encrypted delivery  

**Answer: C**  
**Explanation:** The transcript states that the network layer makes a "best try" to deliver data based on the destination address. It does not guarantee delivery — packets can be lost due to buffer overflow at routers.

---

### Q3. Flow control and error control at the transport layer are:

A) Optional and can be skipped  
B) Only needed for UDP  
C) Essential  
D) Only needed at the data link layer  

**Answer: C**  
**Explanation:** The transcript clearly states: "Flow control and error control at the transport layer are essential." At the data link layer, they improve performance.

---

### Q4. In the example from the lecture, the path S → R1 → R2 → R3 → D has link speeds of 10, 5, 3, and 1 Mbps respectively. What is the effective end-to-end rate?

A) 10 Mbps  
B) 19 Mbps  
C) 5 Mbps  
D) 1 Mbps  

**Answer: D**  
**Explanation:** The effective end-to-end rate equals the bottleneck (slowest) link speed, which is R3→D = 1 Mbps. The transcript explicitly states this.

---

### Q5. In Stop and Wait protocol, how many frames can be outstanding (in transit) at one time?

A) As many as the window size allows  
B) 2  
C) 1  
D) It depends on the bandwidth  

**Answer: C**  
**Explanation:** In Stop and Wait, the sender sends one frame and waits for its ACK before sending the next. So only one frame can be outstanding at any time.

---

### Q6. In Stop and Wait for a noisy channel, what is the maximum number of distinct sequence numbers needed?

A) 1  
B) 2 (0 and 1)  
C) 4  
D) 8  

**Answer: B**  
**Explanation:** The transcript states that since only one frame is outstanding at a time, a 2-value sequence number (0 and 1) is sufficient. The sequence numbers just alternate: 0, 1, 0, 1...

---

### Q7. In Stop and Wait ARQ, what happens when the sender's timer expires (timeout) before receiving an ACK?

A) The sender sends the next frame  
B) The sender closes the connection  
C) The sender retransmits the same frame and restarts the timer  
D) The sender sends an error message to the receiver  

**Answer: C**  
**Explanation:** The transcript describes that on timeout, the sender retransmits the packet through the `udt_send()` mechanism and restarts the timer.

---

### Q8. What is piggybacking?

A) Sending multiple data frames at once  
B) Attaching the acknowledgement along with a data frame being sent in the reverse direction  
C) Sending data without any acknowledgement  
D) Splitting a large frame into smaller frames  

**Answer: B**  
**Explanation:** Piggybacking means when B sends data to A, it also attaches the ACK for the frame it previously received from A in the same packet.

---

### Q9. What is the main problem with the Stop and Wait protocol?

A) It is too complex to implement  
B) It does not use sequence numbers  
C) It wastes network resources because the sender stays idle waiting for ACK  
D) It cannot work on noisy channels  

**Answer: C**  
**Explanation:** The transcript explains that Stop and Wait wastes resources because for every packet, the sender must wait for the ACK. The sender cannot send the next packet during this idle time, leading to poor network utilization.

---

### Q10. Sliding Window protocols solve the problem of Stop and Wait by:

A) Removing the need for acknowledgements  
B) Allowing multiple frames to be sent without waiting for individual ACKs  
C) Using only one sequence number  
D) Making the channel error-free  

**Answer: B**  
**Explanation:** The transcript states that sliding window protocols allow sending multiple frames or packets altogether without waiting for the corresponding acknowledgement. This pipelined approach utilizes the network much more efficiently.

---

*End of Lecture 14 — Complete Notes & MCQs*
