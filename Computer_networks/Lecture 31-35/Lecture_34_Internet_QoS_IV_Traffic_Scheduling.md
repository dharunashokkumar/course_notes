# Lecture 34: Internet QoS — IV (Traffic Scheduling)

**Course:** Computer Networks and Internet Protocol  
**Instructor:** Prof. Sandip Chakraborty, IIT Kharagpur  
**Topic:** Queuing, Traffic Scheduling, and Congestion Avoidance

---

## Concept 1: Recap — Basic QoS Architecture

📌 **Concept Name:** Basic QoS Architecture (Quick Recap)

🧠 **Simple Explanation:**

Before we learn traffic scheduling, let us remember the full QoS pipeline that was covered in earlier lectures. Whenever packets enter a network, they go through a series of steps to ensure quality of service:

**Step 1 — Admission Control:** The network first checks whether it can support the quality of service that a new flow is requesting. If yes, the flow is admitted into the network. If not, it is rejected.

**Step 2 — Classification and Marking:** Once a flow is admitted, individual packets are marked based on their QoS class. Think of it like putting colored labels on packets. For example, red packets might be VoIP (voice), green packets might be video, and blue packets might be FTP (file transfer).

**Step 3 — Traffic Policing and Shaping:** At intermediate routers, traffic policing and shaping mechanisms are applied to control the rate of traffic. This ensures that no flow sends more data than agreed.

**Step 4 — Traffic Scheduling:** This is the focus of today's lecture. After packets are marked with their priority, the router needs a strategy to decide which packet to send first. This is traffic scheduling. The scheduler at each intermediate router decides the order in which packets from different queues are transmitted to the output port.

🎯 **Exam Important Points:**
- QoS architecture has 4 stages: Admission Control → Classification & Marking → Traffic Policing/Shaping → Traffic Scheduling
- Traffic scheduling happens at intermediate routers
- Different marked packets get different levels of service

⚠️ **Common Confusion:** Students often confuse traffic policing with traffic scheduling. Policing controls the *rate* of traffic entering the network. Scheduling decides the *order* in which packets are transmitted from different queues at a router.

---

## Concept 2: Classification and Marking — Detailed Understanding

📌 **Concept Name:** Classification and Marking of Packets

🧠 **Simple Explanation:**

Classification and marking is the step where data packets from different applications are identified and labeled (marked) into different traffic classes.

Imagine you are using your smartphone. You might be running three apps at the same time: a VoIP call (like WhatsApp call), YouTube, and downloading a file via FTP. All three generate data packets that go into the same network.

Now, the network (starting from the first hop router like your base station) needs to understand which packets belong to which application. It does this by classifying the packets and marking them. For example:
- VoIP packets → marked as **high priority** (delay-sensitive)
- Video packets → marked as **medium priority** (bandwidth-hungry)
- FTP packets → marked as **low priority** (best effort)

This marking is based on the **Service Level Agreement (SLA)** — a contract between you and your service provider. The SLA tells the network what kind of service you are paying for.

🛠 **Real-world Example (from transcript):**

When you buy a data pack from Airtel or Vodafone, the pack might say: "100 minutes free calling, 100 SMS/day, 1.2 GB uplink, 5 GB downlink per day." This is a **user-level SLA**. An application-level SLA would specifically say: "Give priority to my VoIP data."

The transcript mentions that application-level SLAs are not very popular in India yet because VoIP services are not widely used. But once 5G networks become popular and VoIP services grow, we will see application-level SLAs.

🎯 **Exam Important Points:**
- Classification and marking divides packets into traffic classes
- SLA = Service Level Agreement (contract between user and service provider)
- Two types of SLA: user-level SLA and application-level SLA
- Three main traffic classes: high priority delay-sensitive (VoIP), high bandwidth (VoD/IPTV), best effort (HTTP/FTP)

⚠️ **Common Confusion:** SLA is not just about speed. It defines the *type* of service and quality guarantees for specific applications.

---

## Concept 3: Multi-Class Scheduling

📌 **Concept Name:** Multi-Class Scheduling

🧠 **Simple Explanation:**

Multi-class scheduling is the technique where different classes of traffic are treated differently at the router.

Instead of having one single queue for all packets, the router maintains **multiple queues** — one for each traffic class. After classification, packets are placed into their respective queues:

- **Traffic Class 1 (High Priority):** Delay-sensitive traffic like VoIP → The goal is to ensure **minimum queuing delay** for these packets.
- **Traffic Class 2 (Medium Priority):** Bandwidth-hungry traffic like Video on Demand → The goal is to ensure **sufficient bandwidth**.
- **Traffic Class 3 (Low Priority):** Best effort traffic like HTTP, FTP → No specific guarantee. Whatever bandwidth is available, use it.

Each queue is treated with a different scheduling strategy. This way, the router can provide the right QoS to each class of traffic.

🛠 **Real-world Example (from transcript):**

Think of a flow coming from the network. The classifier looks at it and says: "Flow 3 is highest priority → goes to high priority queue. Flow 7 and Flow 2 are medium priority → go to medium priority queue. Flow 6, Flow 8, and Flow 1 are low priority → go to low priority queue." Then the scheduler runs over these individual queues and sends traffic based on the scheduling strategy.

🎯 **Exam Important Points:**
- Multi-class scheduling = multiple queues, each treated differently
- Traffic Class 1 → minimum queuing delay
- Traffic Class 2 → sufficient bandwidth
- Traffic Class 3 → best effort (no guarantee)
- Queuing delay is the dominant component of delay in networks

⚠️ **Common Confusion:** Multi-class scheduling is not about one scheduling algorithm. It is a framework where different queuing strategies can be applied to different queues.

---

## Concept 4: Priority Queuing — Non-Preemptive

📌 **Concept Name:** Non-Preemptive Priority Queuing

🧠 **Simple Explanation:**

In priority queuing, packets are placed into queues of different priority levels: High, Medium, and Low.

**The basic rule is:** Always serve the highest priority queue first. Only when the high priority queue becomes empty, move to the medium priority queue. Only when the medium priority queue becomes empty, move to the low priority queue.

In **non-preemptive** priority queuing, the scheduler works in a round-robin fashion across the priority levels:

1. Serve ALL packets from the high priority queue
2. When it becomes empty, serve ALL packets from the medium priority queue
3. When it becomes empty, serve ALL packets from the low priority queue
4. Then go back to the high priority queue

The key word is "non-preemptive" — meaning once the scheduler starts serving a queue, it is NOT interrupted (not broken in between). It finishes serving that queue completely before moving to the next.

🎯 **Exam Important Points:**
- Non-preemptive = scheduler is not interrupted while serving a queue
- Serves queues in round-robin fashion across priority levels
- High priority queue served first → then medium → then low → repeat

---

## Concept 5: Priority Queuing — Preemptive

📌 **Concept Name:** Preemptive Priority Queuing

🧠 **Simple Explanation:**

Preemptive priority queuing is similar to non-preemptive, but with one important difference: the scheduler **CAN be interrupted**.

Here is how it works:

1. The scheduler serves all packets from the high priority queue.
2. When the high priority queue becomes empty, it moves to the medium priority queue.
3. **While serving the medium priority queue**, if a new packet arrives in the high priority queue, the scheduler **immediately stops** (preempts) service at the medium queue and goes back to serve the high priority queue.
4. Once the high priority queue is empty again, it returns to the medium priority queue.
5. Same rule applies when serving the low priority queue — if any packet arrives in the high or medium queue, the scheduler preempts the low priority queue immediately.

🛠 **Real-world Example (from transcript):**

Think of the VIP gate at an airport. VIPs are served immediately. Even if regular passengers are being processed, when a VIP arrives, the VIP is served first. The regular passengers have to wait. Similarly, high priority packets (like network control packets) are served immediately without waiting.

**Advantage:** Very low delay and low jitter for high priority packets.

**Disadvantage:** The low priority queue may get **stuck** (starved). If high priority packets keep arriving continuously, the low priority packets never get served. This is called **starvation**.

🎯 **Exam Important Points:**
- Preemptive = scheduler CAN be interrupted when a higher priority packet arrives
- Ensures very low delay for high priority traffic
- **Problem:** Low priority queue may experience starvation
- Network control packets are examples of very high priority packets — delay in them can affect the entire network
- VIP gate analogy: VIPs (high priority) are served immediately, regular passengers (low priority) wait

⚠️ **Common Confusion:** Students confuse preemptive and non-preemptive. The key difference is: Can the scheduler be interrupted? Preemptive = YES, Non-preemptive = NO.

📝 **Possible NPTEL-style Question:**
"In preemptive priority scheduling, what happens when a high priority packet arrives while the scheduler is serving the low priority queue?"  
→ Answer: The scheduler immediately preempts service at the low priority queue and serves the high priority packet.

---

## Concept 6: Custom Queuing

📌 **Concept Name:** Custom Queuing

🧠 **Simple Explanation:**

Custom queuing is a scheduling strategy where you assign **different queue sizes** to different traffic classes. The scheduler uses a simple **round-robin** approach — it takes one packet from each queue in turn.

Here is the key idea: The queue sizes are different for different classes. For example (normalized to 1):
- Queue 1 (high priority): size = 0.3
- Queue 2 (medium priority): size = 0.2
- Queue 3 (low priority or bandwidth-hungry): size = 0.5

Now, during **peak hours** (when the network is heavily loaded and all queues are full):
- The scheduler goes round-robin: one packet from Queue 1, one from Queue 2, one from Queue 3, repeat.
- But because Queue 3 is the largest (0.5), it can hold more packets. When it gets full, new packets for Queue 3 are dropped.
- Effectively, Queue 1 gets **30%** of the total capacity, Queue 2 gets **20%**, and Queue 3 gets **50%**.

This is because in peak hours, all queues are always full. The queue size directly determines how much capacity each class gets.

**Important note from transcript:** QoS matters only during peak hours. During non-peak hours, the network has sufficient capacity and everyone gets served within their time bound. The problem starts when the network is heavily loaded.

🛠 **Real-world Example (from transcript):**

At an airport during non-peak hours (say 2 PM), there are very few passengers. You can go through any gate quickly — nobody cares about VIP or non-VIP. But during peak hours, when there are huge numbers of passengers, you need proper scheduling and priority management. Custom queuing works the same way — the queue sizes matter when the network is congested.

🎯 **Exam Important Points:**
- Custom queuing = different queue sizes + round-robin scheduling
- Queue sizes determine the proportion of bandwidth each class gets
- Provides **guaranteed bandwidth** to different classes
- QoS matters mainly during peak hours / congestion
- Useful for applications needing guaranteed bandwidth (like video)

⚠️ **Common Confusion:** Custom queuing does NOT give priority to one class over another. It gives a guaranteed **proportion** of bandwidth based on queue sizes. This is different from priority queuing where one class is always served first.

---

## Concept 7: Weighted Fair Queuing (WFQ)

📌 **Concept Name:** Weighted Fair Queuing (WFQ)

🧠 **Simple Explanation:**

Weighted Fair Queuing is a scheduling strategy that ensures **fairness** among different traffic classes, even when packet sizes are different.

In the earlier methods (priority queuing, custom queuing), we assumed fixed packet sizes. But in reality, different applications send packets of different sizes. For example:
- Blue packets might be 1 unit each
- Red packets might be 4 units each
- Green packets might be 2 units each

Now, if we want fairness — meaning each class should get an equal amount of bandwidth — we cannot just send one packet from each queue (because their sizes are different).

**What WFQ does:**
- It sends **4 blue packets** (4 × 1 = 4 units)
- It sends **1 red packet** (1 × 4 = 4 units)
- It sends **2 green packets** (2 × 2 = 4 units)

This way, all three classes get exactly **4 units** of bandwidth each. That is fairness!

Queues can also be assigned **weights**, so fairness is proportional to the weights. If one class has weight 2 and another has weight 1, the first class gets twice the bandwidth.

🎯 **Exam Important Points:**
- WFQ ensures fairness among flows even with variable packet sizes
- It calculates how many packets to send from each queue based on packet size and weight
- Queues can have different weights → fairness is proportional to weight
- WFQ is useful when you need fair bandwidth distribution

⚠️ **Common Confusion:** Fair does NOT mean equal number of packets. It means equal (or proportional) **bandwidth** in terms of total data transmitted.

---

## Concept 8: Multilevel Queue Scheduling

📌 **Concept Name:** Multilevel Queue Scheduling (Combining Strategies)

🧠 **Simple Explanation:**

Sometimes, one scheduling strategy is not enough. You might need both priority and fairness at the same time. In that case, you can combine multiple scheduling strategies using **multilevel queue scheduling**.

Here is how it works:

**Level 1 — Priority Queuing:**
After classification, packets are put into different priority classes: Priority 1, Priority 2, Priority 3. The scheduler serves Priority 1 first, then Priority 2, then Priority 3.

**Level 2 — Weighted Fair Queuing (within each priority class):**
Within Priority 1, there might be packets of different sizes. So, instead of just picking one packet at a time, we apply Weighted Fair Queuing inside Priority 1 to ensure fairness among the different-sized packets.

So the architecture becomes:
- **First level** ensures priority → higher priority classes served first
- **Second level** ensures fairness → within each priority class, different flows get fair bandwidth

🎯 **Exam Important Points:**
- Multilevel scheduling = combining two or more scheduling strategies
- First level: Priority scheduling (priority between classes)
- Second level: Weighted Fair Queuing (fairness within each class)
- This supports BOTH priority AND fairness simultaneously

⚠️ **Common Confusion:** Multilevel scheduling is NOT a separate algorithm. It is a way of combining existing algorithms (like priority + WFQ) in a layered fashion.

---

## Concept 9: Congestion Avoidance vs. Congestion Control

📌 **Concept Name:** Congestion Avoidance vs. TCP Congestion Control

🧠 **Simple Explanation:**

This is a very important distinction for the exam.

**TCP Congestion Control (what we learned earlier):**
- TCP **reacts** to congestion AFTER it happens
- TCP detects congestion through packet loss (3 duplicate ACKs or timeout)
- On detecting congestion, TCP reduces the sending rate
- This is a **reactive** approach — congestion already occurred, now fix it

**Congestion Avoidance (new concept in this lecture):**
- Congestion avoidance tries to **prevent** congestion BEFORE it happens
- It works at the network layer (at routers), not at the transport layer
- It monitors queue lengths and drops packets early if congestion seems likely
- This is a **proactive** approach — detect the signs of congestion and take action early

**Key Question from transcript:** "If congestion avoidance is there at the network layer, do we still need congestion control at the transport layer?"

**Answer: YES, we need both!**

Here is why: Congestion avoidance is class-based. It protects high priority traffic (like VoIP) from congestion. But low priority traffic (like FTP) can still experience congestion. When FTP gets into congestion, TCP's congestion control is needed to make FTP come out of congestion.

Similarly, the reverse is also true: If only TCP congestion control exists (without congestion avoidance), then even high priority traffic like VoIP would get affected by congestion. So we need congestion avoidance to protect VoIP.

**Bottom line:** Both congestion avoidance AND congestion control are needed together in the internet to support QoS.

🎯 **Exam Important Points:**
- Congestion Control (TCP) = reactive, after congestion occurs, reduces rate
- Congestion Avoidance = proactive, prevents congestion before it occurs
- Congestion avoidance works at network layer (routers), congestion control at transport layer (TCP)
- Both are needed together — congestion avoidance protects high priority traffic, TCP congestion control handles low priority traffic when congestion occurs
- Congestion avoidance is class-based

⚠️ **Common Confusion:** Students often think that congestion avoidance can replace TCP congestion control. It cannot! They serve different purposes and work at different layers.

---

## Concept 10: Elastic Traffic vs. Inelastic Traffic

📌 **Concept Name:** Elastic and Inelastic Traffic

🧠 **Simple Explanation:**

The internet carries traffic from many different applications. Broadly, all traffic falls into two categories:

**Elastic Traffic:**
- This is TCP-based traffic
- It has an "elastic" (flexible) nature — it can expand and contract its rate
- TCP uses AIMD (Additive Increase Multiplicative Decrease): When no congestion → increase rate. When congestion detected → decrease rate.
- This expand-contract behavior is what makes it "elastic"
- Examples: HTTP, FTP

**Inelastic Traffic:**
- This is UDP-based traffic
- It is smooth, controlled, or constant bit rate traffic
- It does NOT change its rate based on congestion — it keeps sending at the same rate
- Used for real-time applications like VoIP
- Real-time applications prefer UDP because TCP's congestion control introduces **jitter** (variation in delay)

**Why does TCP introduce jitter?**
When TCP increases rate → less delay. When TCP decreases rate → more delay. This variation in delay = jitter. For real-time voice/video, jitter is very bad. That is why real-time traffic prefers UDP with constant bit rate.

**Important clarification from transcript:** YouTube is NOT real-time traffic. YouTube Live is real-time, but standard YouTube is pre-recorded video that is streamed — it is not the same as real-time. Real-time traffic uses protocols like RTP (Real-time Transport Protocol) or RTSP (Real-time Streaming Protocol) over UDP.

🎯 **Exam Important Points:**
- Elastic traffic = TCP, flexible rate (AIMD), examples: HTTP, FTP
- Inelastic traffic = UDP, constant bit rate, examples: VoIP, real-time video
- TCP congestion control introduces jitter → bad for real-time applications
- YouTube is NOT real-time (YouTube Live is real-time)
- Real-time applications prefer UDP (RTP/RTSP protocols)

⚠️ **Common Confusion:** Many students think YouTube is real-time traffic. The transcript specifically warns: "Do not get confused with YouTube." Standard YouTube is pre-recorded and streamed, not real-time.

---

## Concept 11: Why Congestion Avoidance is Necessary — Elastic vs. Inelastic Problem

📌 **Concept Name:** Why Congestion Avoidance is Necessary for QoS

🧠 **Simple Explanation:**

This concept explains WHY we need congestion avoidance when elastic and inelastic traffic share the same network.

**The problem:** If you have both elastic traffic (TCP/FTP) and inelastic traffic (UDP/VoIP) on the same link, which traffic will dominate?

**Answer: Elastic traffic will dominate!**

Here is why:
1. Elastic traffic (TCP) keeps trying to **increase its bandwidth** (AIMD — additive increase phase)
2. As TCP increases its rate, it starts taking more and more bandwidth from the link
3. Inelastic traffic (UDP) has **no control over congestion** — it cannot fight back, it cannot reduce or increase its rate
4. As TCP grabs more bandwidth, inelastic traffic starts experiencing **significant packet loss**
5. This has an **adverse effect** on real-time applications like VoIP

So the problem is: TCP is aggressive (it keeps increasing rate), while UDP is passive (it sends at constant rate). TCP will eat up the bandwidth, and UDP traffic will suffer.

**Solution:** Congestion avoidance! We need congestion avoidance at the network layer to protect inelastic traffic from being overwhelmed by elastic traffic.

🎯 **Exam Important Points:**
- When elastic and inelastic traffic share a link, elastic traffic dominates
- TCP keeps increasing rate → takes bandwidth away from UDP
- Inelastic traffic has no congestion control mechanism → suffers packet loss
- This is why congestion avoidance is needed to protect high priority inelastic traffic
- Without congestion avoidance, real-time traffic quality degrades significantly

📝 **Possible NPTEL-style Question:**
"If elastic and inelastic traffic share the same link, which traffic will dominate and why?"
→ Elastic traffic will dominate because TCP's AIMD keeps increasing the rate, taking bandwidth from inelastic traffic which has no congestion control mechanism.

---

## Concept 12: Random Early Detection (RED) — Overview

📌 **Concept Name:** Random Early Detection (RED)

🧠 **Simple Explanation:**

RED is the algorithm used for **congestion avoidance** in the internet. Let us break down the name:

- **"Early Detection"** = We detect congestion early, before it actually happens, by observing the queue length at routers.
- **"Random"** = We randomly drop packets to avoid congestion. The randomness is important (we will see why).

**The basic principle of RED:**
To avoid congestion, the only option is to **drop packets** for certain applications that are sending too much traffic. But we do not drop all packets — we drop them with a certain **probability** that depends on:
1. The nature of the traffic (elastic or inelastic)
2. The QoS class of the traffic
3. The current congestion level (measured by queue length)

RED smooths out the drop probability across all flows depending on the congestion probability. If congestion probability is high, RED randomly drops packets **before** enqueuing them.

🎯 **Exam Important Points:**
- RED = Random Early Detection, used for congestion avoidance
- Two key terms: "Random" (random packet dropping) + "Early Detection" (detect congestion early via queue length)
- Drop probability is different for different traffic classes
- RED drops packets BEFORE enqueuing to prevent congestion

---

## Concept 13: RED — How It Works (Queue Length Thresholds)

📌 **Concept Name:** RED Mechanism — Min Threshold and Max Threshold

🧠 **Simple Explanation:**

RED works by observing the **average queue length** at a router. It uses two thresholds:

1. **Minimum Threshold (MinThresh):** The safe boundary. Below this, everything is fine.
2. **Maximum Threshold (MaxThresh):** The danger boundary. Above this, congestion is certain.

**The three zones:**

**Zone 1: Average queue length < MinThresh**
- You are in the **safe zone**
- Action: **Enqueue the packet** normally (no dropping)
- Everything is fine, no congestion expected

**Zone 2: MinThresh ≤ Average queue length ≤ MaxThresh**
- You are in the **danger zone** (approaching congestion)
- Action: Calculate a **packet drop probability**
- If the probability says drop → **drop the packet**
- If the probability says don't drop → **enqueue the packet**
- The closer you are to MaxThresh, the higher the drop probability

**Zone 3: Average queue length > MaxThresh**
- You are already **in the danger zone** (congestion is imminent)
- Action: **Drop the packet** (drop probability = 1, meaning drop ALL packets)

🛠 **Real-world Analogy (from transcript idea):**

Think of a queue at a ticket counter. When there are very few people (below MinThresh), everyone is let in. When the queue starts getting long (between MinThresh and MaxThresh), some people are randomly told "sorry, come back later" to prevent the queue from becoming too long. When the queue is completely full (above MaxThresh), nobody new is allowed in.

🎯 **Exam Important Points:**
- RED uses average queue length to detect congestion
- Two thresholds: MinThresh and MaxThresh
- Below MinThresh → enqueue (safe zone)
- Between MinThresh and MaxThresh → calculate drop probability, may drop or enqueue
- Above MaxThresh → drop all packets (drop probability = 1)
- Queue length is a reliable indicator of congestion

---

## Concept 14: RED — Packet Drop Probability Formula

📌 **Concept Name:** RED Drop Probability Calculation

🧠 **Simple Explanation:**

RED needs to calculate the probability of dropping a packet when the average queue length is between MinThresh and MaxThresh. Here is the formula:

### Formula:

**d(k) = Max_p × (k - MinThresh) / (MaxThresh - MinThresh)**

Where:
- **d(k)** = packet drop probability (what we want to calculate)
- **k** = current average queue length
- **Max_p** = maximum packet drop probability (a configured value)
- **MinThresh** = minimum queue length threshold
- **MaxThresh** = maximum queue length threshold

**Understanding the formula:**

When k = MinThresh → d(k) = Max_p × (0) / (MaxThresh - MinThresh) = **0** (no drop)

When k = MaxThresh → d(k) = Max_p × (MaxThresh - MinThresh) / (MaxThresh - MinThresh) = **Max_p** (maximum drop probability)

So the drop probability increases **linearly** from 0 to Max_p as the queue length grows from MinThresh to MaxThresh.

Beyond MaxThresh → drop probability = **1** (drop all packets)

### Graph behavior:
- From 0 to MinThresh: drop probability = 0 (flat line at 0)
- From MinThresh to MaxThresh: drop probability increases linearly from 0 to Max_p
- Beyond MaxThresh: drop probability = 1 (all packets dropped)

🎯 **Exam Important Points:**
- Formula: d(k) = Max_p × (k - MinThresh) / (MaxThresh - MinThresh)
- Drop probability increases LINEARLY between MinThresh and MaxThresh
- Below MinThresh: probability = 0
- At MaxThresh: probability = Max_p
- Above MaxThresh: probability = 1 (drop all)
- k = current average queue length
- Max_p = maximum drop probability (configured parameter)

---

## Concept 15: RED — Why Random Dropping is Important (Connection to TCP)

📌 **Concept Name:** Significance of Random Dropping in RED

🧠 **Simple Explanation:**

The word "Random" in RED is very important. Let us understand why.

**Connection to TCP congestion detection:**

Recall from earlier lectures that TCP detects congestion in two ways:
1. **3 duplicate ACKs** (3 consecutive packet losses)
2. **Timeout**

Now, in RED, packets are dropped **randomly** (not all at once). This random dropping has a special implication:

**When RED drops one packet randomly:**
- Only one packet is lost
- The sender gets a **single duplicate ACK** (not 3)
- TCP does NOT trigger its congestion control algorithm yet
- This is a gentle hint to the sender: "slow down a bit"

**When congestion increases gradually:**
- RED drops more packets (probability increases)
- Now the sender starts getting **3 duplicate ACKs** or experiences timeouts
- TCP triggers its congestion control algorithm (like reducing the window size)
- The system starts coming out of congestion

**Why not just drop all packets at once?**
If you drop all packets at once, TCP would immediately trigger congestion control for ALL flows simultaneously. This would cause a sudden and severe reduction in throughput for everyone — a phenomenon called **global synchronization**. Random dropping avoids this by gently nudging individual flows.

🎯 **Exam Important Points:**
- Random dropping prevents TCP from triggering congestion control too early
- Single random packet loss → single duplicate ACK → TCP does NOT trigger congestion control
- As congestion increases → more drops → 3 duplicate ACKs → TCP triggers congestion control
- Random dropping avoids global synchronization problem
- RED provides early, gradual congestion signals to TCP senders
- Both congestion avoidance (RED) and congestion control (TCP) work together

⚠️ **Common Confusion:** RED does not replace TCP congestion control. RED provides early hints. As load increases, eventually TCP congestion control kicks in. Both are needed together.

---

## Concept 16: Complete Picture — Congestion Avoidance + Congestion Control

📌 **Concept Name:** Why Both Congestion Avoidance and Congestion Control are Needed

🧠 **Simple Explanation:**

The transcript emphasizes that to support quality of service in the internet, we need BOTH:

1. **Congestion Avoidance (RED at network layer):** Detects early signs of congestion by monitoring queue length. Randomly drops packets to prevent congestion from happening. Protects high priority traffic.

2. **Congestion Control (TCP at transport layer):** Reacts when congestion actually occurs. Reduces the sending rate of flows that are causing congestion.

**How they work together:**
- As load increases gradually, RED starts dropping some packets (early warning)
- If load keeps increasing, RED drops more packets
- Eventually, TCP detects packet loss and triggers congestion control
- TCP reduces rate, which reduces the load, and the system stabilizes

Without congestion avoidance → even VoIP traffic gets into congestion.  
Without congestion control → once congestion happens, nothing brings the system back to normal.

**The transcript concludes** by saying: In the next class, they will look at two specific QoS architectures — **Integrated Service (IntServ)** and **Differentiated Service (DiffServ)**.

🎯 **Exam Important Points:**
- Both congestion avoidance and congestion control are necessary
- Congestion avoidance = proactive, at network layer
- Congestion control = reactive, at transport layer
- They complement each other — one prevents, the other fixes
- Next lecture will cover IntServ and DiffServ architectures

---

## Summary Table: All Scheduling Strategies

| Strategy | Key Idea | Best For | Problem |
|---|---|---|---|
| **Priority Queuing (Non-Preemptive)** | Serve highest priority queue first, no interruption | Strict priority needs | Low priority may wait long |
| **Priority Queuing (Preemptive)** | Serve highest priority first, CAN interrupt lower queues | Delay-sensitive traffic (VoIP, network control) | Low priority starvation |
| **Custom Queuing** | Different queue sizes + round-robin | Guaranteed bandwidth allocation | No strict priority |
| **Weighted Fair Queuing (WFQ)** | Fairness among flows with variable packet sizes | Fair bandwidth distribution | Complexity in calculation |
| **Multilevel Queue Scheduling** | Combine priority + WFQ | Need both priority AND fairness | More complex |

---

## Summary Table: Congestion Avoidance vs. Congestion Control

| Feature | Congestion Avoidance (RED) | Congestion Control (TCP) |
|---|---|---|
| **Layer** | Network layer (at routers) | Transport layer (at endpoints) |
| **Approach** | Proactive (prevent) | Reactive (fix after detection) |
| **Mechanism** | Monitor queue length, drop packets early | Detect packet loss, reduce rate |
| **Trigger** | Queue length exceeds threshold | 3 duplicate ACKs or timeout |
| **Scope** | Class-based (protects high priority) | Flow-based (per TCP connection) |

---

## Summary Table: Elastic vs. Inelastic Traffic

| Feature | Elastic Traffic | Inelastic Traffic |
|---|---|---|
| **Protocol** | TCP | UDP |
| **Rate behavior** | Flexible — increases/decreases (AIMD) | Constant bit rate |
| **Congestion control** | Yes (built into TCP) | No |
| **Jitter** | Introduces jitter (rate changes) | No jitter (constant rate) |
| **Example** | HTTP, FTP | VoIP, RTP, RTSP |
| **Dominance** | Dominates when sharing a link | Gets overwhelmed |

---

## 10 MCQs — Strictly from Lecture 34

### Q1. What is the correct order of stages in the basic QoS architecture?

(a) Traffic Scheduling → Classification → Admission Control → Traffic Policing  
(b) Admission Control → Classification and Marking → Traffic Policing → Traffic Scheduling  
(c) Classification → Admission Control → Traffic Scheduling → Traffic Policing  
(d) Traffic Policing → Admission Control → Classification → Traffic Scheduling

**Answer: (b)**  
**Explanation:** The transcript describes the order as: Admission Control → Classification and Marking → Traffic Policing/Shaping → Traffic Scheduling. The packets first need to be admitted, then classified and marked, then policed, and finally scheduled at the router.

---

### Q2. In preemptive priority scheduling, what happens when a high priority packet arrives while the scheduler is serving the low priority queue?

(a) The low priority packet continues being served until completion  
(b) The high priority packet waits until the low priority queue is empty  
(c) The scheduler immediately preempts the low priority queue and serves the high priority packet  
(d) Both packets are served simultaneously

**Answer: (c)**  
**Explanation:** In preemptive scheduling, the scheduler can be interrupted. When a high priority packet arrives, the scheduler immediately stops serving the low priority queue and goes to serve the high priority queue.

---

### Q3. What problem can occur with preemptive priority scheduling?

(a) High priority packets experience high delay  
(b) Medium priority packets always get served first  
(c) Low priority queue may get stuck (starvation)  
(d) All queues get equal bandwidth

**Answer: (c)**  
**Explanation:** The transcript states that in preemptive service, the low priority queue may get stuck because high priority and medium priority packets keep arriving, and the scheduler never gets a chance to serve low priority packets. This is called starvation.

---

### Q4. In Custom Queuing with queue sizes 0.3, 0.2, and 0.5 (normalized to 1), what percentage of capacity does the third queue get during peak hours?

(a) 30%  
(b) 20%  
(c) 50%  
(d) 33%

**Answer: (c)**  
**Explanation:** The transcript explains that during peak hours when all queues are full, the queue sizes determine the proportion of capacity. Queue 3 with size 0.5 gets 50% of the total capacity because the round-robin scheduler serves equal turns but the larger queue holds more packets.

---

### Q5. What does Weighted Fair Queuing (WFQ) ensure?

(a) Higher priority packets are always served first  
(b) All queues get the same number of packets  
(c) Fairness among flows by ensuring equal or proportional bandwidth regardless of packet size  
(d) Guaranteed minimum delay for all packets

**Answer: (c)**  
**Explanation:** WFQ ensures fairness among different traffic classes even when packet sizes vary. It calculates how many packets to send from each queue so that each class gets equal (or proportionally weighted) bandwidth in terms of total data, not just number of packets.

---

### Q6. How is congestion avoidance different from TCP congestion control?

(a) Congestion avoidance works at the transport layer  
(b) TCP congestion control prevents congestion before it happens  
(c) Congestion avoidance prevents congestion proactively; TCP congestion control reacts after congestion occurs  
(d) There is no difference; they are the same thing

**Answer: (c)**  
**Explanation:** The transcript clearly distinguishes: congestion avoidance is proactive (prevents congestion before it happens by monitoring queue lengths), while TCP congestion control is reactive (detects congestion through packet loss and then reduces the sending rate).

---

### Q7. Why do real-time applications prefer inelastic (UDP) traffic over elastic (TCP) traffic?

(a) TCP provides more bandwidth  
(b) UDP has built-in congestion control  
(c) TCP's congestion control introduces jitter, which is bad for real-time applications  
(d) UDP guarantees delivery of all packets

**Answer: (c)**  
**Explanation:** The transcript explains that TCP's elastic behavior (increasing and decreasing rate via AIMD) introduces jitter — variation in delay. When rate increases, delay decreases; when rate drops, delay increases. This jitter is harmful for real-time voice/video, so they prefer UDP with constant bit rate.

---

### Q8. In RED, when the average queue length is between MinThresh and MaxThresh, what happens?

(a) All packets are dropped  
(b) All packets are enqueued  
(c) A drop probability is calculated; packets may be dropped or enqueued based on this probability  
(d) The queue is reset to zero

**Answer: (c)**  
**Explanation:** The transcript describes three zones in RED. When the average queue length is between MinThresh and MaxThresh, RED calculates a packet drop probability. Based on this probability, the packet is either dropped or enqueued.

---

### Q9. What is the formula for drop probability d(k) in RED?

(a) d(k) = Max_p × (MaxThresh - k) / (MaxThresh - MinThresh)  
(b) d(k) = Max_p × (k - MinThresh) / (MaxThresh - MinThresh)  
(c) d(k) = k / MaxThresh  
(d) d(k) = MinThresh / (MaxThresh × k)

**Answer: (b)**  
**Explanation:** The transcript gives the formula: d(k) = Max_p × (k - MinThresh) / (MaxThresh - MinThresh), where k is the current queue length, Max_p is the maximum drop probability, MinThresh and MaxThresh are the two thresholds.

---

### Q10. Why does RED use random packet dropping instead of dropping all packets at once?

(a) To save bandwidth  
(b) Because random dropping prevents TCP from triggering congestion control prematurely and avoids global synchronization  
(c) Because the router cannot drop all packets  
(d) To increase the queue length

**Answer: (b)**  
**Explanation:** The transcript explains that random dropping causes only single packet losses, which result in single duplicate ACKs — not enough to trigger TCP's congestion control (which requires 3 duplicate ACKs). This provides a gentle warning. As congestion grows, more drops occur, and TCP eventually triggers congestion control. Random dropping prevents all flows from reducing rate simultaneously (global synchronization).

---

*End of Lecture 34 — Complete Coverage*  
*Next Lecture (35): Integrated Service (IntServ) and Differentiated Service (DiffServ) architectures*
