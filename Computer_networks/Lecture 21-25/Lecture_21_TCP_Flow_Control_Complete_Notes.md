# Lecture 21: Transmission Control Protocol – III (Flow Control)

**Course:** Computer Networks and Internet Protocol
**Instructor:** Prof. Sandip Chakraborthy, IIT Kharagpur

---

## Overview of This Lecture

This lecture covers three major areas of TCP:

1. TCP Flow Control Algorithm (Sliding Window with Go-Back-N)
2. Handling small segments (Delayed ACK, Nagle's Algorithm, Silly Window Syndrome, Clark's Solution)
3. Handling out-of-order segments and Duplicate ACKs
4. TCP Timers (RTO, SRTT, EWMA, Karn's Algorithm, Persistent Timer, Keepalive Timer, Time-Wait)

Let us go concept by concept.

---

## Concept 1: TCP Flow Control Using Sliding Window with Go-Back-N

### 📌 Concept Name
TCP Sliding Window Flow Control (Go-Back-N ARQ)

### 🧠 Simple Explanation

TCP uses a **sliding window** flow control mechanism based on the **Go-Back-N ARQ** (Automatic Repeat reQuest) principle.

Here is how it works step by step:

- The **sender** sends data to the **receiver**.
- The **receiver** has a **buffer** (a temporary storage space) of a fixed size (e.g., 4 KB).
- The receiver tells the sender how much space is available in its buffer using a value called the **window size** (also called the **advertised window**).
- The sender can only send as much data as the receiver's window allows. If the window becomes **0**, the sender is **blocked** — it cannot send any more data.
- When the receiver's application reads data from the buffer, space becomes free, and the receiver sends a new **acknowledgement** with an updated (non-zero) window size. Now the sender can resume sending.

**Go-Back-N principle:** If a timeout occurs (i.e., the sender does not get an acknowledgement in time), the sender **retransmits all the data** that is currently in its sender window.

### 🛠 Example from Transcript

Imagine the receiver buffer can hold **4 KB** of data.

1. **Step 1:** Sender sends **2 KB** of data (starting from sequence number 0).
   - Receiver gets 2 KB → buffer now has 2 KB used, 2 KB free.
   - Receiver sends ACK with **ACK number = 2048** and **window size = 2048** (2 KB free).

2. **Step 2:** Sender sends another **2 KB** (starting from sequence number 2048).
   - Receiver gets this 2 KB → buffer is now **full** (4 KB used, 0 KB free).
   - Receiver sends ACK with **ACK number = 4096** and **window size = 0**.
   - **Sender is now blocked** — it cannot send any more data because the window is 0.

3. **Step 3:** The receiver's application reads 2 KB from the buffer.
   - Buffer now has **2 KB free**.
   - Receiver sends ACK with **ACK number = 4096** and **window size = 2048**.
   - **Sender unblocks** and can now send up to 2 KB of new data.

4. **Step 4:** Sender sends **1 KB** (starting from sequence number 4096).
   - Receiver gets 1 KB → buffer has 1 KB free.
   - Receiver sends ACK with **ACK number = 5120** (4096 + 1024) and **window size = 1024**.

**Key point:** The sent data remains in the sender's buffer until the sender receives an acknowledgement. If the ACK is lost, the sender retransmits the entire data from its buffer (Go-Back-N).

### 🎯 Exam Important Points
- TCP uses **byte-level sequence numbers** (not packet numbers).
- The receiver controls the sender's rate using the **advertised window size**.
- When window = 0, the sender is **blocked**.
- Go-Back-N means: on timeout, retransmit **all** unacknowledged data.

### ⚠️ Common Confusions
- Sequence number 0 is used here **for simplicity only** — the actual initial sequence number is decided during the **3-way handshake** (connection establishment).
- Window size is in **bytes**, not packets.
- The sender stays blocked until it receives a **new ACK with window > 0**.

---

## Concept 2: The Problem of Small Segments (Telnet Example)

### 📌 Concept Name
Overhead Problem with Small Segments

### 🧠 Simple Explanation

Consider the **Telnet** application. Telnet is used to make a **remote connection** to a server and execute commands on it.

When you type a command like `ls` in telnet, every single **keystroke** (every character) is sent individually to the server over the network.

Now here is the problem:

- The TCP header is **20 bytes** long.
- Telnet sends only **1 byte** of data at a time.
- So, TCP creates a segment of **21 bytes** (20 bytes header + 1 byte data).
- Then the receiver also sends an **ACK** with a window update — that is another packet.

This results in **huge bandwidth wastage**! You are using 20 bytes of header to send just 1 byte of useful data. The overhead is enormous.

### 🎯 Exam Important Points
- TCP header = **20 bytes**.
- With Telnet, each keystroke = 1 byte data → 21 byte TCP segment.
- This creates a **very high overhead**.
- This problem motivates the need for **Delayed ACK** and **Nagle's Algorithm**.

---

## Concept 3: Delayed Acknowledgement

### 📌 Concept Name
Delayed Acknowledgement (Delayed ACK)

### 🧠 Simple Explanation

To solve the small-segment problem, TCP uses a technique called **Delayed Acknowledgement**.

The idea is simple:

- When the receiver gets a small piece of data, it does **NOT** send the ACK immediately.
- Instead, it **waits** for up to **500 milliseconds** (by default in TCP).
- The hope is that during this waiting time, more data packets will arrive.
- After waiting, the receiver sends a **single ACK** for all the data it received during that period.
- This way, the receiver avoids sending many small ACKs and reduces overhead.

**How does this help?** Since the receiver delays the ACK, the sender (which is waiting for the ACK before knowing the available buffer space) will not send many small segments in rapid succession.

### 🎯 Exam Important Points
- Delayed ACK waits for up to **500 ms** before sending acknowledgement.
- Purpose: reduce the number of small ACKs and small window update messages.
- The delay is at the **receiver** side.

### ⚠️ Common Confusions
- Delayed ACK does NOT stop the sender from sending — the sender can still send multiple short segments if it wants. Delayed ACK only delays the **receiver's response**.

---

## Concept 4: Nagle's Algorithm

### 📌 Concept Name
Nagle's Algorithm

### 🧠 Simple Explanation

While Delayed ACK works at the **receiver** side, **Nagle's Algorithm** works at the **sender** side to prevent sending many small segments.

The rule is:

1. When data comes to the sender in small pieces (e.g., 1 byte at a time from telnet), **send the first piece immediately**.
2. **Buffer all remaining pieces** (don't send them yet).
3. Wait until the **ACK for the first piece comes back**.
4. By the time the ACK arrives, hopefully the application has written more data to the buffer.
5. Now **combine all buffered data** into one larger segment and send it.

**Key idea:** At any given time, **only one small segment** is outstanding (unacknowledged) in the network. This prevents flooding the network with many tiny packets.

### 🛠 Example from Transcript
- Sender receives bytes A, B, C, D one by one from the application.
- Sender sends byte A immediately.
- Bytes B, C, D are buffered.
- When ACK for A arrives, sender combines B, C, D into one segment and sends it.

### 🎯 Exam Important Points
- Nagle's Algorithm is at the **sender** side.
- Rule: Send the first small piece; buffer the rest until ACK comes.
- Only **one small segment outstanding** in the network at a time.
- Reduces the number of tiny packets in the network.

### ⚠️ Common Confusions
- Nagle's algorithm **increases delay** because it waits for ACK before sending more data.
- It should **NOT** be used for **delay-sensitive applications** (like real-time applications).

---

## Concept 5: Problem When Nagle's Algorithm + Delayed ACK Are Used Together

### 📌 Concept Name
Starvation Due to Nagle's Algorithm + Delayed ACK Together

### 🧠 Simple Explanation

If you use **both** Nagle's Algorithm and Delayed ACK at the same time, a serious problem occurs:

- **Nagle's Algorithm (sender side):** The sender has sent one small segment and is **waiting for the ACK** before sending more.
- **Delayed ACK (receiver side):** The receiver has received the small segment but is **delaying the ACK** (waiting for more data before responding).

**Result:** The sender is waiting for the ACK, and the receiver is delaying the ACK. Both are waiting for each other! This leads to **starvation** and a **very slow response time** for the application.

**Conclusion:** You should **NOT** implement Delayed ACK and Nagle's Algorithm together.

### 🎯 Exam Important Points
- Nagle's Algorithm + Delayed ACK together → **starvation / deadlock-like situation**.
- Sender waits for ACK; receiver delays ACK → both wait → slow response.
- This combination must be **avoided**.

---

## Concept 6: Silly Window Syndrome (SWS)

### 📌 Concept Name
Silly Window Syndrome

### 🧠 Simple Explanation

This is another problem related to small segments, but this time the problem is at the **receiver** side.

**Scenario:**
- The sender has lots of data to send.
- The receiver has an interactive application that reads data very slowly — for example, **1 byte at a time**.
- The receiver buffer becomes **full** → receiver sends window = 0 → sender is **blocked**.
- The receiver application reads just **1 byte** → 1 byte becomes free in the buffer.
- The receiver immediately sends a window update: **window = 1 byte**.
- The sender sees this and sends a segment with just **1 byte of data**.
- That 1 byte fills the buffer again → window = 0 → sender blocked again.
- Receiver reads 1 byte again → sends window = 1 → sender sends 1 byte...

**This cycle repeats!** The sender keeps sending **tiny 1-byte segments** with 20-byte TCP headers each time. This is a huge waste of bandwidth.

This problem is called the **Silly Window Syndrome** because the window update is too small (silly) to be useful.

### 🎯 Exam Important Points
- Silly Window Syndrome happens when the **receiver** sends very small window updates (like 1 byte).
- The sender is forced to send tiny segments with large headers → huge overhead.
- It is a receiver-side problem (the receiving application reads data too slowly).

---

## Concept 7: Clark's Solution to Silly Window Syndrome

### 📌 Concept Name
Clark's Solution

### 🧠 Simple Explanation

To solve the Silly Window Syndrome, **Clark** proposed a simple rule:

**Do NOT send a window update for just 1 byte.** Instead, **wait** until **sufficient space** is available in the receiver buffer before sending the window update.

- "Sufficient space" depends on the TCP implementation — it could be a certain percentage of the total buffer size.
- Once enough space is free, only then does the receiver inform the sender with an updated window size.

This prevents the sender from being tempted to send many tiny segments.

### 🎯 Exam Important Points
- Clark's solution: delay the window update until **sufficient buffer space** is available.
- It is a **receiver-side** solution.
- It directly solves the Silly Window Syndrome.

---

## Concept 8: Nagle's Algorithm and Clark's Solution Are Complementary

### 📌 Concept Name
Complementary Nature of Nagle's Algorithm and Clark's Solution

### 🧠 Simple Explanation

Unlike the problematic combination of Nagle's Algorithm + Delayed ACK (which causes starvation), **Nagle's Algorithm + Clark's Solution work well together** — they are **complementary**.

Here is why:

| Problem | Solution | Side |
|---------|----------|------|
| Sending application delivers data to TCP 1 byte at a time | **Nagle's Algorithm** | Sender side |
| Receiving application reads data from TCP 1 byte at a time | **Clark's Solution** | Receiver side |

- Nagle prevents the **sender** from sending small segments.
- Clark prevents the **receiver** from advertising small windows.

Together, they handle the small-segment problem from **both sides** without causing starvation.

### ⚠️ Important Exception — PSH Flag
Even with Nagle's and Clark's, there can be some increased response time. For **real-time or delay-sensitive applications**, you can set the **PSH (Push) flag** in the TCP header. The PSH flag tells the sender to create and send a segment **immediately** without waiting for more data.

### 🎯 Exam Important Points
- Nagle's Algorithm and Clark's Solution are **complementary** (they work well together).
- Nagle's Algorithm and Delayed ACK are **NOT complementary** (they cause starvation).
- PSH flag bypasses these mechanisms for immediate data transfer.

---

## Concept 9: Handling Out-of-Order Segments and Duplicate ACKs (DUPACK)

### 📌 Concept Name
Out-of-Order Segments and Duplicate Acknowledgements (DUPACK)

### 🧠 Simple Explanation

Sometimes, segments arrive at the receiver **out of order** (not in the expected sequence). TCP handles this as follows:

1. TCP **buffers** (stores) the out-of-order segment in the receiver buffer.
2. TCP sends a **duplicate ACK (DUPACK)** — an acknowledgement with the **same ACK number** it sent before.

**Why duplicate?** Because TCP uses **cumulative acknowledgements**. The ACK number tells the sender: "I have received everything up to this byte, and I am expecting this byte next." If an out-of-order segment arrives, the receiver still expects the missing bytes, so it sends the same ACK number again.

### 🛠 Example from Transcript

- Receiver has received bytes 0, 1, 2. It sends **ACK = 3** (expecting byte 3 next).
- Byte 3 is **lost**, but bytes 4, 5, 6, 7 arrive.
- Receiver buffers bytes 4–7 but still expects byte 3.
- Receiver sends a **duplicate ACK with ACK = 3** (it still needs byte 3).
- This duplicate ACK triggers a **congestion control mechanism** (discussed in the next lecture).
- After a timeout, the sender retransmits **byte 3**.
- Receiver now has bytes 0 through 7 (complete).
- Receiver sends **ACK = 8** (a cumulative ACK acknowledging everything up to byte 7, expecting byte 8 next).

### 🎯 Exam Important Points
- TCP **buffers** out-of-order segments (does not discard them).
- TCP sends **duplicate ACKs** when out-of-order segments arrive.
- TCP uses **cumulative acknowledgements** — the ACK number indicates the **next expected byte**.
- Duplicate ACKs are important for triggering the **congestion control algorithm** (covered in the next lecture).

### ⚠️ Common Confusions
- DUPACK does NOT mean the receiver received duplicate data. It means the receiver is **repeating the same ACK number** because it did not receive the expected byte.

---

## Concept 10: TCP Retransmission Timeout (RTO)

### 📌 Concept Name
TCP Retransmission Timeout (RTO)

### 🧠 Simple Explanation

TCP uses a **timer** for each segment it sends. This timer is called the **Retransmission Timer**, and the timeout value is called **RTO (Retransmission Timeout)**.

**How it works:**
1. When the sender sends a segment, it **starts a timer**.
2. If the **ACK arrives before the timer expires** → the timer is stopped (everything is fine).
3. If the **timer expires before the ACK arrives** → the sender assumes the segment was **lost** and **retransmits** it.

Timeout also triggers the **TCP congestion control algorithm** (next lecture topic).

### 🎯 Exam Important Points
- RTO = Retransmission Timeout.
- Timer starts when a segment is sent.
- If ACK comes before timeout → timer stops.
- If timeout happens before ACK → segment is retransmitted.
- Timeout also triggers congestion control.

---

## Concept 11: Estimating RTT — Why It Is Difficult at the Transport Layer

### 📌 Concept Name
RTT Estimation Challenge at the Transport Layer

### 🧠 Simple Explanation

To set a good RTO value, you need to know the **Round Trip Time (RTT)** — the time it takes to send a segment and receive its ACK.

**Why not just use a fixed value?**

At the **data link layer**, two nodes are **directly connected** by a single link. The RTT variation is very low and predictable. A simple average gives a good estimate.

But at the **transport layer**, the sender and receiver are separated by the **entire internet** — many routers, many links, varying congestion levels. The RTT **varies significantly**. The variance is very high.

If you just take a simple average of RTT, you will get a poor estimate. The actual RTT might be very different from the average. If you set RTO based on a bad RTT estimate, you will get **spurious RTOs** (unnecessary retransmissions — the timer expires even though the segment was not actually lost).

**Solution:** Use a **dynamic algorithm** that constantly adapts the timeout based on continuous measurement of network performance.

### 🎯 Exam Important Points
- RTT at data link layer: low variance → easy to estimate.
- RTT at transport layer: **high variance** → difficult to estimate.
- Simple average of RTT is **not good enough** for the transport layer.
- Need a **dynamic algorithm** for RTO estimation.

---

## Concept 12: Jacobson's Algorithm and SRTT (Smoothed RTT)

### 📌 Concept Name
Jacobson's Algorithm / Smoothed Round Trip Time (SRTT) / EWMA

### 🧠 Simple Explanation

To estimate RTT dynamically, TCP uses **Jacobson's Algorithm** (proposed in 1988).

TCP maintains a variable called **SRTT (Smoothed Round Trip Time)**, which is the **best current estimate** of the round trip time.

**How SRTT is updated:**

When the sender sends a segment and receives the ACK, it measures the actual time taken. This measured value is called **R** (measured RTT).

Then, SRTT is updated using this formula:

> **SRTT = α × SRTT(old) + (1 − α) × R**

Where:
- **SRTT(old)** = previous estimate of RTT
- **R** = newly measured RTT
- **α (alpha)** = smoothing factor = **7/8** (as set by Jacobson for TCP)

This technique is called **Exponentially Weighted Moving Average (EWMA)**.

- **α = 7/8** means: you give **7/8 weight** (87.5%) to the old estimate and **1/8 weight** (12.5%) to the new measurement.
- This ensures the estimate changes **slowly and smoothly**, not jumping wildly with every new measurement.
- Old values are gradually "forgotten" over time.

### 🎯 Exam Important Points
- SRTT = Smoothed Round Trip Time (best estimate of RTT).
- Formula: **SRTT = α × SRTT_old + (1 − α) × R**
- α = **7/8** (Jacobson's value for TCP).
- This mechanism is called **EWMA (Exponentially Weighted Moving Average)**.
- α is the smoothing factor that controls how fast old values are forgotten.

### ⚠️ Common Confusions
- α = 7/8 means the **old estimate gets more weight**, so the SRTT changes slowly.
- R is the **measured RTT**, not the estimated one.

---

## Concept 13: Problem with SRTT Alone — Need for Variance (RTTVAR)

### 📌 Concept Name
RTO Estimation Using RTT Variance (RTTVAR)

### 🧠 Simple Explanation

Even with a good SRTT, setting the RTO is still a problem.

**Initial approach:** Early TCP used **RTO = 2 × SRTT**.

**Problem:** This fixed multiplier is **inflexible**. When the network load is high, the RTT fluctuates a lot (high variance). A constant multiplier fails to adapt to these fluctuations, leading to **spurious RTOs**.

**Solution:** Consider the **variance** (deviation) of RTT along with the average.

TCP maintains another variable called **RTTVAR (RTT Variance)**:

> **RTTVAR = β × RTTVAR(old) + (1 − β) × |SRTT − R|**

Where:
- **β (beta)** = **3/4**
- **|SRTT − R|** = absolute difference between the current estimate and the measured RTT (this gives the current deviation/variance)

Now, the RTO is calculated as:

> **RTO = SRTT + 4 × RTTVAR**

**Why multiply by 4?** The transcript mentions that the value 4 was chosen somewhat arbitrarily by Jacobson. The reason 4 was specifically used is that **4 = 2²**, which allows the computation to be done using **binary shift operations** (faster and more lightweight than multiplication).

### 🎯 Exam Important Points
- **RTTVAR formula:** RTTVAR = β × RTTVAR_old + (1 − β) × |SRTT − R|
- β = **3/4**
- **RTO formula:** RTO = SRTT + 4 × RTTVAR
- The factor 4 is used because 4 = 2², allowing efficient binary shift computation.
- This approach adapts to **high network load** where RTT variance increases.

### ⚠️ Common Confusions
- Do NOT confuse α and β: **α = 7/8** is for SRTT, **β = 3/4** is for RTTVAR.
- RTO is NOT simply 2 × SRTT (that was the old, inflexible approach). The modern approach includes variance.

---

## Concept 14: Karn's Algorithm

### 📌 Concept Name
Karn's Algorithm

### 🧠 Simple Explanation

There is a problem with measuring RTT when a segment is **lost and retransmitted**.

**The problem:**
1. Sender sends a segment and starts a timer.
2. The segment is **lost** (never reaches the receiver).
3. Timer expires (timeout) → sender **retransmits** the same segment.
4. Now the sender gets an ACK.

**Question:** Should the sender use this time measurement to update SRTT? **NO!** Because the measured time includes the time for the original (lost) segment AND the retransmission. This is **not a true RTT measurement**.

**Karn's Algorithm says two things:**

1. **Do NOT update RTT estimates** (SRTT and RTTVAR) for any segment that has been **retransmitted**. Simply skip the RTT measurement for retransmitted segments.

2. **Double the RTO** on each successive retransmission until the segment gets through for the first time (this is called **exponential backoff**):
   - First timeout → retransmit → set RTO = **2 × RTO**
   - Second timeout → retransmit → set RTO = **4 × RTO**
   - Keep doubling until ACK is received.
   - Once ACK is received, **reset RTO** to the normal calculated value.

### 🎯 Exam Important Points
- Karn's Algorithm: **Do NOT update RTT estimates on retransmitted segments.**
- On each successive retransmission, **double the RTO** (exponential backoff).
- Once ACK is received successfully, reset RTO to the normal value.

### ⚠️ Common Confusions
- Karn's Algorithm does NOT say "never retransmit." It says "do not use the time from retransmitted segments to update RTT estimates."
- The doubling is **temporary** — once the segment gets through, RTO goes back to normal.

---

## Concept 15: Other TCP Timers

### 📌 Concept Name
Persistent Timer, Keepalive Timer, Time-Wait Timer

### 🧠 Simple Explanation

Apart from the retransmission timer, TCP has three other important timers:

### (a) Persistent Timer
- **Purpose:** Avoid **deadlock** when the receiver advertises window = 0.
- **Problem:** If the receiver sends window = 0, the sender stops sending. Later, the receiver sends a new ACK with window > 0, but this ACK is **lost**. Now the sender is still waiting, and the receiver thinks the sender knows about the new window. **Deadlock!**
- **Solution:** When window = 0, the sender starts a **persistent timer**. When this timer expires, the sender sends a small **probe packet** to the receiver. The receiver responds with the current window size. This breaks the deadlock.

### (b) Keepalive Timer
- **Purpose:** Close a connection that has been **idle for a long time**.
- If no data is being sent on a TCP connection for a long duration, the keepalive timer goes off and the connection is **closed**.

### (c) Time-Wait Timer (from Connection Closure)
- **Purpose:** Wait before fully closing a connection.
- Duration: generally **2 × packet lifetime** (twice the maximum time a packet can live in the network).
- This was discussed in the connection closure lecture (previous lecture on TCP connection establishment).

### 🎯 Exam Important Points
- **Persistent timer:** prevents deadlock when window = 0 → sends a **probe packet** after timer expires.
- **Keepalive timer:** closes **idle connections** after long inactivity.
- **Time-Wait timer:** waits for **2 × packet lifetime** before fully closing a connection.

---

## Summary Table of Key Concepts

| Concept | Purpose | Side | Key Detail |
|---------|---------|------|------------|
| Sliding Window (Go-Back-N) | Flow control | Both | Receiver advertises window; sender retransmits all on timeout |
| Delayed ACK | Reduce small ACK overhead | Receiver | Waits up to 500 ms before sending ACK |
| Nagle's Algorithm | Reduce small segment overhead | Sender | Buffer data until ACK for first piece arrives |
| Delayed ACK + Nagle's | PROBLEM | Both | Causes starvation — avoid this combination |
| Silly Window Syndrome | Problem with tiny window updates | Receiver | Receiver sends window = 1 byte → sender sends tiny segments |
| Clark's Solution | Fix Silly Window Syndrome | Receiver | Wait for sufficient space before sending window update |
| Nagle's + Clark's | Complementary | Both | Work well together, no starvation |
| PSH Flag | Immediate data transfer | Sender | Bypasses Nagle's and Clark's for real-time apps |
| Duplicate ACK (DUPACK) | Handle out-of-order segments | Receiver | Sends same ACK number again; triggers congestion control |
| RTO | Retransmission timeout | Sender | Timer-based; retransmit if ACK not received in time |
| SRTT (EWMA) | Estimate RTT | Sender | α = 7/8 |
| RTTVAR | RTT variance | Sender | β = 3/4 |
| RTO Formula | Calculate timeout | Sender | RTO = SRTT + 4 × RTTVAR |
| Karn's Algorithm | Handle retransmitted segments | Sender | Don't update RTT on retransmissions; double RTO each time |
| Persistent Timer | Avoid deadlock at window = 0 | Sender | Sends probe packet |
| Keepalive Timer | Close idle connections | Both | Connection closed after long inactivity |
| Time-Wait Timer | Safe connection closure | Both | Wait 2 × packet lifetime |

---

## Key Formulas to Remember

| Formula | Values |
|---------|--------|
| **SRTT = α × SRTT_old + (1 − α) × R** | α = 7/8 |
| **RTTVAR = β × RTTVAR_old + (1 − β) × \|SRTT − R\|** | β = 3/4 |
| **RTO = SRTT + 4 × RTTVAR** | Factor 4 (= 2²) for binary shift efficiency |
| **Karn's: RTO doubling** | RTO → 2×RTO → 4×RTO ... until ACK received |

---

## 10 MCQs Based on Lecture 21

### Q1. TCP uses which type of flow control mechanism?

(A) Stop-and-Wait ARQ
(B) Selective Repeat ARQ
(C) Sliding Window with Go-Back-N ARQ
(D) Pure ALOHA

**Answer: (C)**
**Explanation:** The transcript clearly states that TCP uses a sliding window flow control algorithm with the Go-Back-N ARQ principle. On timeout, it retransmits all the data inside the sender window.

---

### Q2. In the Telnet example, what is the size of a TCP segment carrying just 1 byte of application data?

(A) 1 byte
(B) 20 bytes
(C) 21 bytes
(D) 40 bytes

**Answer: (C)**
**Explanation:** The TCP header is 20 bytes. With 1 byte of data from Telnet, the total TCP segment size is 20 + 1 = 21 bytes.

---

### Q3. What is the default delay in the Delayed Acknowledgement mechanism?

(A) 100 milliseconds
(B) 200 milliseconds
(C) 500 milliseconds
(D) 1000 milliseconds

**Answer: (C)**
**Explanation:** The transcript states that in delayed acknowledgement, the receiver delays ACK and window updates for up to 500 milliseconds.

---

### Q4. What happens when Nagle's Algorithm and Delayed Acknowledgement are implemented together?

(A) They complement each other perfectly
(B) Data transfer becomes faster
(C) Starvation / very slow response time occurs
(D) The connection is automatically closed

**Answer: (C)**
**Explanation:** Nagle's Algorithm makes the sender wait for ACK, and Delayed ACK makes the receiver delay the ACK. Together, they cause both sides to wait for each other, resulting in starvation and slow response.

---

### Q5. The Silly Window Syndrome occurs because:

(A) The sender sends very large segments
(B) The receiver advertises very small window sizes (like 1 byte)
(C) The sender does not implement Go-Back-N
(D) The connection was not established properly

**Answer: (B)**
**Explanation:** Silly Window Syndrome occurs when the receiver's application reads data very slowly (like 1 byte at a time), causing the receiver to send very small window updates. This forces the sender to send tiny segments with high overhead.

---

### Q6. Clark's solution to the Silly Window Syndrome says:

(A) Send window update immediately for every byte freed
(B) Wait until sufficient space is available before sending a window update
(C) Close the connection when the window becomes 0
(D) Double the window size on every ACK

**Answer: (B)**
**Explanation:** Clark's solution says: do not send a window update for a tiny space like 1 byte. Wait until sufficient buffer space is available, then send the window update.

---

### Q7. In Jacobson's Algorithm, the value of smoothing factor α for SRTT in TCP is:

(A) 1/2
(B) 3/4
(C) 7/8
(D) 15/16

**Answer: (C)**
**Explanation:** The transcript states that Jacobson set α = 7/8 for the SRTT estimation in TCP using the EWMA formula.

---

### Q8. The RTO in TCP (Jacobson's approach) is calculated as:

(A) RTO = 2 × SRTT
(B) RTO = SRTT + RTTVAR
(C) RTO = SRTT + 4 × RTTVAR
(D) RTO = 4 × SRTT

**Answer: (C)**
**Explanation:** The transcript gives the formula RTO = SRTT + 4 × RTTVAR. The factor 4 was chosen because 4 = 2², allowing efficient binary shift computation.

---

### Q9. Karn's Algorithm states that:

(A) Always update RTT estimates, even for retransmitted segments
(B) Do not update RTT estimates for retransmitted segments, and double the RTO on each successive retransmission
(C) Halve the RTO on each retransmission
(D) Use a fixed RTO value regardless of network conditions

**Answer: (B)**
**Explanation:** Karn's Algorithm says: (1) Do not update RTT estimates on retransmitted segments. (2) Double the RTO on each successive retransmission until the segment gets through. Once ACK is received, reset RTO to the normal value.

---

### Q10. The Persistent Timer in TCP is used to:

(A) Close idle connections
(B) Wait before closing a connection fully
(C) Avoid deadlock when the receiver advertises window = 0
(D) Measure the Round Trip Time

**Answer: (C)**
**Explanation:** The persistent timer prevents deadlock when the receiver's window = 0. When the timer expires, the sender sends a probe packet to the receiver to get the updated window size. This avoids the situation where both sender and receiver are waiting for each other.

---

*End of Lecture 21 Notes*
