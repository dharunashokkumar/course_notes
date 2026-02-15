# Lecture 18: Transport Layer Primitives — Complete Explanation

**Course:** Computer Networks and Internet Protocol  
**Instructor:** Prof. Sandip Chakraborty, IIT Kharagpur  
**Lecture Title:** Transport Layer Primitives

---

## What This Lecture Is About (Big Picture)

In earlier lectures, you learned individual transport layer services like flow control, reliability, congestion control, and connection establishment. In **this lecture**, the professor combines ALL those services together and shows how they work as a **complete end-to-end transport layer protocol**. He also introduces the **primitives** (function calls) that an application uses to talk to the transport layer, and defines important terminology like segment, packet, and frame.

Think of it like this: earlier you learned individual parts of a car (engine, brakes, steering). Now in this lecture, you are putting the full car together and learning how to drive it.

---

## Concept 1: Multiple Applications on a Single Machine

### 📌 Concept Name
**Multiplexing — Multiple Applications Communicating Over the Network**

### 🧠 Simple Explanation

Imagine you have a desktop computer (D1). On this computer, you might be running a web browser, a chat application, and an email client — all at the same time. Now, another computer (D3) is also running multiple applications.

The problem is: **How does the network know which application on D1 wants to talk to which application on D3?**

Just knowing the machine address (IP address) is NOT enough. You also need to know **which application** on that machine you want to reach.

### 🛠 Real-world Example (from transcript)

The professor gives this example: Desktop D1 has applications A1 and A2. Desktop D3 has applications A1, A2, A3. If A1 on D1 wants to communicate with A2 on D3, the network must uniquely identify both the machine AND the specific application.

### 🎯 Exam Important Points
- A single machine can run **multiple applications** at the same time (multi-tasking environment).
- Just identifying the machine (IP address) is **not sufficient** — you must also identify the application.
- Two types of addresses are needed: one for the machine, one for the application.

### ⚠️ Common Confusions
- Students sometimes think IP address alone is enough to establish communication. It is NOT — you also need the port number.

---

## Concept 2: IP Address and Port Number

### 📌 Concept Name
**IP Address (Network Layer) and Port Number (Transport Layer)**

### 🧠 Simple Explanation

To solve the above problem, we use **two addresses**:

1. **IP Address** — This is bound to the **network layer**. It uniquely identifies a **machine** on the network.
2. **Port Number** — This is bound to the **transport layer**. It uniquely identifies an **application** running on that machine.

So, to reach a specific application on a specific machine, you need both the IP address AND the port number.

In the transport layer header, you put:
- **Source Port Number** → which application is sending
- **Destination Port Number** → which application should receive

### 🎯 Exam Important Points
- IP address = identifies the **machine** (network layer)
- Port number = identifies the **application** (transport layer)
- Transport layer header contains **source port** and **destination port**
- Port number maps a transport layer entity to a particular application

### ⚠️ Common Confusions
- IP address is NOT a transport layer concept — it belongs to the network layer.
- Port number is NOT a network layer concept — it belongs to the transport layer.

---

## Concept 3: The Logical Pipe

### 📌 Concept Name
**Logical Pipe Between Two Transport Layer Entities**

### 🧠 Simple Explanation

When two applications communicate, the transport layer creates a **logical pipe** between them. This pipe is not a physical wire — it is a virtual/logical connection.

All the transport layer services (flow control, reliability, congestion control) are implemented **on top of this logical pipe**.

Think of a telephone call: when you call someone, you establish a logical connection. Your words travel as signals over physical wires, but from your perspective, you are just talking through a "pipe" to the other person.

### 🛠 Real-world Example (from transcript)

The professor compares it to a phone call: You say "Hello, how are you?" and wait. If you get a reply "I am fine," you know the other end received your message. If no reply comes for 2 minutes, you say "Hello, are you hearing?" — this is like the retransmission mechanism.

### 🎯 Exam Important Points
- The transport layer provides **end-to-end** connectivity through a logical pipe.
- The layers below transport (network layer and below) are **unreliable** — packets may get dropped.
- The transport layer detects dropped packets using **sequence numbers** and **retransmits** them.

---

## Concept 4: Uniquely Identifying the Logical Pipe — The 6 Tuples

### 📌 Concept Name
**6-Tuple Identification of a Connection (Socket)**

### 🧠 Simple Explanation

Each logical pipe (connection) between two applications is uniquely identified by **6 things** (called 6 tuples):

1. **Source IP** — address of the sending machine
2. **Source Port** — application number on the sending machine
3. **Source Initial Sequence Number** — the starting sequence number chosen by the sender
4. **Destination IP** — address of the receiving machine
5. **Destination Port** — application number on the receiving machine
6. **Destination Initial Sequence Number** — the starting sequence number chosen by the receiver

**Why do we need the initial sequence numbers?**

Because if a system crashes and restarts, it might reuse the same source IP, source port, destination IP, and destination port. But the old connection might still have some delayed packets floating in the network (delayed duplicates). The initial sequence number ensures the new connection does NOT use any sequence number from the **forbidden region** of the previous connection. This prevents old delayed packets from being confused with new ones.

### 🎯 Exam Important Points
- A connection (socket) is identified by **6 tuples**: Source IP, Source Port, Source Initial Sequence Number, Destination IP, Destination Port, Destination Initial Sequence Number.
- The initial sequence number must **not fall within the forbidden region** of the previous connection using the same IP-port pair.
- In Unix terminology, this logical pipe is called a **socket**.

### ⚠️ Common Confusions
- Many students think only 4 tuples (source IP, source port, destination IP, destination port) are enough. But to avoid **delayed duplicate problem**, the initial sequence numbers are also needed — making it 6 tuples.

### 📝 Possible NPTEL-style Question
"How many tuples are used to uniquely identify a transport layer connection?" → **Answer: 6 tuples**

---

## Concept 5: Hypothetical Transport Layer Primitives

### 📌 Concept Name
**Transport Layer Primitives — LISTEN, CONNECT, SEND, RECEIVE, DISCONNECT**

### 🧠 Simple Explanation

The transport layer is implemented inside the **operating system**. Your application (like a chat app) runs in the **user space**. To send data over the network, your application must interact with the transport layer through special function calls called **primitives**.

The professor designs a **hypothetical protocol** using a client-server model to explain these primitives:

**Primitive 1: LISTEN**
- Used by the **server**.
- The server puts itself in a "ready to accept connection" state.
- If the server is NOT in the listen state, no client can connect to it.
- Think of it like a shopkeeper opening their shop — until the shop is open, no customer can come in.

**Primitive 2: CONNECT**
- Used by the **client**.
- The client asks the transport layer to initiate a connection to the server.
- If 3-way handshaking is the method, the transport layer will execute the 3-way handshake.

**Primitive 3: SEND**
- Used by **either client or server** after the connection is established.
- It sends data through the logical pipe to the other end.

**Primitive 4: RECEIVE**
- Used by **either client or server**.
- It reads/accepts data from the transport layer buffer.
- The data arrives at the transport layer and waits in the **receiver buffer**. The application must call RECEIVE to get that data.

**Primitive 5: DISCONNECT**
- Used to **close** the connection when data transmission is complete.

### 🎯 Exam Important Points
- Server must be in **LISTEN** state before any client can connect.
- Client initiates the connection using **CONNECT**.
- **SEND** and **RECEIVE** can only be called after the connection is **established**.
- DISCONNECT closes the connection.
- These primitives allow user-space applications to interact with the OS-level transport layer.

### ⚠️ Common Confusions
- SEND and RECEIVE are NOT valid before the connection is established. You MUST be in the connected/established state first.

---

## Concept 6: Stateful Protocol — Remembering the State of the Pipe

### 📌 Concept Name
**Stateful Protocol at the Transport Layer**

### 🧠 Simple Explanation

A **stateful protocol** means the transport layer **remembers the current state** of the connection (pipe) at all times.

Why is this important? Because you cannot just make a SEND call randomly. The system must first check: "Am I in the connected state?" If yes, sending is allowed. If not, the call is invalid.

The pseudo code looks like this:
- **Sender side:** `if connected → send; else → wait`
- **Receiver side:** `if connected → receive; else → wait for connection`

So, every time you want to send or receive, the transport layer checks the state of the pipe. This is what makes it a **stateful** protocol — with every connection, the system remembers what state it is in.

### 🎯 Exam Important Points
- Transport layer is a **stateful protocol** — it remembers the state of the pipe.
- You cannot make a SEND call before the connection is established — it is an **invalid call**.
- State must be checked before every send/receive operation.

### 📝 Possible NPTEL-style Question
"Why is the transport layer called a stateful protocol?" → Because it remembers the state of the connection and allows actions (send/receive) only in appropriate states.

---

## Concept 7: State Transition Diagram

### 📌 Concept Name
**Transport Layer Protocol — State Transition Diagram**

### 🧠 Simple Explanation

The state transition diagram shows how the client and server move from one state to another during a connection. This is a very important concept for exams.

Here is the flow step by step:

**Step 1: IDLE State**
Both client and server start in the **IDLE** state (doing nothing).

**Step 2: Connection Establishment**
- The **client** executes the CONNECT primitive → moves to **Active Establishment Pending** state.
- The **server** receives the connection request segment → moves to **Passive Establishment Pending** state.
- The server executes the connection handshake (e.g., 3-way handshake), sends acknowledgement → server moves to **ESTABLISHED** state.
- The client receives the "connection accepted" segment → client moves to **ESTABLISHED** state.

**Step 3: Data Transfer**
- In the **ESTABLISHED** state, both client and server can call SEND and RECEIVE.
- This is the ONLY state where data transfer is allowed.

**Step 4: Disconnection**
- The **client** executes the DISCONNECT primitive → moves to **Active Disconnection Pending** state.
- The **server** receives the disconnection request segment → moves to **Passive Disconnection Pending** state.
- The server executes the disconnect primitive, sends acknowledgement → server moves back to **IDLE** state.
- The client receives the disconnection acknowledgement → client moves back to **IDLE** state.

**Key visual note:** In the diagram, **solid lines** represent the client side and **dotted lines** represent the server side.

### 🎯 Exam Important Points
- States: IDLE → Active/Passive Establishment Pending → ESTABLISHED → Active/Passive Disconnection Pending → IDLE
- Data transfer (SEND/RECEIVE) is allowed ONLY in the **ESTABLISHED** state.
- State transitions are triggered by **sending or receiving messages**.
- "Active" means the entity **initiated** the action (client usually).
- "Passive" means the entity **received** the request (server usually).

### ⚠️ Common Confusions
- Active Establishment = the one who **initiates** the connection (usually client).
- Passive Establishment = the one who **receives** the connection request (usually server).
- Don't confuse "Active" with "Server" — it is usually the CLIENT that does active establishment.

---

## Concept 8: Segment, Packet, and Frame — Terminology

### 📌 Concept Name
**Segment (Transport Layer), Packet (Network Layer), Frame (Data Link Layer)**

### 🧠 Simple Explanation

Each layer of the protocol stack has its own name for the data unit it handles:

**At the Transport Layer → SEGMENT**
- The data that the transport layer works with is called a **segment**.
- The transport layer adds a **segment header** to the data and passes it down to the network layer.

**At the Network Layer → PACKET (or Datagram)**
- Whatever the network layer receives from the transport layer becomes its **payload**.
- The network layer adds a **packet header** and passes it to the data link layer.
- In the context of UDP, it is also called a **datagram**.

**At the Data Link Layer → FRAME**
- Whatever the data link layer receives from the network layer becomes its **payload**.
- The data link layer adds a **frame header** and sends it to the physical layer for transmission.

So the encapsulation works like this:
```
Application Data
    ↓ (add segment header)
[Segment Header | Segment Payload] = SEGMENT (Transport Layer)
    ↓ (add packet header)
[Packet Header | Packet Payload] = PACKET (Network Layer)
    ↓ (add frame header)
[Frame Header | Frame Payload] = FRAME (Data Link Layer)
    ↓
Physical Transmission
```

### 🎯 Exam Important Points
- **Transport Layer** → Segment
- **Network Layer** → Packet (or Datagram for UDP)
- **Data Link Layer** → Frame
- Each layer adds its own header to the data from the upper layer.
- The data from the upper layer becomes the **payload** of the current layer.
- Flow control operates on both segments (transport layer) and frames (data link layer).

### ⚠️ Common Confusions
- Do NOT use "packet" when talking about transport layer — the correct term is "segment."
- Do NOT use "frame" when talking about network layer — the correct term is "packet."
- "Datagram" is specifically used in the context of **UDP** at the network layer.

### 📝 Possible NPTEL-style Question
"What is the data unit at the transport layer called?" → **Segment**
"What is the data unit at the data link layer called?" → **Frame**

---

## Concept 9: Complete Transport Layer Process Flow

### 📌 Concept Name
**Combining All Service Primitives — End-to-End Transport Layer Flow**

### 🧠 Simple Explanation

Now the professor combines everything learned in previous lectures into one complete flow. Here is the full process:

**Phase 1: Connection Establishment**
- A connection is initiated by selecting an **initial sequence number**.
- This initial sequence number must NOT fall in the **forbidden region** of any previous connection that used the same source IP, source port, destination IP, destination port pair.
- The connection (socket) is uniquely identified by the **6 tuples**.

**Phase 2: Flow Control and Reliability**
- Once the initial sequence numbers are set, they are used for **flow control** and **reliability** through **ARQ (Automatic Repeat Request)** protocols.
- The sender will NOT send data at a rate higher than what the receiver can handle.
- Sequence numbers uniquely identify each **byte** (for byte sequence number protocols) or each **packet** (for packet sequence number protocols).
- Lost packets are handled through **retransmission**.

**Phase 3: Congestion Control**
- The congestion control algorithm reduces the transmission rate when congestion is detected.
- The formula for sender rate is:

  **Sender Rate = minimum(Network Rate, Receiver Rate)**

- The **Receiver Rate** is advertised by the receiver with every individual acknowledgement.
- The **Network Rate** is managed using the **AIMD (Additive Increase Multiplicative Decrease)** protocol, which ensures both efficiency and fairness simultaneously.

**Phase 4: Connection Closure**
- When data transmission is complete, the connection is closed.
- **Asynchronous closure** (both sides close independently) is ideal but NOT possible in a distributed system with unreliable channels.
- Therefore, we use **synchronous closure with timeout**.

### 🎯 Exam Important Points
- Connection establishment → selects initial sequence number (avoiding forbidden region)
- Flow control + reliability → uses ARQ, sequence numbers, retransmission
- Sender Rate = min(Network Rate, Receiver Rate)
- Congestion control → uses AIMD protocol
- Connection closure → synchronous closure with timeout (because asynchronous closure is not possible on unreliable channels)

---

## Concept 10: Congestion Control Behavior — The AIMD Graph

### 📌 Concept Name
**AIMD (Additive Increase Multiplicative Decrease) and Sender Rate Over Time**

### 🧠 Simple Explanation

The professor draws a graph showing how the sender rate changes over time when congestion control is active.

Here is how it works step by step:

1. **Start low:** The sender starts with a very low network rate (e.g., 1 kbps).
2. **Gradually increase:** The sender gradually increases its rate. During this time, since the network rate is low, the formula `Sender Rate = min(Network Rate, Receiver Rate)` makes the network rate the bottleneck.
3. **Saturation at Receiver Rate:** When the network rate exceeds the receiver advertised rate, the sender rate gets **saturated** at the receiver rate — it cannot go higher than what the receiver can handle.
4. **Packet loss detected:** At some point, a packet loss occurs (indicating congestion).
5. **AIMD kicks in:** When packet loss is detected, the sender **drops the rate** (multiplicative decrease) and starts the gradual increase again (additive increase).
6. **Repeat:** This process keeps repeating — rate goes up gradually, hits a limit or detects loss, drops, and increases again.

The key insight is that **flow control** (governed by receiver rate) and **congestion control** (governed by network rate) work **together** — they are **coupled**.

### 🎯 Exam Important Points
- AIMD = **Additive Increase, Multiplicative Decrease**
- Sender Rate = min(Network Rate, Receiver Rate)
- Start with a low rate, gradually increase.
- Sender rate saturates at receiver advertised rate.
- On packet loss → multiplicative decrease (drop rate), then additive increase again.
- Flow control and congestion control are **coupled together**.
- AIMD ensures both **efficiency** and **fairness**.

### ⚠️ Common Confusions
- "Receiver Rate" is what the receiver **advertises** in each ACK — it tells the sender how much it can handle.
- "Network Rate" is what the network can support — you discover this by gradually increasing and checking for packet loss.
- The sender rate is limited by **whichever is smaller** — receiver rate or network rate.

---

## Concept 11: Connection Closure — Synchronous vs Asynchronous

### 📌 Concept Name
**Synchronous Closure with Timeout**

### 🧠 Simple Explanation

When you are done sending data, you need to close the connection. There are two approaches:

1. **Asynchronous Closure** — Both sides close independently without coordinating. This is the ideal approach, but the professor says it is **NOT possible to implement** in a distributed system with an unreliable communication channel.

2. **Synchronous Closure with Timeout** — Both sides coordinate to close the connection, and a timeout mechanism is used. This is what is actually implemented.

### 🎯 Exam Important Points
- Asynchronous closure is good in theory but **not implementable** on unreliable channels.
- Synchronous closure with timeout is the **practical approach** used.
- This was discussed in earlier lectures as well (two army problem context).

---

## Concept 12: TCP Protocol — Introduction/Preview

### 📌 Concept Name
**TCP (Transmission Control Protocol) — Brief Introduction**

### 🧠 Simple Explanation

At the end of the lecture, the professor previews what comes next. He mentions that from the next lecture onwards, they will study the **TCP (Transmission Control Protocol)** in detail.

Key fact mentioned: Around **80% of the traffic** over the global internet uses TCP. This makes TCP the most widely used transport layer protocol.

### 🎯 Exam Important Points
- TCP is the most widely used transport layer protocol.
- About **80%** of global internet traffic uses TCP.
- TCP will be covered in detail in upcoming lectures.

---

## Summary Table — Lecture 18 at a Glance

| Topic | Key Point |
|---|---|
| Multiple applications | Single machine can run multiple apps; need port numbers to identify them |
| IP Address | Identifies the machine (network layer) |
| Port Number | Identifies the application (transport layer) |
| Logical Pipe | Virtual connection between two transport entities |
| 6 Tuples | Source IP, Source Port, Source Seq#, Dest IP, Dest Port, Dest Seq# |
| Socket | Unix term for the logical pipe |
| Primitives | LISTEN, CONNECT, SEND, RECEIVE, DISCONNECT |
| Stateful Protocol | Transport layer remembers connection state |
| State Transition | IDLE → Establishment → ESTABLISHED → Disconnection → IDLE |
| Segment | Data unit at Transport Layer |
| Packet/Datagram | Data unit at Network Layer |
| Frame | Data unit at Data Link Layer |
| Sender Rate Formula | min(Network Rate, Receiver Rate) |
| AIMD | Additive Increase Multiplicative Decrease for congestion control |
| Connection Closure | Synchronous closure with timeout (asynchronous not possible) |
| TCP | ~80% of internet traffic; covered in next lectures |

---

---

# 10 MCQs — Strictly from Lecture 18

---

**Q1. How many tuples are used to uniquely identify a transport layer connection (socket)?**

A) 2  
B) 4  
C) 5  
D) 6  

**✅ Answer: D) 6**

**Explanation:** As per the transcript, a connection is uniquely identified by 6 tuples: Source IP, Source Port, Source Initial Sequence Number, Destination IP, Destination Port, and Destination Initial Sequence Number. The initial sequence numbers are included to avoid the delayed duplicate problem.

---

**Q2. What is the data unit at the transport layer called?**

A) Frame  
B) Packet  
C) Segment  
D) Datagram  

**✅ Answer: C) Segment**

**Explanation:** The transcript clearly states that at the transport layer, the data primitive is called a segment. Packet is used at the network layer, and frame at the data link layer.

---

**Q3. Which primitive must the server execute before any client can connect to it?**

A) CONNECT  
B) SEND  
C) LISTEN  
D) RECEIVE  

**✅ Answer: C) LISTEN**

**Explanation:** The server must be in the LISTEN state (ready to accept connections). If the server is not listening, no client can initiate a connection.

---

**Q4. What is the formula for sender rate in the transport layer as discussed in this lecture?**

A) Sender Rate = Network Rate + Receiver Rate  
B) Sender Rate = max(Network Rate, Receiver Rate)  
C) Sender Rate = min(Network Rate, Receiver Rate)  
D) Sender Rate = Network Rate × Receiver Rate  

**✅ Answer: C) Sender Rate = min(Network Rate, Receiver Rate)**

**Explanation:** The transcript states that the sender rate is the minimum of the network supported rate and the receiver advertised rate. The sender cannot send faster than either of these limits.

---

**Q5. In the state transition diagram, when is the SEND/RECEIVE operation allowed?**

A) In the IDLE state  
B) In the Active Establishment Pending state  
C) In the ESTABLISHED state  
D) In the Passive Disconnection Pending state  

**✅ Answer: C) In the ESTABLISHED state**

**Explanation:** The transcript explicitly states that you can execute SEND and RECEIVE calls ONLY when you are in the ESTABLISHED state. In any other state, these calls are not valid.

---

**Q6. What does AIMD stand for?**

A) Automatic Increase Maximum Decrease  
B) Additive Increase Multiplicative Decrease  
C) Adaptive Increase Minimum Decrease  
D) Additive Increase Minimum Decrease  

**✅ Answer: B) Additive Increase Multiplicative Decrease**

**Explanation:** AIMD stands for Additive Increase Multiplicative Decrease. This protocol gradually increases the sending rate (additive increase) and sharply reduces it upon detecting packet loss (multiplicative decrease) to ensure both efficiency and fairness.

---

**Q7. Why is asynchronous closure not used in practice for connection termination?**

A) It is too slow  
B) It is not possible to implement in a distributed system with unreliable channels  
C) It requires too much bandwidth  
D) It does not support TCP  

**✅ Answer: B) It is not possible to implement in a distributed system with unreliable channels**

**Explanation:** The transcript states that although asynchronous closure is good in theory, it is not possible to implement in a distributed system with unreliable communication channels. Therefore, synchronous closure with timeout is used.

---

**Q8. What is the data unit called at the data link layer?**

A) Segment  
B) Packet  
C) Datagram  
D) Frame  

**✅ Answer: D) Frame**

**Explanation:** At the data link layer, the data unit is called a frame. The data link layer adds a frame header to the payload received from the network layer.

---

**Q9. Why are initial sequence numbers included in the 6-tuple identification of a connection?**

A) To increase the speed of data transfer  
B) To avoid delayed duplicate packets from a previous connection  
C) To reduce congestion  
D) To identify the application  

**✅ Answer: B) To avoid delayed duplicate packets from a previous connection**

**Explanation:** The transcript explains that if a system crashes and restarts, it might reuse the same IP-port pair. The initial sequence number must not fall in the forbidden region of the previous connection to avoid delayed duplicate packets being confused with new data.

---

**Q10. Approximately what percentage of global internet traffic uses TCP?**

A) 50%  
B) 60%  
C) 70%  
D) 80%  

**✅ Answer: D) 80%**

**Explanation:** The professor mentions at the end of the lecture that around 80% of traffic over the global internet uses TCP, making it the most widely used transport layer protocol.

---

*End of Lecture 18 — Complete Explanation and MCQs*
