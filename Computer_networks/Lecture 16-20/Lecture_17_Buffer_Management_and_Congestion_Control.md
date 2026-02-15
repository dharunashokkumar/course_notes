# Lecture 17: Buffer Management and Congestion Control

## 📌 Lecture Overview

This lecture is part of the **Transport Layer** series. In the previous lecture (Lecture 16), you learned about basic performance modules in the transport layer and a hypothetical transport layer protocol for interfacing with the application layer.

In **Lecture 17**, two major topics are covered:

1. **Buffer Management at the Transport Layer (Receiver Side)**
2. **Congestion Control Algorithms**

Let's learn each concept step-by-step.

---

## CONCEPT 1: Transport Buffer at the Receiver Side

### 📌 What is a Transport Buffer?

A **transport buffer** is a **software queue** maintained at both the sender side and the receiver side in the transport layer.

### 🧠 Simple Explanation

Think of the transport buffer like a **waiting room**.

- **Sender side:** When the application wants to send data, it puts the data into this queue. The transport layer picks data from this queue and sends it to the network layer (unreliable IP layer).
- **Receiver side:** When data arrives from the network layer, it is placed into this queue. The application then reads data from this queue using system calls.

**Key point from transcript:** The transport layer uses **port numbers** to identify which buffer (which application) the incoming data belongs to.

### 🛠 How Does It Work (Receiver Side)?

1. The network layer receives data using a hypothetical function called `ip_receive`.
2. This data is placed into the receiver's transport layer buffer (queue).
3. The application uses the **`read` system call** (part of socket programming) to fetch data from this buffer.
4. At the sender side, the application uses the **`write` system call** to send data.

**Important architectural point:** The protocol stack (transport layer implementation) is inside the **Kernel** (operating system), while the application runs in **user space**.

### 🎯 Exam Important Points

- Transport buffer = software queue at both sender and receiver
- Port numbers identify which buffer receives data
- `read` system call = application reads from transport buffer
- `write` system call = application sends data to transport buffer
- Protocol stack = inside OS Kernel; Application = user space
- The frequency of the `read` call is managed by the application

### ⚠️ Common Confusion

- Students confuse transport buffer with network layer buffer. Transport buffer is at the **transport layer**, not at routers.
- The `read` and `write` system calls are part of **socket programming**, which is discussed in later lectures.

---

## CONCEPT 2: The Rate Mismatch Problem

### 📌 What Is the Problem?

The rate at which the **network delivers data** to the receiver buffer can be **different** from the rate at which the **application reads data** from the buffer.

### 🧠 Simple Explanation

Imagine a water pipe (network) filling a bucket (buffer) at 1 litre per second, but someone is drinking from the bucket (application reading) at only 0.1 litre per second. The bucket will overflow!

**From the transcript:**

- Network may receive data at **1 Mbps**
- Application may read data at only **10 Kbps**
- Because of this difference, data keeps accumulating in the buffer
- Eventually, the **buffer becomes full**
- Once the buffer is full → **packet drop** happens (new incoming data is lost)

### 🎯 Exam Important Points

- Rate mismatch between network receive rate and application read rate causes buffer overflow
- Buffer overflow leads to **packet drop**
- This is why we need **dynamic buffer management** and **flow control**

### ⚠️ Common Confusion

- This is NOT congestion. This is a local problem at the receiver side due to speed mismatch between application and transport layer.

---

## CONCEPT 3: Dynamic Buffer Management for Window-Based Flow Control

### 📌 What Is Dynamic Buffer Management?

It is a mechanism where the **receiver tells the sender** how much free buffer space it has, so the sender does not send more data than the receiver can hold.

### 🧠 Simple Explanation

Imagine you are sending parcels to a friend's small room. Your friend tells you: "I have space for only 4 more boxes." So you send only 4. Once your friend clears some boxes, they tell you: "Now I have space for 3 more." You adjust accordingly.

**From the transcript:**

- Sender and receiver **dynamically adjust** their buffer allocations.
- The receiver buffer has three parts:
  1. **Data already read** by the application (freed space)
  2. **Data waiting** inside the buffer (occupied)
  3. **Free space** available for new data
- The **free space** is **advertised to the sender** so the sender limits its window size to that value.
- The sender's **window size** = maximum amount of data it can send without waiting for an acknowledgement.
- Sender sets its window size ≤ receiver's advertised free buffer space.

### 🛠 How Does This Work Step-by-Step?

1. Receiver checks how much free buffer space is available
2. Receiver sends this information along with the ACK to the sender
3. Sender adjusts its sliding window size to not exceed that advertised space
4. Sender sends data within the window limit
5. Once the application reads data from the buffer, more free space becomes available
6. Receiver sends a new ACK with the updated available buffer space

### 🎯 Exam Important Points

- Receiver **advertises available buffer space** through ACK messages
- Sender window size ≤ Receiver advertised buffer space
- This prevents sender from overwhelming the receiver
- This is tied to **sliding window flow control**

---

## CONCEPT 4: Detailed Example of Dynamic Buffer Management (Nodes A and B)

### 📌 The Scenario

Node **A** = Sender, Node **B** = Receiver. Each segment is assumed to be fixed size (simplified assumption; not true for TCP in general).

### 🧠 Step-by-Step Walkthrough

**Step 1:** A requests 8 buffer spaces from B.

**Step 2:** B only has 4 buffer spaces available. B sends ACK with **buffer space = 4**.

**Step 3:** A sends message m0. Now A has 3 buffers left.

**Step 4:** A sends message m1. Now A has 2 buffers left.

**Step 5:** A sends message m2. But this message is **lost** in the network. A thinks it has 1 buffer left.

**Step 6:** B acknowledges m0 and m1 using a **cumulative acknowledgement** (ACK = 1). B also advertises **buffer space = 3**.

**Step 7:** A now knows: m0 and m1 received. Buffer space = 3. A sends m3, m4, and m2 (retransmission). After sending 3 messages, A's window is exhausted.

**Step 8:** B sends ACK = 4 (cumulative, acknowledging up to m4) with **buffer space = 0**. This means B's buffer is completely full. The application has NOT read the data yet.

**Step 9:** A is now **blocked** — it cannot send any more data because buffer space = 0.

**Step 10:** Once B's application reads some data, B sends another ACK with the same ACK number (4) but **buffer space = 1**. Now A can send 1 more segment.

### 🎯 Exam Important Points

- Cumulative ACK: ACK number N means all segments up to N-1 are received
- Buffer space is advertised with every ACK
- When buffer space = 0, sender gets **blocked**
- Sender resumes only when receiver advertises new buffer space

---

## CONCEPT 5: Deadlock Problem and Its Solution

### 📌 What Is the Deadlock?

If the receiver sends buffer space = 0, and then later sends a new ACK with available buffer space, but that **ACK gets lost** in the network — the sender will wait forever, and the receiver will wait for data forever. This is a **deadlock**.

### 🧠 Simple Explanation

It's like your friend says "My room is full, don't send anything." Later they clear space and call you to say "Send more!" — but the phone call doesn't connect. You both wait forever.

### 🛠 Solution (from transcript)

- After every **timeout**, if B is not receiving any more data from A and the connection is still open, B should **resend a duplicate ACK** announcing available buffer space.
- This ensures A gets the update and can resume sending.

### 🎯 Exam Important Points

- Deadlock occurs when buffer space = 0 ACK is sent, and subsequent ACK with available space is lost
- Solution: Receiver sends **duplicate ACKs** after timeout with updated buffer space
- This keeps ACKs flowing continuously in the network

### ⚠️ Common Confusion

- This deadlock is NOT a congestion issue. It is a flow control issue caused by lost ACKs when buffer is full.

---

## CONCEPT 6: Congestion Control — Introduction

### 📌 What Is Congestion?

Congestion happens when **more data is being pushed into the network than the network can handle**, specifically at bottleneck links.

### 🧠 Simple Explanation

Think of a road network: if too many cars try to use a narrow road at the same time, there's a traffic jam. Similarly, in a computer network, if too much data is sent through a bottleneck link, the buffers at intermediate routers fill up, packets get dropped, and delays increase.

### 🛠 Max-Flow Min-Cut Example (from transcript)

In a centralized network, you can calculate the maximum flow from source S to destination D using the **max-flow min-cut theorem** (from algorithms course).

**Example from transcript:**

- A network has links with capacities
- The minimum cut gives the bottleneck capacity
- In the example: bottleneck = 6 + 4 + 10 + 2 = **12 Mbps** (this was the example calculation, though the transcript notes the specific link values)
- You can never send more than 12 Mbps from S to D, even with no other flows

**The problem:** In a **distributed network**, you cannot apply this centralized algorithm because individual routers and end hosts do NOT have complete network information. This is why congestion happens — senders may push more data than the bottleneck can handle.

### 🎯 Exam Important Points

- Bottleneck capacity = maximum flow capacity on the path
- Max-flow min-cut theorem can find bottleneck in centralized scenarios
- In distributed networks, centralized algorithms cannot be applied
- Congestion = intermediate buffer overflow → packet loss → increased delay

---

## CONCEPT 7: Multiple Flows Sharing a Bottleneck Link

### 📌 What Happens When Multiple Flows Share a Link?

When multiple flows share the same bottleneck link, the total bandwidth must be divided among them.

### 🧠 Step-by-Step Example (from transcript)

Assume a bottleneck link capacity = **1 Mbps**.

1. **Flow 1 starts alone** → It gets the full **1 Mbps**.
2. **Flow 2 starts** → Both F1 and F2 share the link → Each gets approximately **0.5 Mbps** (if their sending rate exceeds 0.5 Mbps).
3. **Flow 3 starts** with a required rate of only **100 Kbps** → F3 takes 100 Kbps. The remaining **900 Kbps** is shared between F1 and F2.
4. **Flow 2 finishes** → Now F1 gets approximately **900 Kbps**, and F3 still uses **100 Kbps**.

### 🎯 Exam Important Points

- Bandwidth allocation among flows changes dynamically as flows enter and exit
- Flows share bottleneck link capacity
- If a flow needs less bandwidth, the remaining is shared among others

---

## CONCEPT 8: Congestion Avoidance Algorithm

### 📌 Why Congestion Avoidance (Not Control)?

From the transcript: "We apply something called a **congestion avoidance** algorithm rather than a congestion control because **a priori estimation of congestion is difficult**."

In simple words: We cannot predict congestion in advance in a distributed network. So we **detect** congestion when it happens and **react** to it.

### 🧠 The Core Formula

**Sending Rate = minimum(Network Rate, Receiver Rate)**

- **Receiver Rate** → Comes from the flow control algorithm (receiver advertised window size)
- **Network Rate** → We do NOT have a direct way to know this. So we **gradually increase** the network rate and **observe** what happens.

### 🛠 How Does Congestion Detection Work?

1. In a **wired network**, loss due to channel errors is very small.
2. So, if a **packet loss** occurs, it is most likely due to **buffer overflow at intermediate routers**.
3. Buffer overflow means: **Total incoming rate (λ) > Total outgoing rate (μ)**
4. This gives a **signature of congestion**.

**The broad approach:**

1. Gradually **increase** the network rate
2. At some point, you will experience **packet loss** (congestion detected)
3. **Drop** the rate
4. Again gradually **increase** the rate
5. Repeat this process

### 🛠 Road Network Analogy (from transcript)

Think of two roads merging into a narrow road. If the total traffic from both roads exceeds the narrow road's capacity, there's a jam (congestion). The same thing happens in network buffers.

### 🎯 Exam Important Points

- Sending Rate = min(Network Rate, Receiver Rate)
- We cannot directly measure network rate → gradually increase and observe
- Packet loss = indicator of congestion (buffer overflow at routers)
- λ > μ at a router means congestion (incoming rate exceeds outgoing rate)
- Congestion **avoidance** (not control) because we react to congestion, not predict it

### ⚠️ Common Confusion

- Flow control ≠ Congestion control. Flow control manages receiver buffer overflow. Congestion control manages network buffer overflow at intermediate routers.

---

## CONCEPT 9: Congestion Collapse — Impact on Goodput and Delay

### 📌 What Is Goodput?

Goodput = the number of **useful packets per second** successfully received at the transport layer (not counting retransmissions).

### 🧠 What Happens During Congestion?

From the transcript:

1. As offered load increases, goodput increases up to the **maximum capacity**.
2. When congestion occurs, packets get **dropped**.
3. The flow control algorithm tries to **retransmit** dropped packets.
4. But if the link is still congested, retransmitted packets may also get dropped.
5. This causes a **significant drop in goodput** — this is called **congestion collapse**.
6. During congestion collapse, **delay increases significantly** because packets keep getting dropped and retransmitted.

### 🎯 Exam Important Points

- **Congestion collapse** = sharp drop in goodput when load exceeds capacity
- Retransmitted packets may also get dropped in congestion → worsens the situation
- Delay increases dramatically during congestion collapse
- Goodput ≠ Throughput. Goodput counts only successfully delivered useful data.

### ⚠️ Common Confusion

- Throughput includes retransmissions. Goodput does NOT. Congestion collapse affects goodput severely.

---

## CONCEPT 10: Congestion Control and Fairness

### 📌 What Is Fairness?

Fairness ensures that the rate of **all flows** in the network is controlled in a **fair way** — no flow should be starved.

### 🧠 Simple Explanation

If congestion control is bad, some flows may get a lot of bandwidth while others get almost none (starvation). A good congestion control algorithm ensures fair sharing.

### 📌 Why Is Hard Fairness Difficult?

From the transcript: In a **decentralized network**, ensuring hard fairness requires complete network information and complex mathematical calculations (like min-cut theorem). This is **very difficult** in practice.

So instead, we aim for **Max-Min Fairness**.

---

## CONCEPT 11: Max-Min Fairness

### 📌 Definition (from transcript)

> "An allocation is max-min fair if the bandwidth given to one flow cannot be increased without decreasing the bandwidth given to another flow with an allocation."

### 🧠 Simple Explanation

If you have two flows A₁ and A₂, and the allocation is max-min fair:

- You **cannot give more bandwidth to A₁** without **taking some away from A₂**.
- And vice versa.

In other words, everyone is getting the most they can get without hurting others.

### 🛠 Detailed Example (from transcript)

A network has 4 flows: **A, B, C, D** and multiple links.

- The **bottleneck link** is shared by **3 flows** (B, C, D).
- Since 3 flows share the bottleneck, each gets **1/3** of the bandwidth.
- Flow D uses 1/3 of the bottleneck bandwidth.
- Flow C uses 1/3 of the bottleneck bandwidth.
- Flow B uses 1/3 of the bottleneck bandwidth.
- On another link, only Flow A and Flow B share it. Since Flow B is already limited to 1/3, it uses 1/3 on this link too. The remaining **2/3** goes to Flow A.

**Why is this max-min fair?**

- If you try to increase Flow B's bandwidth beyond 1/3, you must decrease Flow C or Flow D (because of the bottleneck link).
- If you try to increase Flow C or Flow D, you must decrease Flow B.
- No flow can get more without another flow getting less.

### 🎯 Exam Important Points

- Max-min fairness: Cannot increase one flow's bandwidth without decreasing another's
- Bottleneck link determines the fair share
- Flows sharing the bottleneck get equal share of bottleneck bandwidth
- Remaining capacity on non-bottleneck links can be used by flows not limited there

---

## CONCEPT 12: AIMD — Additive Increase Multiplicative Decrease

### 📌 What Is AIMD?

**AIMD** stands for **Additive Increase Multiplicative Decrease**. It was proposed by **Chiu and Jain in 1989**.

It is the algorithm used to achieve **max-min fairness in a distributed way**.

### 🧠 How Does AIMD Work?

Let **w(t)** = sending rate at time t.

- **a** = additive increase factor
- **b** = multiplicative decrease factor (0 < b < 1)

**Rule:**

- **No congestion detected** → Increase rate **additively**: w(t+1) = w(t) + a
- **Congestion detected** (packet loss) → Decrease rate **multiplicatively**: w(t+1) = w(t) × b

Since b is between 0 and 1, multiplying by b means you are **dropping** the rate significantly.

### 🛠 The Pattern

1. Rate increases linearly (slowly, one step at a time)
2. When congestion is detected (loss), rate drops sharply (multiplicative drop)
3. Then again increases linearly
4. This creates a **sawtooth pattern** over time

### 🎯 Exam Important Points

- AIMD = Additive Increase, Multiplicative Decrease
- Proposed by Chiu and Jain (1989)
- Additive increase: w(t+1) = w(t) + a (when no congestion)
- Multiplicative decrease: w(t+1) = w(t) × b, where 0 < b < 1 (when congestion detected)
- Used by **TCP** to adjust sliding window size
- Achieves max-min fairness in a distributed way

---

## CONCEPT 13: Why AIMD? Comparison with AIAD and MIMD

### 📌 What Are AIAD and MIMD?

- **AIAD** = Additive Increase, Additive Decrease (increase and decrease both by adding/subtracting a fixed value)
- **MIMD** = Multiplicative Increase, Multiplicative Decrease (increase and decrease both by multiplying)

### 🧠 Two-Flow Example Explained

Imagine two users sharing a bottleneck link. We plot their bandwidth allocations on a graph:

- **X-axis** = User A's bandwidth
- **Y-axis** = User B's bandwidth
- **Fairness line** = 45° line (where both users get equal bandwidth)
- **Efficiency line** = line where total allocation = 100% of link capacity

**The optimal point** = where fairness line and efficiency line intersect (both flows get 50% each, and the full link is utilized).

### 🛠 What Happens with Each Algorithm?

**AIAD (Additive Increase, Additive Decrease):**

- You start at some point
- Increase additively → move diagonally (both users increase equally)
- When you cross the efficiency line → congestion detected → decrease additively
- Result: You **oscillate along the efficiency line** but NEVER converge to the optimal point

**MIMD (Multiplicative Increase, Multiplicative Decrease):**

- You increase multiplicatively and decrease multiplicatively
- Result: You **oscillate along the efficiency line** (with a different slope from AIAD) but still do NOT converge to the optimal point

**AIMD (Additive Increase, Multiplicative Decrease):**

- You start at some point
- Additive increase → move at 45° angle
- Cross efficiency line → congestion → Multiplicative decrease toward the origin
- Then additive increase again → multiplicative decrease again
- Result: You **gradually converge toward the optimal point!**

### 📌 Why Does AIMD Converge?

- Additive increase: Both users increase at the same rate → moves at 45° (toward fairness)
- Multiplicative decrease: Both users reduce by the same fraction → moves toward the origin (proportionally)
- The combination of these two movements brings the system closer and closer to the optimal point with every cycle

### 🎯 Exam Important Points

- AIAD → oscillates on efficiency line, does NOT converge to optimal
- MIMD → oscillates on efficiency line (different slope), does NOT converge to optimal
- **AIMD → converges to the optimal point** (fair + efficient)
- AIMD is used by **TCP**
- Additive increase = 45° angle movement; Multiplicative decrease = toward origin
- Optimal point = intersection of fairness line and efficiency line

### ⚠️ Common Confusion

- Students confuse the fairness line with the efficiency line. Fairness line = equal sharing (45°). Efficiency line = full utilization (x + y = total capacity).
- AIMD does NOT reach the optimal point in one step. It **gradually converges** over multiple cycles.

---

## CONCEPT 14: TCP Uses AIMD

### 📌 Key Takeaway from the Transcript

From the transcript: "This particular algorithm is used by **TCP** to adjust the size of the **sliding window** to control the rates."

- TCP uses AIMD to adjust its congestion window
- This will be discussed in more detail in later lectures

### 🎯 Exam Important Points

- TCP uses AIMD for congestion control
- TCP adjusts the sliding window size based on AIMD
- Details of TCP's implementation will come in future lectures

---

## Summary Table

| Concept | Key Idea |
|---|---|
| Transport Buffer | Software queue at sender and receiver sides |
| Rate Mismatch | Network delivers faster than app reads → buffer overflow → packet loss |
| Dynamic Buffer Management | Receiver advertises free space; sender limits window accordingly |
| Deadlock Problem | Lost ACK with buffer space info can block sender forever; solved by duplicate ACKs on timeout |
| Congestion | Too much data pushed into bottleneck → buffer overflow at routers |
| Congestion Avoidance | React to congestion (packet loss) rather than predict it |
| Sending Rate Formula | Sending Rate = min(Network Rate, Receiver Rate) |
| Congestion Collapse | Sharp drop in goodput when load exceeds capacity |
| Fairness | All flows should get fair share; hard fairness is difficult in distributed networks |
| Max-Min Fairness | Cannot increase one flow without decreasing another |
| AIMD | Additive Increase + Multiplicative Decrease → converges to optimal point |
| AIAD / MIMD | Both oscillate on efficiency line, do NOT converge to optimal |
| TCP | Uses AIMD to adjust sliding window for congestion control |

---

## 📝 10 MCQs from Lecture 17 (Strictly from Transcript)

---

### Q1. What causes packet drop at the receiver's transport layer buffer?

(A) Network congestion at routers
(B) Sender sending data too slowly
(C) Application reading data slower than the network delivers it, causing buffer overflow
(D) Port number mismatch

**Answer: (C)**
**Explanation:** The transcript explains that when the application reads at a slower rate (e.g., 10 Kbps) than the network delivers (e.g., 1 Mbps), the buffer fills up and packets get dropped.

---

### Q2. In dynamic buffer management, what does the receiver advertise to the sender?

(A) Total buffer size
(B) Number of segments received so far
(C) Available free buffer space
(D) Maximum segment size

**Answer: (C)**
**Explanation:** The transcript clearly states that the receiver advertises the **available free buffer space** to the sender through ACK messages, so the sender limits its window size accordingly.

---

### Q3. What happens when the receiver advertises buffer space = 0?

(A) The sender retransmits all data
(B) The sender gets blocked and cannot send more data
(C) The connection is terminated
(D) The sender increases its window size

**Answer: (B)**
**Explanation:** From the transcript: when buffer space = 0 is advertised, the sender gets blocked and cannot send any more data until the receiver announces available buffer space.

---

### Q4. How is the deadlock problem solved when the ACK with available buffer space is lost?

(A) Sender sends a probe packet
(B) Connection is reset
(C) Receiver sends duplicate ACKs after timeout announcing available buffer space
(D) Sender assumes buffer space is available after a fixed time

**Answer: (C)**
**Explanation:** The transcript states that after every timeout, if B is not receiving data from A and the connection is still open, B should send a **duplicate acknowledgement** announcing available buffer space.

---

### Q5. What does the Sending Rate formula in congestion control equal?

(A) Maximum of network rate and receiver rate
(B) Network rate only
(C) Receiver rate only
(D) Minimum of network rate and receiver rate

**Answer: (D)**
**Explanation:** The transcript explicitly states: **Sending Rate = minimum(Network Rate, Receiver Rate)**.

---

### Q6. In a wired network, packet loss primarily indicates:

(A) Channel error
(B) Buffer overflow at intermediate routers (congestion)
(C) Application crash
(D) DNS failure

**Answer: (B)**
**Explanation:** The transcript explains that in wired networks, loss due to channel error is very small. So if packet loss occurs, it is most likely due to **buffer overflow at intermediate routers**, indicating congestion.

---

### Q7. What is congestion collapse?

(A) A sudden increase in throughput
(B) A significant drop in goodput when offered load exceeds network capacity
(C) When all routers in the path fail
(D) When the receiver buffer is full

**Answer: (B)**
**Explanation:** From the transcript: Congestion collapse is when goodput drops significantly because packets get dropped and retransmitted packets may also be dropped during congestion.

---

### Q8. What is max-min fairness?

(A) All flows get maximum bandwidth
(B) An allocation where no flow's bandwidth can be increased without decreasing another flow's bandwidth
(C) All flows get minimum bandwidth
(D) Only the largest flow gets bandwidth

**Answer: (B)**
**Explanation:** The transcript defines max-min fairness as: "An allocation is max-min fair if the bandwidth given to one flow cannot be increased without decreasing the bandwidth given to another flow."

---

### Q9. In the AIMD algorithm, what happens when congestion is detected?

(A) Rate is increased additively
(B) Rate is increased multiplicatively
(C) Rate is decreased multiplicatively (multiplied by b where 0 < b < 1)
(D) Rate is set to zero

**Answer: (C)**
**Explanation:** AIMD means Additive Increase Multiplicative Decrease. When congestion is detected, the rate is **decreased multiplicatively** by multiplying with factor b (between 0 and 1).

---

### Q10. Why does AIMD converge to the optimal point while AIAD and MIMD do not?

(A) AIMD uses faster increase rate
(B) AIAD and MIMD oscillate along the efficiency line, while AIMD's combination of 45° additive increase and multiplicative decrease toward origin gradually converges to the optimal fair + efficient point
(C) AIMD always uses the full bandwidth
(D) AIAD and MIMD cause packet loss

**Answer: (B)**
**Explanation:** From the transcript: AIAD oscillates along the efficiency line. MIMD also oscillates along the efficiency line (different slope). But AIMD's additive increase (45° toward fairness) combined with multiplicative decrease (toward origin) gradually moves toward the **optimal point** where both fairness and efficiency are achieved.

---

*End of Lecture 17 Notes — Buffer Management and Congestion Control*
