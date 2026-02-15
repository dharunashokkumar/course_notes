# Lecture 22: Transmission Control Protocol – IV (Congestion Control)

**Course:** Computer Networks and Internet Protocol
**Instructor:** Prof. Sandip Chakraborthy, IIT Kharagpur
**Topic:** TCP Congestion Control Algorithms

---

## What This Lecture Covers

This lecture is fully about **congestion control in TCP**. In the previous lecture, we studied flow control. Now, we learn how TCP handles **congestion** in the network. This is one of the most important topics in TCP and very likely to appear in the NPTEL exam.

The lecture covers these concepts step by step:

1. Basic idea of congestion control in TCP (AIMD + Window)
2. Congestion Window and Sending Rate
3. Combining Flow Control and Congestion Control
4. History — The Congestion Collapse of 1986
5. Packet Loss as a Signal for Congestion
6. ACK Clocking
7. Why Additive Increase alone is slow
8. TCP Slow Start (Exponential Increase)
9. Slow Start Threshold (ssthresh)
10. Congestion Avoidance (Additive Increase Phase)
11. Two ways TCP detects congestion: RTO and Duplicate ACKs
12. Fast Retransmission
13. TCP Tahoe
14. TCP Reno and Fast Recovery
15. Other TCP Variants (brief mention)

---

## Concept 1: Basic Congestion Control in TCP — AIMD with Window

### 📌 Concept Name
**Additive Increase Multiplicative Decrease (AIMD) with Window-Based Control**

### 🧠 Simple Explanation

TCP's congestion control is built on the **AIMD principle** using a **window-based mechanism**.

Here is what this means in simple words:

- **Additive Increase** means: When the network is fine (no congestion), TCP slowly increases its sending speed — it adds a little bit more data each time.
- **Multiplicative Decrease** means: When TCP detects congestion (a packet is lost), it **cuts its sending speed sharply** (usually to half).
- **Window-based** means: TCP does not directly control its "speed." Instead, it controls how many bytes it can have "in flight" in the network at any time. This is done through a **congestion window (cwnd)**.

The goal of AIMD is to achieve two things at the same time:

- **Efficiency (Maximize Capacity):** Use as much of the available network bandwidth as possible.
- **Fairness:** When multiple TCP connections share the same bottleneck link, each connection should get a fair share of the bandwidth.

Earlier in the course, we learned that AIMD provides **max-min fairness** along with maximizing link utilization. This is better than AIAD (additive increase, additive decrease) or MIMD (multiplicative increase, multiplicative decrease), which either don't converge to fairness or don't maximize capacity.

TCP uses **packet loss** as the signal that congestion has occurred.

### 🎯 Exam Important Points
- TCP congestion control = AIMD + window-based control.
- TCP treats **packet loss** as the indicator of congestion.
- AIMD achieves both **max-min fairness** and **capacity maximization**.
- AIMD is better than AIAD and MIMD for distributed congestion control.

### ⚠️ Common Confusions
- "Additive increase" does NOT mean TCP increases speed very fast. It means TCP increases **slowly and linearly** (one unit at a time per RTT).
- "Multiplicative decrease" means a **sharp cut** (typically by half), not a small reduction.
- AIMD is about **rate control**, not about retransmission directly.

---

## Concept 2: Congestion Window (cwnd) and Sending Rate

### 📌 Concept Name
**Congestion Window and Its Relationship to Sending Rate**

### 🧠 Simple Explanation

The **congestion window (cwnd)** is the number of bytes that the TCP sender is allowed to have in the network at any instant — meaning, bytes that have been sent but not yet acknowledged.

The **sending rate** of TCP can be approximated as:

> **Sending Rate = cwnd / RTT**

Where **RTT** is the Round Trip Time — the time for a packet to go from sender to receiver and for the acknowledgement to come back.

So, if cwnd is large, you are sending more data per RTT — your speed is high. If cwnd is small, your speed is low.

### 🛠 Real-world Example
Think of cwnd as the number of trucks you can have on a highway at the same time. RTT is the time one truck takes to go and come back. If you can put 10 trucks on the road and each takes 1 hour round trip, your throughput is 10 trucks/hour.

### 🎯 Exam Important Points
- **Sending Rate = cwnd / RTT** — this formula is important.
- cwnd controls how much data TCP pushes into the network.
- A larger cwnd → faster sending rate. A smaller cwnd → slower sending rate.

### ⚠️ Common Confusions
- cwnd is NOT the same as the receiver window (rwnd). cwnd is about what the **network** can handle. rwnd is about what the **receiver** can handle.

---

## Concept 3: Combining Flow Control and Congestion Control

### 📌 Concept Name
**Sender Window = min(cwnd, rwnd)**

### 🧠 Simple Explanation

TCP has two separate controls:

1. **Flow Control** → managed by the **receiver window (rwnd)**. The receiver tells the sender: "I have this much buffer space — don't send more than this." This prevents the receiver's buffer from overflowing.
2. **Congestion Control** → managed by the **congestion window (cwnd)**. This prevents the network from being overloaded.

The actual sending window of TCP is the **minimum** of these two:

> **Sender Window = min(cwnd, rwnd)**

Why minimum? Because:
- If the network can handle 100 packets but the receiver can only handle 50, you should send only 50 (otherwise receiver overflows).
- If the receiver can handle 100 packets but the network can only handle 30, you should send only 30 (otherwise network gets congested).

So you always pick the smaller value — the bottleneck.

### 🎯 Exam Important Points
- **Sender Window = min(cwnd, rwnd)** — very important formula for the exam.
- cwnd comes from congestion control. rwnd comes from flow control.
- The actual sending rate is limited by whichever is smaller.

### ⚠️ Common Confusions
- Students sometimes forget that TCP uses BOTH windows together. It is not "either cwnd or rwnd" — it is always the minimum of both.

---

## Concept 4: History — The Congestion Collapse of 1986

### 📌 Concept Name
**Congestion Collapse and Van Jacobson's Contribution**

### 🧠 Simple Explanation

In **1986**, as the internet was growing in popularity, the first major **congestion collapse** happened. This was a prolonged period during which the useful throughput of the network (called **goodput**) dropped by more than a factor of 100. The network was basically jammed — lots of data was being sent, but very little was actually getting through.

**Van Jacobson** investigated this problem and designed the first TCP congestion control algorithm. His initial proposal came around **1988**.

The big challenge Jacobson faced was: he had to design the congestion control algorithm **without making major changes to the existing TCP protocol**. Why? Because TCP was already widely deployed. If you design a completely new protocol, you have to update every machine on the internet — that's a **backward compatibility** problem.

So Jacobson found a clever solution: he used **packet loss** as the signal for congestion, and he used the existing **timeout mechanism** and **acknowledgement system** to implement the algorithm — no new protocol fields were needed.

### 🎯 Exam Important Points
- Congestion collapse happened in **1986**.
- **Van Jacobson** designed TCP's congestion control algorithm, proposed around **1988**.
- The goodput dropped by more than a **factor of 100** during congestion collapse.
- Jacobson's key challenge: implement congestion control **without changing TCP protocol structure** (backward compatibility).

### ⚠️ Common Confusions
- "Congestion collapse" does not mean the network physically broke. It means the **useful throughput** (goodput) dropped dramatically even though lots of packets were being sent.

---

## Concept 5: Packet Loss as a Signal for Congestion

### 📌 Concept Name
**Why Packet Loss Indicates Congestion**

### 🧠 Simple Explanation

Jacobson observed that in a **wired network** (which was the dominant type of network in the 1980s), the communication links are guided media (copper cables, fiber optics). In wired links, the **link layer** handles errors from the channel, so you almost never lose a packet because of channel noise. The channels are essentially **lossless**.

So, if a packet is lost in a wired network, it can only mean one thing: **a buffer at some intermediate router has overflowed**. A buffer overflows because too many packets arrived at that router — which means there is **congestion**.

Therefore: **Packet loss → Buffer overflow → Congestion**.

TCP uses **timeout** to detect packet loss. When a timeout happens, TCP concludes that a packet was lost and therefore the network is congested.

### 🛠 Real-world Example
Imagine a highway toll booth. Cars on the highway don't just "disappear." If a car doesn't arrive, it means the parking area before the toll booth got too full and cars couldn't enter — that's congestion at the toll booth.

### 🎯 Exam Important Points
- In wired networks, channels are essentially lossless.
- Packet loss in wired networks → buffer overflow at a router → congestion.
- TCP uses timeout to detect packet loss, which signals congestion.
- This assumption works well for wired networks but has issues in **wireless networks** (where packets can be lost due to the channel, not congestion). The transcript notes that wireless was in a very nascent stage when Jacobson designed this.

### ⚠️ Common Confusions
- This logic **does NOT work perfectly for wireless networks**, where packets can be lost due to interference, not congestion. However, the original TCP congestion control was designed for wired networks.

---

## Concept 6: ACK Clocking

### 📌 Concept Name
**Using Acknowledgements as Clocks to Adjust Congestion Window**

### 🧠 Simple Explanation

This is one of the cleverest ideas in TCP congestion control.

Consider a network path: Sender → Fast Link → Router → Slow Link (bottleneck) → Receiver.

The sender sends packets in a burst over the fast link. But at the slow link (bottleneck), packets take more time to pass through. The receiver sends acknowledgements (ACKs) back. The **rate** at which ACKs arrive at the sender reflects the rate of the **slowest (bottleneck) link** in the path.

So, the sender does not need to know the speed of each link. It just monitors the **rate of incoming ACKs**. This ACK rate naturally tells the sender how fast the bottleneck link is processing packets.

TCP uses these acknowledgements as **"clocks"** to trigger congestion window adjustments. Every time an ACK arrives, TCP adjusts its cwnd.

### 🎯 Exam Important Points
- ACKs arrive at the sender at the rate of the **slowest link** in the end-to-end path.
- TCP uses ACKs as clocks to trigger congestion window adjustments.
- This was an important observation by **Van Jacobson**.
- The sender doesn't need explicit knowledge of link speeds — ACK timing provides this information implicitly.

### ⚠️ Common Confusions
- ACK clocking is not a separate algorithm. It is the **mechanism** by which TCP knows when to adjust cwnd — it adjusts cwnd based on when ACKs arrive.

---

## Concept 7: Why Pure Additive Increase Is Too Slow

### 📌 Concept Name
**Problem with Starting from Additive Increase Only**

### 🧠 Simple Explanation

Suppose TCP starts with cwnd = 1 packet, and uses pure additive increase (add 1 packet to cwnd per RTT):

- RTT 1: cwnd = 1 → send 1 packet
- RTT 2: cwnd = 2 → send 2 packets
- RTT 3: cwnd = 3 → send 3 packets
- ...and so on.

Now let's see the problem with a real example from the transcript:

- **Link speed:** 10 Mbps
- **RTT:** 100 milliseconds
- **Bandwidth Delay Product (BDP):** 10 Mbps × 100 ms = 1 Megabit = 1,000,000 bits
- **Packet size:** 1250 bytes = 10,000 bits
- **Number of packets to fill the pipe:** 1,000,000 / 10,000 = **100 packets**

If cwnd starts at 1 and increases by 1 per RTT, it takes **100 RTTs** to reach the full capacity. With 100 ms per RTT, that is **10 seconds**.

This is way too slow! Most HTTP connections (web page loads) finish much faster than 10 seconds. By the time TCP reaches full capacity, the connection might already be closed.

This motivates the need for a **faster initial increase** — which is called **Slow Start**.

### 🎯 Exam Important Points
- BDP = Bandwidth × RTT (this is the ideal cwnd to fully utilize the link).
- With pure additive increase from cwnd=1, it takes BDP/packet_size RTTs to reach full capacity.
- In the example: 10 Mbps link, 100 ms RTT, 1250 byte packets → takes 100 RTTs (≈10 seconds).
- This is why **slow start** is needed.

### ⚠️ Common Confusions
- BDP is in **bits**, not bytes. Be careful with unit conversion in exams.
- The "100 RTTs" calculation: BDP = 1 Mbit, each packet = 10,000 bits, so 1,000,000/10,000 = 100 packets needed.

---

## Concept 8: TCP Slow Start (Exponential Increase)

### 📌 Concept Name
**TCP Slow Start**

### 🧠 Simple Explanation

Despite its name, **slow start does NOT mean slow**. The name comes from the fact that you **start from a slow (small) rate** — but then you increase **very fast (exponentially)**.

In Slow Start:

- Start with cwnd = 1 packet (1 MSS — Maximum Segment Size).
- For **each ACK** received, increase cwnd by 1 MSS.
- This means cwnd **doubles every RTT**.

Here is how it works step by step:

| RTT | cwnd at start | Packets Sent | ACKs Received | cwnd after ACKs |
|-----|--------------|-------------|---------------|-----------------|
| 1   | 1            | 1           | 1             | 2               |
| 2   | 2            | 2           | 2             | 4               |
| 3   | 4            | 4           | 4             | 8               |
| 4   | 8            | 8           | 8             | 16              |

So: 1 → 2 → 4 → 8 → 16 → ...

This is **exponential growth** (doubling). It is much faster than additive increase (1 → 2 → 3 → 4 → 5...).

The rule is: **Every acknowledgement allows two more segments to be sent.** Or equivalently, for each segment acknowledged before the retransmission timer goes off, the sender adds 1 MSS to cwnd.

### 🎯 Exam Important Points
- Slow Start: cwnd **doubles every RTT** (exponential increase).
- For each ACK received, cwnd increases by 1 MSS.
- The name "slow start" is ironic — it starts slow but grows fast.
- Slow Start is **NOT** AIMD. It is the initial phase before AIMD kicks in.

### ⚠️ Common Confusions
- "Slow start" does NOT mean the rate increases slowly. It means you START from a slow rate and then increase exponentially.
- In slow start, cwnd increases by 1 for **each ACK**, not 1 per RTT. Since you get multiple ACKs per RTT (equal to cwnd), the cwnd effectively doubles per RTT.

---

## Concept 9: Slow Start Threshold (ssthresh)

### 📌 Concept Name
**Slow Start Threshold (ssthresh)**

### 🧠 Simple Explanation

If TCP kept doing exponential increase forever, it would quickly send too many packets and cause congestion. So, TCP needs to know **when to stop the exponential increase and switch to the slower additive increase**.

This is controlled by a value called the **slow start threshold (ssthresh)**.

How it works:

1. **Initially**, ssthresh is set to a very high value (like the BDP or the maximum window size).
2. TCP starts with **Slow Start** (exponential increase).
3. When cwnd reaches ssthresh, TCP **switches to Additive Increase** (called Congestion Avoidance).
4. When a packet loss is detected (by Retransmission Timeout): **ssthresh is set to half of the current cwnd**, and cwnd is reset to 1 MSS. Then slow start begins again from 1 MSS.

The process looks like this:

```
                    ↗ Exponential (Slow Start)
cwnd               /
    ↗ Exponential / ← ssthresh → Linear (Additive Increase / Congestion Avoidance)
   /             /                    ↗↗↗
  /    ssthresh /                   /
 /     ↓      /        Loss!      /
/____________/___________________/______→ Time
1 MSS        new ssthresh = cwnd/2
```

After a loss:
- New ssthresh = half of cwnd at the time of loss.
- cwnd resets to 1 MSS.
- Slow start runs again until new ssthresh is reached.
- Then additive increase continues.

### 🎯 Exam Important Points
- ssthresh controls the transition from slow start to congestion avoidance.
- Initially ssthresh = high value (BDP or arbitrary maximum).
- On packet loss (RTO): **ssthresh = cwnd / 2**, and **cwnd = 1 MSS**.
- Below ssthresh → Slow Start (exponential). Above ssthresh → Congestion Avoidance (additive/linear).

### ⚠️ Common Confusions
- ssthresh is **NOT fixed**. It changes every time a loss is detected.
- After a loss, ssthresh becomes half of the **current cwnd** (not half of the old ssthresh).

---

## Concept 10: Congestion Avoidance (Additive Increase Phase)

### 📌 Concept Name
**Congestion Avoidance — Additive Increase after ssthresh**

### 🧠 Simple Explanation

Once cwnd crosses the ssthresh, TCP switches from exponential increase (slow start) to **linear increase** (additive increase). This phase is called **Congestion Avoidance**.

In Congestion Avoidance:
- cwnd increases by approximately **1 MSS per RTT** (not per ACK like in slow start).
- The formula used is:

> **cwnd = cwnd + (MSS × MSS) / cwnd**

This formula ensures that over one RTT (during which cwnd/MSS ACKs are received), the total increase in cwnd is approximately 1 MSS.

The step-by-step behavior:
- cwnd = 1 → gets 1 ACK → cwnd = 2
- cwnd = 2 → gets 2 ACKs → cwnd = 3
- cwnd = 3 → gets 3 ACKs → cwnd = 4
- cwnd = 4 → gets 4 ACKs → cwnd = 5

This is the "additive increase" part of AIMD — a slow, careful increase to probe for available bandwidth without causing congestion.

### 🎯 Exam Important Points
- Congestion Avoidance = Additive Increase phase.
- cwnd increases by approximately **1 MSS per RTT** (linear growth).
- Formula: **cwnd = cwnd + (MSS × MSS) / cwnd** (applied per ACK).
- This phase starts only **after** cwnd crosses ssthresh.

### ⚠️ Common Confusions
- In slow start, cwnd doubles per RTT (exponential). In congestion avoidance, cwnd increases by 1 MSS per RTT (linear). Don't mix them up.
- The formula `cwnd + MSS²/cwnd` gives a **partial increment per ACK**, not a full MSS per ACK.

---

## Concept 11: Two Ways TCP Detects Congestion

### 📌 Concept Name
**Retransmission Timeout (RTO) vs. Triple Duplicate ACK**

### 🧠 Simple Explanation

TCP uses **two methods** to detect that congestion has occurred:

**Method 1: Retransmission Timeout (RTO)**
- If the sender does not receive an ACK within the timeout period, it assumes the packet was lost.
- This is a **sure indication** of congestion, but it takes a long time (you wait for the full timeout).
- This indicates **severe congestion** — the network is completely choked.

**Method 2: Triple Duplicate ACK (3 Duplicate ACKs)**
- The receiver sends a duplicate ACK when it receives an **out-of-order segment**.
- Example: Receiver gets packets 1, 2, 3, then packet 4 is lost, but packets 5, 6, 7 arrive. For each of packets 5, 6, 7, the receiver sends ACK for packet 3 (duplicate ACKs).
- TCP assumes that receiving **3 duplicate ACKs** means the packet has been lost.
- This is **faster** than waiting for a full timeout.
- This indicates **mild congestion** — some packets are still flowing (the receiver is getting packets 5, 6, 7, just not packet 4).

Why 3 duplicate ACKs? The transcript says this number is **arbitrary** — Jacobson chose 3, and there is no specific mathematical logic behind this choice.

**Key advantage of duplicate ACKs:** Because TCP uses **cumulative acknowledgement**, the duplicate ACK tells you exactly **which packet was lost** — it is the very next packet after the acknowledged sequence number.

### 🎯 Exam Important Points
- **RTO**: Sure indication of congestion, but slow. Indicates severe congestion.
- **3 Duplicate ACKs**: Faster detection, indicates mild congestion (some packets still flowing).
- 3 duplicate ACKs → you can identify which packet was lost (cumulative ACK tells you).
- The choice of "3" is arbitrary (no specific mathematical reason).
- Both methods trigger congestion control, but TCP responds **differently** to each.

### ⚠️ Common Confusions
- "3 duplicate ACKs" means 3 ACKs **in addition to** the original — so you receive the same ACK number **4 times total** (original + 3 duplicates). Some references count it differently, but the transcript says "three duplicate acknowledgements."
- RTO ≠ Duplicate ACK. They trigger **different responses** in TCP Reno.

---

## Concept 12: Fast Retransmission

### 📌 Concept Name
**Fast Retransmit**

### 🧠 Simple Explanation

**Fast Retransmit** means: when TCP receives 3 duplicate ACKs, it **immediately retransmits the lost packet** without waiting for the retransmission timeout.

Why is it called "fast"? Because normally, TCP would wait for the full timeout period before retransmitting. But with fast retransmit, it retransmits as soon as it gets 3 duplicate ACKs — which is much faster.

How does TCP know which packet to retransmit? Because of **cumulative ACKs**:
- If receiver sent ACK for packet 3 three times (duplicate), it means packet 4 is missing.
- So the sender retransmits packet 4 immediately.

This takes about **one RTT** to complete (send the retransmission and get back the ACK).

### 🎯 Exam Important Points
- Fast retransmit = retransmit lost packet immediately after 3 duplicate ACKs.
- No need to wait for timeout.
- Possible because cumulative ACK identifies the lost packet.

---

## Concept 13: TCP Tahoe

### 📌 Concept Name
**TCP Tahoe**

### 🧠 Simple Explanation

TCP Tahoe is one of the earliest versions of TCP congestion control. It uses **fast retransmit** but treats both RTO and triple duplicate ACK **the same way**.

When TCP Tahoe detects congestion (by either RTO or 3 duplicate ACKs):

1. **Retransmit the lost packet** (fast retransmit if detected by duplicate ACKs).
2. **Set ssthresh = cwnd / 2** (half of current congestion window).
3. **Set cwnd = 1 MSS** (reset to the minimum).
4. **Start Slow Start again** from cwnd = 1 MSS.

So in TCP Tahoe, every time congestion is detected, TCP goes all the way back to 1 MSS and starts over with slow start.

Example from transcript:
- Say cwnd reaches 40 and congestion is detected.
- ssthresh is set to 20 (half of 40).
- cwnd is set to 1 MSS.
- Slow start runs from 1 until cwnd reaches 20 (the new ssthresh).
- Then additive increase (congestion avoidance) begins.

### 🎯 Exam Important Points
- TCP Tahoe: On any congestion → ssthresh = cwnd/2, cwnd = 1 MSS, restart slow start.
- Uses fast retransmit on 3 duplicate ACKs.
- Does NOT differentiate between RTO and duplicate ACK responses (both go to cwnd = 1).
- **Disadvantage**: Going back to cwnd = 1 is very wasteful when congestion is mild.

### ⚠️ Common Confusions
- TCP Tahoe always resets cwnd to 1 MSS — regardless of whether congestion was detected by timeout or by duplicate ACKs.

---

## Concept 14: TCP Reno and Fast Recovery

### 📌 Concept Name
**TCP Reno — Fast Recovery Algorithm**

### 🧠 Simple Explanation

TCP Reno improved upon TCP Tahoe by adding a key observation:

**If congestion is detected by 3 duplicate ACKs (not by timeout), it means some packets are still flowing in the network.** The network is not completely jammed — the receiver is still getting some packets (that's why it can send duplicate ACKs). So the congestion is **not severe**.

In this case, resetting cwnd to 1 MSS (like Tahoe does) is too aggressive. TCP Reno introduces **Fast Recovery** to handle this better.

### How Fast Recovery Works (Step by Step):

1. When 3 duplicate ACKs are received:
   - Set **ssthresh = cwnd / 2**.
   - **Retransmit the lost packet** (fast retransmit).
   - Set **cwnd = ssthresh + 3 MSS**.
     - Why +3? Because you received 3 duplicate ACKs, which means the receiver has already received 3 more packets (out of order). So those 3 packets have "left" the network, and you can send 3 new ones.
2. For **each additional duplicate ACK** received: increase cwnd by 1 MSS and send a new packet if allowed.
3. When a **new (non-duplicate) ACK** arrives: This means the retransmitted packet was received. The receiver sends a cumulative ACK for all the packets it has now received in order. At this point:
   - Set **cwnd = ssthresh** (the value set in step 1).
   - **Exit fast recovery**.
   - Continue with **Congestion Avoidance** (additive increase).

### TCP Reno: How it responds differently to the two types of congestion

| Congestion Signal | ssthresh | cwnd | Next Phase |
|---|---|---|---|
| **3 Duplicate ACKs** | cwnd / 2 | ssthresh + 3 → Fast Recovery | Congestion Avoidance (after new ACK) |
| **Retransmission Timeout (RTO)** | cwnd / 2 | **1 MSS** | Slow Start (back to the beginning) |

Why the difference?
- **RTO** = severe congestion (no packets flowing) → go back to 1 MSS, start slow start.
- **3 Dup ACKs** = mild congestion (some packets still flowing) → use fast recovery, avoid slow start.

### The Big Advantage of TCP Reno over TCP Tahoe:
In TCP Reno, when congestion is detected by duplicate ACKs, the sender does NOT go back to cwnd = 1. Instead, it starts additive increase from ssthresh (which is cwnd/2). This means it **reaches the operating point much faster** than Tahoe, which always restarts from 1 MSS.

### 🎯 Exam Important Points
- TCP Reno adds **Fast Recovery** on top of TCP Tahoe.
- On 3 duplicate ACKs: ssthresh = cwnd/2, cwnd = ssthresh + 3, then fast recovery.
- On RTO: ssthresh = cwnd/2, cwnd = 1 MSS, then slow start (same as Tahoe).
- Fast recovery: for each additional duplicate ACK, cwnd += 1 MSS.
- When new ACK arrives: cwnd = ssthresh, exit fast recovery, continue with congestion avoidance.
- **+3** because 3 duplicate ACKs mean 3 packets already received out of order.

### ⚠️ Common Confusions
- **TCP Tahoe vs TCP Reno**: The KEY difference is what happens on 3 duplicate ACKs. Tahoe → cwnd = 1 MSS (slow start). Reno → cwnd = ssthresh + 3 (fast recovery, then congestion avoidance).
- Both Tahoe and Reno behave the **same** on RTO (both go to cwnd = 1 MSS).
- Fast Recovery is entered ONLY on duplicate ACKs, NEVER on RTO.

---

## Concept 15: The Three Phases of TCP Congestion Control

### 📌 Concept Name
**Three Phases: Slow Start → Congestion Avoidance → Fast Recovery/Retransmit**

### 🧠 Simple Explanation

The transcript summarizes that the basic notion of TCP congestion control has three phases:

1. **Slow Start** — Exponential increase of cwnd from 1 MSS until ssthresh is reached.
2. **Congestion Avoidance** — Linear (additive) increase of cwnd after ssthresh is crossed.
3. **Fast Retransmit and Fast Recovery** — When 3 duplicate ACKs are received, retransmit the lost packet immediately, and recover without going back to slow start.

These three phases together form the core of TCP congestion control.

### 🎯 Exam Important Points
- The three phases: Slow Start, Congestion Avoidance, Fast Retransmit/Recovery.
- These are the foundation of TCP congestion control — everything else builds on these.

---

## Concept 16: Other TCP Variants (Brief Mention)

### 📌 Concept Name
**TCP New Reno, TCP SACK, and Other Variants**

### 🧠 Simple Explanation

The transcript briefly mentions that after TCP Reno, many other variants of TCP were developed:

- **TCP New Reno** — an improvement over TCP Reno (details not given in this transcript).
- **TCP SACK (Selective Acknowledgement)** — Instead of cumulative ACK (like Go-Back-N), TCP SACK uses **selective repeat ARQ**. The receiver explicitly tells the sender which specific packets were lost, and the sender retransmits only those lost packets, not the entire window.
- The original (normal) TCP uses **Go-Back-N** style flow control, where cumulative ACKs are used.

The transcript says these variants incorporate further optimizations, and if you are interested, you can refer to the relevant RFCs.

The transcript also mentions that **more than 90% of internet traffic uses TCP**, making it the most widely deployed transport layer protocol. However, some applications use **UDP** (which does not support reliability, flow control, or congestion control). The next lecture will cover UDP.

### 🎯 Exam Important Points
- TCP SACK uses **selective repeat ARQ** (explicit indication of lost packets).
- Normal TCP uses **Go-Back-N** principle (cumulative ACKs).
- More than **90%** of internet traffic uses TCP.
- UDP does not support reliability, flow control, or congestion control.

### ⚠️ Common Confusions
- TCP SACK is different from normal TCP — it changes how acknowledgements work (selective vs cumulative).

---

## Complete Summary Table

| Phase | cwnd Behavior | When Active |
|---|---|---|
| **Slow Start** | Doubles every RTT (exponential) | cwnd < ssthresh |
| **Congestion Avoidance** | Increases by ~1 MSS per RTT (linear) | cwnd ≥ ssthresh |
| **Fast Retransmit** | Retransmit lost packet immediately | On 3 duplicate ACKs |
| **Fast Recovery** (Reno only) | cwnd = ssthresh+3, then +1 per dup ACK | After fast retransmit, until new ACK |

| Event | TCP Tahoe | TCP Reno |
|---|---|---|
| **3 Duplicate ACKs** | ssthresh = cwnd/2, cwnd = 1 MSS, slow start | ssthresh = cwnd/2, cwnd = ssthresh+3, fast recovery → congestion avoidance |
| **Retransmission Timeout** | ssthresh = cwnd/2, cwnd = 1 MSS, slow start | ssthresh = cwnd/2, cwnd = 1 MSS, slow start |

---

## 📝 10 NPTEL-Style MCQs from Lecture 22

---

### Q1. What principle does TCP congestion control use?

(A) Multiplicative Increase Multiplicative Decrease (MIMD)
(B) Additive Increase Additive Decrease (AIAD)
(C) Additive Increase Multiplicative Decrease (AIMD)
(D) Multiplicative Increase Additive Decrease (MIAD)

**Answer: (C)**
**Explanation:** The transcript clearly states that TCP's congestion control is based on AIMD — additive increase when no congestion, multiplicative decrease on detecting congestion. AIMD achieves both max-min fairness and capacity maximization.

---

### Q2. The sending rate of TCP is approximated as:

(A) cwnd × RTT
(B) RTT / cwnd
(C) cwnd / RTT
(D) cwnd + RTT

**Answer: (C)**
**Explanation:** The transcript states that sending rate = congestion window (cwnd) divided by Round Trip Time (RTT).

---

### Q3. The sender window in TCP is:

(A) Equal to the congestion window only
(B) Equal to the receiver window only
(C) Maximum of congestion window and receiver window
(D) Minimum of congestion window and receiver window

**Answer: (D)**
**Explanation:** The transcript states that the sender window should be the minimum of the congestion window (network supported rate) and the receiver window (receiver supported rate).

---

### Q4. The congestion collapse of 1986 caused the goodput to drop by more than a factor of:

(A) 2
(B) 10
(C) 50
(D) 100

**Answer: (D)**
**Explanation:** The transcript says the congestion collapse was a prolonged period during which the goodput dropped significantly, more than a factor of 100.

---

### Q5. In TCP Slow Start, the congestion window:

(A) Increases by 1 MSS per RTT (linear)
(B) Doubles every RTT (exponential)
(C) Remains constant
(D) Decreases by half every RTT

**Answer: (B)**
**Explanation:** In slow start, for each ACK received, cwnd increases by 1 MSS. Since the number of ACKs per RTT equals the current cwnd (in packets), the cwnd effectively doubles every RTT — this is exponential growth.

---

### Q6. When a retransmission timeout occurs, the slow start threshold (ssthresh) is set to:

(A) The current cwnd
(B) Twice the current cwnd
(C) Half of the current cwnd
(D) 1 MSS

**Answer: (C)**
**Explanation:** The transcript states that whenever a packet loss is detected by a retransmission timeout, ssthresh is set to half of the current congestion window.

---

### Q7. In TCP Tahoe, when 3 duplicate ACKs are received, cwnd is set to:

(A) ssthresh + 3
(B) cwnd / 2
(C) 1 MSS
(D) ssthresh

**Answer: (C)**
**Explanation:** In TCP Tahoe, on detecting congestion (whether by RTO or 3 duplicate ACKs), cwnd is always set to 1 MSS and slow start begins again. There is no fast recovery in Tahoe.

---

### Q8. What is the key improvement of TCP Reno over TCP Tahoe?

(A) TCP Reno uses a larger initial cwnd
(B) TCP Reno uses Fast Recovery on 3 duplicate ACKs instead of resetting to 1 MSS
(C) TCP Reno eliminates the slow start phase entirely
(D) TCP Reno does not use ssthresh

**Answer: (B)**
**Explanation:** TCP Reno adds Fast Recovery. On 3 duplicate ACKs, instead of dropping cwnd to 1 MSS like Tahoe, Reno sets cwnd = ssthresh + 3 and continues with congestion avoidance after recovery. This avoids the costly slow start phase.

---

### Q9. In TCP Reno's Fast Recovery, cwnd is initially set to ssthresh + 3 because:

(A) 3 is the default MSS value
(B) 3 duplicate ACKs mean 3 packets have already left the network (received out of order by receiver)
(C) It takes 3 RTTs to recover
(D) The timeout is 3 times the RTT

**Answer: (B)**
**Explanation:** The transcript explains that the +3 is because 3 duplicate ACKs indicate the receiver has already received 3 additional packets (out of order). Those packets have left the network, so the sender can send 3 more.

---

### Q10. TCP SACK (Selective Acknowledgement) works on the principle of:

(A) Go-Back-N ARQ
(B) Stop-and-Wait ARQ
(C) Selective Repeat ARQ
(D) Pure ALOHA

**Answer: (C)**
**Explanation:** The transcript states that TCP SACK uses the selective repeat ARQ principle, where the receiver explicitly indicates which packets were lost, and only those are retransmitted. Normal TCP uses the Go-Back-N principle with cumulative acknowledgements.

---

## What Else Is in This Course (Upcoming)

The transcript mentions that the **next lecture (Lecture 23)** will cover the **UDP protocol** — which does not support reliability, flow control, or congestion control like TCP does.

---

*These notes are strictly based on the Lecture 22 transcript of the NPTEL course "Computer Networks and Internet Protocol" by Prof. Sandip Chakraborthy, IIT Kharagpur.*
