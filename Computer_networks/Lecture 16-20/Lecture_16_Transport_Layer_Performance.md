# Lecture 16 — Transport Layer Performance

**Course:** Computer Networks and Internet Protocol  
**Professor:** Prof. Sandip Chakraborthy, IIT Kharagpur  
**Topic:** Transport Layer – VI (Performance)

---

## What This Lecture Is About (Overview)

In the previous lectures, you learned about:
- Connection establishment in the transport layer
- Flow control and reliability using ARQ protocols
- Three variants of ARQ: Stop and Wait, Go-Back-N, and Selective Repeat

In **this lecture**, we focus on the **performance** side of the transport layer. The main questions are:
- How do we measure and improve end-to-end performance of a transport layer protocol?
- How do we choose which protocol variant (Stop and Wait vs. Sliding Window) is best for a given network?
- How does the application layer talk to the transport layer in practice?
- How are transport layer buffers organized?

---

## Concept 1: Bandwidth Delay Product (BDP)

### 📌 Concept Name
**Bandwidth Delay Product (BDP)**

### 🧠 Simple Explanation

Bandwidth Delay Product is one of the most important parameters for transport layer performance. It tells you **how much data can be "in flight" (inside the network) at any given moment** between the sender and the receiver.

**Formula:**

> **BDP = Link Bandwidth × Link Delay**

Think of the network link between sender and receiver as a **pipe**:
- **Bandwidth** = the width (cross-section area) of the pipe — how much data can enter per second
- **Delay (latency)** = the length of the pipe — how long it takes for one bit to travel from sender to receiver
- **BDP** = the total volume of the pipe — how much data can fill the pipe at one time

### 🛠 Example from Transcript

- Bandwidth = 50 Kbps
- One-way transit delay = 250 ms
- BDP = 50 Kbps × 250 ms = 50,000 bits/sec × 0.25 sec = **12,500 bits = 12.5 Kbit**

Now, if your segment size is 1000 bits:
- BDP in segments = 12,500 / 1000 = **12.5 segments**

This means 12.5 segments' worth of data can be sitting inside the pipe (the link) at any moment.

### 🎯 Exam Important Points
- BDP = Link Bandwidth × Link Delay (one-way delay)
- It tells you the maximum amount of data that can be in transit in the network at one time
- BDP is measured in bits, but can be converted to segments by dividing by segment size
- BDP is critical for choosing window size and protocol type

### ⚠️ Common Confusions
- BDP uses **one-way delay**, not RTT. But when calculating outstanding segments in the whole round trip, you multiply BDP by 2 (because data goes one way, ACK comes back the other way).
- BDP is about the **link capacity**, not about how fast the application produces data.

---

## Concept 2: Round Trip Time (RTT)

### 📌 Concept Name
**Round Trip Time (RTT)**

### 🧠 Simple Explanation

RTT is the **total time** from the moment you send a data segment until the moment you receive its acknowledgement (ACK) back.

Think of it like this:
1. You (sender) send a packet to the receiver → this takes one-way latency
2. Receiver sends ACK back to you → this takes another one-way latency

So: **RTT = 2 × One-way latency**

If your one-way delay is 200 ms, then RTT = 400 ms.

This assumes the network is running smoothly, with no congestion or extra unexpected delays. In that ideal case, the end-to-end delay is just the propagation delay.

### 🎯 Exam Important Points
- RTT = 2 × one-way latency (in ideal conditions)
- RTT is the time from sending data to receiving its ACK
- Used to estimate how many segments can be outstanding at a time

### ⚠️ Common Confusions
- RTT is not the same as one-way delay. RTT is always double.
- In real networks, RTT may be larger than 2 × propagation delay due to congestion, queuing delays, etc. But in this lecture, we assume ideal conditions.

---

## Concept 3: Maximum Outstanding Segments and Window Size

### 📌 Concept Name
**Maximum Outstanding Segments During One RTT**

### 🧠 Simple Explanation

During one RTT, data is flowing in one pipe (sender → receiver) and ACKs are flowing in the other pipe (receiver → sender). Both pipes can be filled with data at the same time.

So the **maximum number of segments that can be outstanding** (sent but not yet acknowledged) is:

> **Maximum outstanding segments = 2 × BDP (in segments)**

Using the earlier example:
- BDP = 12.5 segments
- Maximum outstanding = 12.5 × 2 = **25 segments**

Why 25? Because:
- 12.5 segments of data are in the data pipe (going to receiver)
- 12.5 segments worth of ACKs are in the ACK pipe (coming back to sender)
- Together, 25 segments are "in flight"

Now, the slide also says: maximum outstanding = 25 + 1 = **26 segments**. The +1 is because one more ACK could have just arrived at the sender but not yet been processed.

### The Key Formula

> **w = 2 × BD + 1**

Where:
- **w** = optimal window size (sender window, swnd)
- **BD** = number of frames equivalent to BDP (i.e., BDP / segment size)

Setting the window size to this value means the **link will always be busy transmitting data segments** — you get **maximum link utilization**.

### 🎯 Exam Important Points
- Maximum outstanding segments = 2 × BD + 1 (this gives max link utilization)
- The sender window (swnd) should ideally be set to 2 × BD + 1
- This ensures the entire network capacity is fully used
- BD = BDP / segment size (number of segments that fit in one-way pipe)

### ⚠️ Common Confusions
- Students sometimes forget the "× 2" — remember, data goes one way, ACK comes back the other way, so both pipes are filled.
- The "+1" accounts for the ACK that has just been received but not processed.

---

## Concept 4: BDP and Its Implication on Protocol Design Choice

### 📌 Concept Name
**Using BDP to Choose Between Stop-and-Wait vs. Sliding Window**

### 🧠 Simple Explanation

Once you know the BDP and the segment size, you can decide which ARQ protocol is best.

**Key Idea:** If the segment size is **larger than the BDP**, the link cannot even hold one full segment. In that case, sending multiple segments in parallel (sliding window) gives no benefit. So Stop-and-Wait is better because it is simpler.

### 🛠 Example from Transcript

- Link bandwidth = 1 Mbps
- Delay = 1 ms
- Segment size = 1 KB = 1024 bytes = 8192 bits
- BDP = 1 Mbps × 1 ms = 1,000,000 × 0.001 = 1000 bits = **1 Kbit** (= 125 bytes)

The segment size (8192 bits) is **8 times larger** than BDP (1000 bits).

The link (pipe) cannot even hold one entire segment! So there is **no advantage** in sending multiple segments at the same time (sliding window). You cannot fill the pipe with even one segment, let alone multiple.

**Conclusion for this case:** Use **Stop-and-Wait** protocol. It is simpler (less overhead, no need to manage sender/receiver windows, no complex sequence numbers).

### When to Use Sliding Window

If BDP is much larger than segment size, you can fit many segments in the pipe. Then sliding window protocols (Go-Back-N or Selective Repeat) will give better performance because they keep the pipe full.

### 🎯 Exam Important Points
- If segment size > BDP → Stop-and-Wait is preferred (simpler, no parallelization benefit)
- If BDP >> segment size → Sliding Window protocols improve performance
- Stop-and-Wait has less complexity (no window management, simpler logic)
- Sliding Window has more overhead (sender/receiver windows, sequence numbers)

### ⚠️ Common Confusions
- Just because sliding window is "more advanced" does not mean it is always better. When BDP is small compared to segment size, sliding window adds unnecessary complexity with no performance gain.

---

## Concept 5: BDP and Sequence Number Space

### 📌 Concept Name
**Connecting Window Size to Sequence Number Space**

### 🧠 Simple Explanation

Once you have chosen the optimal window size (w = 2 × BD + 1), you need to decide how many bits to use for sequence numbers. The sequence number space depends on which protocol you use:

**For Go-Back-N ARQ:**
- Maximum window size = 2^n - 1 (where n = number of bits for sequence number)
- So choose n such that 2^n - 1 ≥ w

**For Selective Repeat ARQ:**
- Maximum window size = 2^n / 2
- So choose n such that 2^n / 2 ≥ w

Once you know the required window size from BDP, you can calculate how many sequence number bits are needed.

### 🎯 Exam Important Points
- Go-Back-N: max window = 2^n - 1
- Selective Repeat: max window = 2^n / 2
- Choose sequence number bits (n) based on the required window size from BDP
- This connects network parameters (bandwidth, delay) to protocol design parameters (sequence number bits)

### ⚠️ Common Confusions
- Go-Back-N and Selective Repeat have different window size formulas — don't mix them up.
- The window size is derived from BDP first, then the sequence number space is determined from the window size.

---

## Concept 6: Application-Transport Interfacing — Sender Side

### 📌 Concept Name
**How Application Layer Sends Data to Transport Layer (Sender Side)**

### 🧠 Simple Explanation

This part explains the internal design of how an application (like a browser) passes data down to the transport layer inside the operating system. The example is based on the Linux OS.

There are two spaces:
- **User Space:** Where your application runs
- **Kernel Space:** Where the transport layer protocol runs

**How data flows (sender side):**

1. The application generates data (e.g., at 10 Mbps)
2. The application uses system calls like **write()** or **send()** to push data to the kernel
3. In the kernel, there is a **Transport Buffer (Sender Buffer)** — this stores the data temporarily
4. A function called **TportSend()** is triggered periodically by the **Transmission Rate Control** module
5. The Transmission Rate Control module decides the rate at which data should be sent (e.g., 2 Mbps), based on the flow control algorithm and the current window size
6. TportSend() picks data from the buffer and sends it down to the IP layer (network layer)

### The Rate Mismatch Problem

The application may produce data at a higher rate (e.g., 10 Mbps) than the transport layer can send (e.g., 2 Mbps). This is because the network may not support 10 Mbps end-to-end.

To handle this mismatch, we need an **intermediate buffer** — the **Transport Buffer at the Sender side**.

### Connection-Specific Buffering

Different connections (e.g., one connection to a web server, another to an email server) are treated independently. So each connection has its **own separate transport buffer**. This is called **connection-specific source buffering**.

### Blocking Behavior

The **write() system call is a blocking call**. This means:
- When the application calls write(), it blocks (waits) until all the data is written into the transport buffer
- If the buffer is full (because the transport layer is sending slower than the application is producing), the write() call **blocks the application** — the application cannot send more data until space is available in the buffer
- This prevents **buffer overflow** at the sender side

### 🎯 Exam Important Points
- Application sends data via write()/send() system calls to the kernel
- Transport buffer at sender stores data between application and network
- Transmission Rate Control triggers TportSend() periodically
- TportSend() sends data from buffer to IP layer
- Application rate and transport rate can be different (asynchronous)
- Each connection has its own separate buffer (connection-specific)
- write() is a blocking call — blocks if buffer is full

### ⚠️ Common Confusions
- The function names (TportSend, etc.) are **hypothetical** — they are used for explanation, not the actual function names in Linux
- "Blocking" means the application pauses/waits, not that data is lost

---

## Concept 7: Application-Transport Interfacing — Receiver Side

### 📌 Concept Name
**How Transport Layer Delivers Data to Application (Receiver Side)**

### 🧠 Simple Explanation

On the receiver side, the process is the reverse:

1. Data arrives from the network (IP layer)
2. A function called **TportRecv()** receives the data from the network layer
3. TportRecv() looks at the **port number** in the transport layer header to determine which application this data belongs to
4. Based on the port number, it places the data into the correct **Transport Buffer (Receiver Buffer)** — each application/connection has its own buffer (identified by port number)
5. The application makes a **read()** or **recv()** system call to get data from this buffer

### Blocking Behavior at Receiver

The **read()/recv() call is also a blocking call**:
- When the application calls read(), if the buffer is **empty** (no data has arrived yet), the call **blocks** — the application waits
- When data arrives in the buffer, an **interrupt** signal is sent to the blocked call
- The call then reads the complete data from the buffer and returns it to the application

### How the Interrupt Works

There is a function called **CheckBuffer()**. When the application makes a read() call and the buffer is empty, the call waits on CheckBuffer(). The moment data arrives in the buffer, CheckBuffer() sends an interrupt, and the read() call completes.

### Summary of Blocking at Both Sides

| Side | Call | Blocks When |
|------|------|-------------|
| Sender | write()/send() | Buffer is **full** |
| Receiver | read()/recv() | Buffer is **empty** |

### 🎯 Exam Important Points
- TportRecv() receives data from IP layer and places it in the correct buffer based on port number
- Each port/application has a separate receiver buffer
- read()/recv() is a blocking call — blocks when buffer is empty
- Interrupt mechanism (CheckBuffer) unblocks the call when data arrives
- Sender blocks when buffer full; Receiver blocks when buffer empty

### ⚠️ Common Confusions
- Port number is used to identify which application's buffer to fill — this is the demultiplexing function
- "Blocking" at receiver means the application waits (does not crash or lose data)

---

## Concept 8: Organizing the Transport Buffer Pool

### 📌 Concept Name
**How to Organize Transport Layer Buffers**

### 🧠 Simple Explanation

The transport buffer is a software buffer. The question is: how do you organize the memory for this buffer? There are three approaches discussed in the lecture:

### Approach 1: Pool of Identically Sized Buffers

- If all segments are nearly the same size, create a pool of fixed-size buffers
- Each buffer slot holds exactly one segment
- Buffer slot size = segment size
- **Advantage:** Simple to implement
- **Disadvantage:** If segment sizes vary widely, space is wasted. For example, if the buffer slot is 1024 bytes but a segment is only 10 bytes, 1014 bytes are wasted per slot.

### Approach 2: Chained Fixed-Size Buffers (Variable Segment Size)

- For variable segment sizes, use a chain of fixed-size buffers connected like a **linked list**
- Each individual buffer is the maximum segment size
- Multiple buffers are linked together to form the complete buffer pool for one port/connection
- **Problem:** If segments vary a lot in size, small segments waste space in large buffer slots. If you make buffer size small, you need multiple buffers for one large segment, which adds complexity.

### Approach 3: Variable Size Buffers

- Each buffer in the pool has a different size (matching the segment it stores)
- Connected via a linked list data structure
- **Advantage:** Better memory utilization — large segments go in large buffers, small segments go in small buffers
- **Disadvantage:** Complicated implementation because you need dynamic memory allocation

### Approach 4: Single Large Circular Buffer

- One big circular buffer for every connection
- Segments of different sizes are stored one after another in the circular buffer
- Unused space at the end can be reused when the buffer wraps around
- **Advantage:** Good use of memory when connections are heavily loaded
- **When it works best:** When connections are heavily loaded, so the circular buffer is well-utilized

### How to Choose?

The choice depends on your application:
- If segments are mostly the same size → Pool of identical buffers
- If segments vary widely and connections are light → Variable size buffers
- If connections are heavily loaded → Circular buffer

### 🎯 Exam Important Points
- Four ways to organize buffers: identical pool, chained fixed-size, variable size, circular
- Identical pool: simple but wastes space if segments vary
- Chained fixed-size: like a linked list, wastes space with varied segments
- Variable size: better memory use, harder to implement (dynamic allocation)
- Circular buffer: good for heavily loaded connections
- Choice depends on application needs and segment size patterns

### ⚠️ Common Confusions
- These are **software buffers** managed by the OS kernel, not hardware
- The buffer organization is **per-connection** (identified by port number)
- Don't confuse the transport buffer with the sliding window buffer — the transport buffer is for interfacing with the application; the sliding window is for flow control

---

## Concept 9: Putting It All Together — Transport Layer Performance Design

### 📌 Concept Name
**Summary: Transport Layer Performance and Design Decisions**

### 🧠 Simple Explanation

This lecture taught you three big design decisions for transport layer performance:

**Decision 1: What window size to use?**
→ Calculate BDP, then use w = 2 × BD + 1 for maximum link utilization

**Decision 2: Which protocol to use?**
→ Compare BDP with segment size. If segment size > BDP, use Stop-and-Wait. Otherwise, use Sliding Window.

**Decision 3: How to manage buffers?**
→ Choose buffer organization (identical, chained, variable, circular) based on your application's segment patterns and load.

### 🎯 Exam Important Points
- BDP drives both window size selection and protocol choice
- Application-transport interface uses blocking system calls and per-connection buffers
- Buffer organization depends on segment size variation and connection load

---

## 📝 Possible NPTEL-Style MCQs from Lecture 16

### Q1.
**What is the formula for Bandwidth Delay Product (BDP)?**

(a) BDP = Bandwidth / Delay  
(b) BDP = Bandwidth + Delay  
(c) BDP = Bandwidth × Delay  
(d) BDP = Delay / Bandwidth  

**Answer:** (c)  
**Explanation:** BDP is the product of link bandwidth and link delay. It tells you how much data can be in transit in the network at one time.

---

### Q2.
**If the link bandwidth is 50 Kbps and one-way delay is 250 ms, what is the BDP?**

(a) 200 Kbit  
(b) 12.5 Kbit  
(c) 25 Kbit  
(d) 50 Kbit  

**Answer:** (b)  
**Explanation:** BDP = 50 Kbps × 250 ms = 50,000 × 0.25 = 12,500 bits = 12.5 Kbit.

---

### Q3.
**RTT (Round Trip Time) is equal to:**

(a) One-way latency  
(b) Half of one-way latency  
(c) Twice the one-way latency  
(d) Four times the one-way latency  

**Answer:** (c)  
**Explanation:** RTT = time to send data + time for ACK to return = 2 × one-way latency (in ideal conditions with no congestion).

---

### Q4.
**For maximum link utilization, the optimal sender window size (w) should be:**

(a) w = BD  
(b) w = 2 × BD  
(c) w = 2 × BD + 1  
(d) w = BD + 1  

**Answer:** (c)  
**Explanation:** w = 2 × BD + 1, where BD is the number of frames equivalent to BDP. The ×2 accounts for data pipe + ACK pipe, and +1 accounts for the ACK just received but not processed.

---

### Q5.
**Given: Bandwidth = 1 Mbps, Delay = 1 ms, Segment size = 1 KB. Which protocol is better?**

(a) Go-Back-N  
(b) Selective Repeat  
(c) Stop-and-Wait  
(d) Any sliding window protocol  

**Answer:** (c)  
**Explanation:** BDP = 1 Mbps × 1 ms = 1000 bits = 125 bytes. Segment size = 1024 bytes. Segment size is 8 times larger than BDP. The link cannot hold even one full segment. Sliding window gives no benefit. Stop-and-Wait is better because it is simpler with less overhead.

---

### Q6.
**If the segment size is much larger than BDP, sliding window protocols do not improve performance because:**

(a) The receiver buffer is too small  
(b) The link cannot hold even one full segment, so parallelization is useless  
(c) The sequence numbers run out  
(d) The ACK is lost every time  

**Answer:** (b)  
**Explanation:** When the pipe (link) is smaller than one segment, you cannot benefit from sending multiple segments in parallel. The pipe cannot hold multiple segments at once.

---

### Q7.
**On the sender side, the write() system call blocks when:**

(a) The transport buffer is empty  
(b) The transport buffer is full  
(c) The network is idle  
(d) The ACK is received  

**Answer:** (b)  
**Explanation:** At the sender side, write() blocks the application when the transport buffer is full. This prevents buffer overflow. The application must wait until space becomes available.

---

### Q8.
**On the receiver side, the read()/recv() system call blocks when:**

(a) The transport buffer is full  
(b) The network has congestion  
(c) The transport buffer is empty  
(d) The sender is idle  

**Answer:** (c)  
**Explanation:** At the receiver side, read()/recv() blocks when the buffer is empty (no data has arrived yet). Once data arrives, an interrupt unblocks the call.

---

### Q9.
**In the circular buffer approach for organizing transport buffers:**

(a) Each buffer slot is the same size  
(b) Memory is wasted when connections are lightly loaded  
(c) It provides good memory utilization when connections are heavily loaded  
(d) Each segment must be the same size  

**Answer:** (c)  
**Explanation:** A single large circular buffer per connection stores segments of varying sizes one after another. It works well when connections are heavily loaded because the buffer stays well-utilized.

---

### Q10.
**For a Go-Back-N protocol, if the optimal window size needed is 25, the minimum number of sequence number bits (n) required is:**

(a) 4  
(b) 5  
(c) 6  
(d) 3  

**Answer:** (b)  
**Explanation:** For Go-Back-N, max window size = 2^n - 1. We need 2^n - 1 ≥ 25, so 2^n ≥ 26. 2^5 = 32 ≥ 26. So n = 5 bits.

---

*End of Lecture 16 — Transport Layer Performance*
