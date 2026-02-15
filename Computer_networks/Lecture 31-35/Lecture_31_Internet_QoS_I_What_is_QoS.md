# Lecture 31 — Internet QoS – I (What is QoS)

**Course:** Computer Networks and Internet Protocol  
**Professor:** Prof. Sandip Chakraborty  
**Institute:** IIT Kharagpur, Department of Computer Science and Engineering

---

## Topics Covered in This Lecture

1. Introduction to Quality of Service (QoS)
2. Why QoS is Needed — Multimedia Streaming vs Normal File Transfer
3. Quality of Experience (QoE)
4. Service Level Agreement (SLA)
5. Revisiting Congestion — Does TCP Congestion Control Ensure No Congestion?
6. Four QoS Parameters — Bandwidth, Delay, Jitter, Loss
7. Network Bandwidth (QoS Parameter 1)
8. Delay — Three Components (QoS Parameter 2)
   - Transmission Delay
   - Propagation Delay
   - Queuing Delay
9. Queuing Delay Dominates
10. Jitter — Variance in End-to-End Delay (QoS Parameter 3)
11. Loss (QoS Parameter 4)
12. Loss in Wireless vs Wired Networks
13. Application-Level QoS Requirements Table
14. Formal Definition of QoS (Cisco Definition)
15. Ensuring QoS over a Packet Switching Network — Four Questions
16. Concept of Flow
17. Why QoS is Considered at the Network Layer
18. Application Classes Based on QoS

---

## Concept 1: Introduction to Quality of Service (QoS)

📌 **Concept Name:** What is Quality of Service?

🧠 **Simple Explanation:**

Quality of Service (QoS) is about how well the network delivers data, especially for applications that need smooth and continuous delivery like video streaming, voice calls, and live sessions.

Think about it this way — today we use mobile phones with thousands of apps. Many of these apps use multimedia: YouTube, Facebook Live, Hotstar, Netflix. All of these need a continuous stream of data. Unlike simply downloading a file (where you can wait and collect all the bits at the end), multimedia streaming requires data to arrive continuously and in real time while you are watching or listening.

When you watch a YouTube video, the data is being transmitted from the YouTube server to your browser client at the same time you are playing the video. If your network quality is poor or bandwidth is insufficient, you may see:
- **Degradation of video quality** (the video becomes blurry suddenly)
- **Re-buffering** (the video gets stuck, and you see a circular spinning icon trying to download data)

To avoid these problems, the network needs to maintain a certain level of quality of service. The client (like the YouTube player) expects a continuous stream of video data so it can render and play the video without quality drops or re-buffering.

🛠 **Real-world Example (from transcript):**
When you stream a YouTube video online, data comes continuously from the YouTube server to your browser. If your bandwidth drops, you see buffering or quality degradation. This is a QoS problem.

🎯 **Exam Important Points:**
- QoS is about maintaining a certain level of service quality for applications over the network
- Multimedia streaming data is fundamentally different from normal file transfer
- Normal file transfer: just transfer bytes, collect at the end, reconstruct the file
- Multimedia streaming: data must arrive continuously while you are playing/watching
- Providing QoS requires special services at the internet level

⚠️ **Common Confusions:**
- Do NOT confuse offline video downloading with streaming. Downloading is like file transfer (you download first, then play). Streaming means data arrives and plays simultaneously.
- QoS is NOT automatic — it requires dedicated resources and agreements with the network service provider.

---

## Concept 2: Quality of Experience (QoE)

📌 **Concept Name:** Quality of Experience

🧠 **Simple Explanation:**

Quality of Experience (QoE) is the user's perception of how good a particular application's service is. For example, when you watch a video on YouTube — how good do YOU think the video quality is? That feeling or perception is the Quality of Experience.

QoE is different from QoS. QoS is about network-level parameters (bandwidth, delay, etc.), while QoE is about what the user actually feels about the application quality.

To get good QoE, the network needs to provide certain QoS services. You may have to pay more to your network service provider and sign an agreement saying you need certain types of video quality. The provider should then deliver that level of QoS.

🎯 **Exam Important Points:**
- QoE = user's perceived quality of a particular application
- QoS parameters at network level directly affect QoE
- Better QoS → Better QoE
- Providing good QoE requires a service level agreement with the provider

---

## Concept 3: Service Level Agreement (SLA)

📌 **Concept Name:** Service Level Agreement

🧠 **Simple Explanation:**

To get good quality for your streaming or voice applications, you need to have an agreement with your network service provider. This is the Service Level Agreement (SLA). You agree with the provider that you require certain types of QoS, and the provider promises to deliver that level of service.

If your current network bandwidth subscription does not provide that kind of SLA, you will experience quality degradation. You need to pay more and subscribe to a plan that guarantees the QoS you need.

🎯 **Exam Important Points:**
- SLA is an agreement between user and network service provider for a certain level of QoS
- Without proper SLA, you cannot expect guaranteed QoS
- You will never get perfect QoS unless you have a dedicated leased line with the entire bandwidth for yourself

⚠️ **Common Confusion:**
- Just because you have a network connection does not mean you have QoS guarantees. QoS needs explicit agreements and dedicated resources.

---

## Concept 4: Revisiting Congestion — Does TCP Congestion Control Ensure No Congestion?

📌 **Concept Name:** TCP Congestion Control and QoS

🧠 **Simple Explanation:**

This is a very important question the transcript raises: **Does TCP congestion control ensure that there will be NO congestion in the network?**

The answer is: **NO, it does NOT.**

Here is why: TCP congestion control works in a reactive way, not a preventive way. What TCP does is:
1. It first allows congestion to happen
2. It detects the congestion (by observing packet loss)
3. Then it reduces its sending rate to come out of the congestion

So TCP does NOT prevent congestion. It only responds AFTER congestion has already happened. TCP detects congestion through packet loss — when you exceed the slow start phase and packets start getting dropped, TCP finds out and reduces the sender's rate.

This means congestion WILL still happen in the network, and while congestion is happening, all four QoS parameters get affected.

🎯 **Exam Important Points:**
- TCP congestion control does NOT ensure zero congestion
- TCP works reactively — it first detects congestion, THEN reduces rate
- TCP detects congestion by observing packet loss
- Congestion still impacts network performance through bandwidth, delay, jitter, and loss
- This is why we need separate QoS mechanisms at the network level

⚠️ **Common Confusion:**
- Many students think TCP congestion control prevents congestion. Wrong! It only reacts to congestion after it has already occurred.

---

## Concept 5: Four QoS Parameters That Congestion Impacts

📌 **Concept Name:** Four Parameters of Network Performance

🧠 **Simple Explanation:**

When there is congestion in the network, four specific parameters get impacted. These are the four primary QoS parameters:

1. **Bandwidth** — When congestion occurs, you get less bandwidth because the same bottleneck bandwidth is shared by multiple applications.

2. **Delay** — During congestion, packets wait longer in the packet buffer at intermediate routers. This increases the queuing delay, making overall delay higher.

3. **Jitter** — This is the variance (difference) in delay between different packets. During congestion, some packets may experience high delay while others experience low delay.

4. **Loss** — During congestion, packets can get dropped at intermediate routers when buffers overflow.

🎯 **Exam Important Points:**
- The four primary QoS parameters are: Bandwidth, Delay, Jitter, and Loss
- All four are affected by congestion
- These are the key parameters that determine the quality of service a network provides

---

## Concept 6: Network Bandwidth (QoS Parameter 1)

📌 **Concept Name:** Network Bandwidth

🧠 **Simple Explanation:**

Network bandwidth is the amount of data that can be transmitted over a link within a fixed amount of time. Think of it as the width of a pipe — a wider pipe can carry more water (data) per second.

The transcript quotes Tim Greene from a book called "QoS vs More Bandwidth": "When a drain chronically runs slow even though it isn't plugged, it's time to get a bigger pipe." This means: if your connection does not have enough bandwidth and you need more, you cannot manage with the existing pipe — you need to upgrade to a higher bandwidth line.

For example, if you have a 1 Mbps connection and you try to watch high-definition video, 1 Mbps may not be enough. You would need to upgrade to 8 Mbps or more.

Some applications are **bandwidth hungry** — especially video applications. Congestion limits the per-user bandwidth. To handle such applications, you need to design networks with high capacity.

🎯 **Exam Important Points:**
- Bandwidth = amount of data transmitted over a link in a fixed time
- Bandwidth is something we do not have much control over at the network level
- Some applications (like video) are "bandwidth hungry"
- Congestion limits per-user bandwidth
- Solution: design networks with high capacity / upgrade to higher bandwidth

⚠️ **Common Confusion:**
- Bandwidth is about the pipe capacity, NOT about how fast individual bits travel (that is propagation delay).

---

## Concept 7: Delay — Three Components (QoS Parameter 2)

📌 **Concept Name:** Three Components of Network Delay

🧠 **Simple Explanation:**

There are three different components of delay in a network:

### (a) Transmission Delay
This is the amount of time to push ALL the packet bits into the network link. It depends on the **capacity (bandwidth) of the channel**.

**Example from transcript:** If your network bandwidth is 8 Mbps and your packet size (including headers) is 1 MB = 8 Megabits, then:
- Transmission Delay = Packet Size / Bandwidth = 8 Megabits / 8 Mbps = **1 second**

Think of it as pouring water into a pipe — how long it takes to pour all the water depends on the width (capacity) of the pipe.

If you increase the capacity to 16 Mbps, the same 8 Megabit packet takes only **0.5 seconds** to push into the pipe. So transmission delay depends on the capacity of the pipe (the width).

### (b) Propagation Delay
This is the time for ONE bit to travel from one end of the link to the other end. It depends on the **length of the channel** (distance between sender and receiver) and the underlying communication medium.

Think of it this way: after you push the first bit into the pipe, that bit has to travel through the entire length of the pipe to reach the other end. The time it takes for that bit to reach the other end is the propagation delay.

**Key Difference:**
- Transmission delay → depends on **capacity (width) of the pipe**
- Propagation delay → depends on **length (distance) of the pipe**

### (c) Queuing Delay
This is the delay at the interface buffer of intermediate routers. When data arrives at a router, it gets enqueued in the router's packet buffer queue. If many devices are sending data through the same router, packets have to wait in the queue before being processed.

**Example from transcript:** If a router can process only 1 packet per second (outgoing link capacity), but 8 packets arrive every second from multiple senders, the queue keeps growing. Each packet waits longer and longer. This is like waiting in a long line at a movie theatre ticket counter — if people arrive faster than the counter can serve them, the line (queue) grows.

🛠 **Real-world Example (from transcript):**
Think of a ticket counter at INOX movie hall. If people are coming faster than the counter can serve, you wait longer. The same happens at router buffers.

🎯 **Exam Important Points:**
- Three delay components: Transmission Delay, Propagation Delay, Queuing Delay
- Transmission Delay = Packet Size / Bandwidth (depends on channel capacity)
- Propagation Delay = time for 1 bit to travel end-to-end (depends on link length and medium)
- Queuing Delay = waiting time in router buffer queue (depends on traffic load)
- **Transmission Delay example:** 8 Mbps link, 1 MB packet → 8 Mbit / 8 Mbps = 1 second
- **If bandwidth doubles to 16 Mbps:** same packet → 8 Mbit / 16 Mbps = 0.5 seconds

⚠️ **Common Confusions:**
- Transmission delay is NOT the same as propagation delay. Transmission delay = time to push all bits onto the link. Propagation delay = time for one bit to travel to the other end.
- Transmission delay depends on bandwidth (pipe width). Propagation delay depends on distance (pipe length).

---

## Concept 8: Queuing Delay Dominates

📌 **Concept Name:** Queuing Delay is the Major Delay Component

🧠 **Simple Explanation:**

Among the three delay components, the transcript makes a very important statement:

**In general, Queuing Delay >> Transmission Delay + Propagation Delay**

This means queuing delay is significantly much more than the other two combined. The queuing delay dominates the overall network delay.

Why? Because packet multiplexing in network devices like routers and switches causes packets to wait in queues. When there is congestion, more packets arrive at the queue than the router can process, and the queuing delay increases sharply.

The impact of congestion is directly seen as an increase in queuing delay.

🎯 **Exam Important Points:**
- Queuing Delay >> Transmission Delay + Propagation Delay (in general)
- Queuing delay is the MAJOR delay component in the network
- Packet multiplexing in routers and switches impacts the queuing delay
- Congestion → more packets in queue → increased queuing delay

---

## Concept 9: Jitter — Variance in End-to-End Delay (QoS Parameter 3)

📌 **Concept Name:** Jitter

🧠 **Simple Explanation:**

Jitter is the **variation in end-to-end delay** among different packets. It is basically the "variance of delay."

Imagine you send 4 packets from source to destination through the network:
- Packet 1 has delay = 10 ms
- Packet 2 has delay = 15 ms
- Packet 3 has delay = 6 ms
- Packet 4 has delay = 2 ms

The maximum delay is 15 ms and minimum delay is 2 ms. This big variance (difference) is the jitter. Why does this happen? Because different packets may experience different levels of congestion at intermediate routers. Some packets may travel during congestion (high delay), while others may travel when congestion has cleared (low delay, because TCP has reduced its rate after detecting packet loss).

**Why jitter matters for video streaming:**

When you watch a live streaming session, video packets arrive one after another at the client buffer, and the player renders them. If different packets have different delays:
- Some packets arrive quickly → client plays immediately
- Next packet has higher delay → client waits → video seems stuck
- Next packet comes very fast → plays immediately
- Next packet again delayed → waits again

This creates a **jerkiness** in video quality. The data is not coming at a constant bitrate — it is coming at a variable bitrate with high variation. This makes the video player unable to play at a constant rate, causing lots of ups and downs in quality.

This is why jitter is very important for ensuring QoS in video streaming applications.

🛠 **Real-world Example (from transcript):**
During a live NPTEL streaming session, if packets arrive with varying delays, the video quality becomes jerky — sometimes good, sometimes stuck, sometimes good again.

🎯 **Exam Important Points:**
- Jitter = Variation in End-to-End Delay
- Jitter = Variance of delay among different packets
- Different packets experience different congestion levels → different delays → jitter
- High jitter → jerky video quality in live streaming
- Jitter is critical for multimedia streaming applications
- Without steady stream of packets, video player cannot play at a constant rate

⚠️ **Common Confusion:**
- Jitter is NOT the same as delay. Delay is the actual time a packet takes. Jitter is the VARIATION (difference) in delay between packets.

---

## Concept 10: Loss (QoS Parameter 4)

📌 **Concept Name:** Packet Loss

🧠 **Simple Explanation:**

Loss is a relative measure of the number of packets (or segments or bits) that were **not received** compared to the total number of packets (or segments or bits) that were transmitted.

Loss is a function of **availability**:
- If the network is available (capacity is more than demand) → loss will generally be **zero**
- If capacity is less than demand (congestion) → you will see **significant loss**

This is why whenever there is congestion, there is significant packet loss, and TCP takes packet loss as an indication of congestion.

**Important Note from transcript:** The assumption that "if capacity > demand, loss = 0" is **NOT true for wireless networks.** In wireless networks, there can be loss from the channel itself due to interference. Think of many people talking in a single room — the noise makes it impossible to hear anyone clearly. The same thing happens in wireless media — interference causes packet loss even without congestion.

🎯 **Exam Important Points:**
- Loss = (packets not received / total packets transmitted) — a relative measure
- Loss is a function of availability
- If capacity > demand → loss ≈ 0 (in wired networks)
- If capacity < demand (congestion) → significant loss
- **Critical exception:** In wireless networks, loss can happen even without congestion due to interference
- TCP interprets packet loss as a sign of congestion
- For voice applications, packet loss is especially critical — even a small amount means you cannot hear properly

⚠️ **Common Confusion:**
- In wired networks, loss mainly happens due to congestion (buffer overflow). But in wireless networks, loss can also happen due to channel interference — this is a key difference.

---

## Concept 11: Application-Level QoS Requirements Table

📌 **Concept Name:** Different Applications Need Different QoS

🧠 **Simple Explanation:**

The transcript presents a very important table (from Cisco) showing what QoS parameters different applications need:

| Application | Loss | Delay (One-way) | Jitter | Bandwidth |
|---|---|---|---|---|
| **Voice** | ≤ 1% | ≤ 150 ms | ≤ 30 ms | 21 Kbps – 320 Kbps |
| **Interactive Video** | ≤ 1% | ≤ 150 ms | ≤ 30 ms | On demand |
| **Streaming Video** | ≤ 5% | ≤ Buffer time | On buffer time | On demand |
| **Data** | — | — | — | Best Effort |

**Explanation of each:**

**Voice:** Very strict requirements. Loss must be ≤ 1%, delay ≤ 150 ms, jitter ≤ 30 ms, and bandwidth of 21–320 Kbps is needed. Voice is very sensitive to loss — if packets are lost, you cannot hear properly.

**Interactive Video (live streaming):** Similar to voice — loss ≤ 1%, delay ≤ 150 ms, jitter ≤ 30 ms. Bandwidth is on demand (high quality video needs more bandwidth).

**Streaming Video (pre-recorded, like YouTube):** Can tolerate more loss (≤ 5%) because if a frame is lost, you can recover it by averaging neighboring frames (frame 1 and frame 3 can help reconstruct lost frame 2). Delay depends on buffer time at the client side. Jitter also depends on buffer time. Bandwidth is on demand.

**Normal Data Transfer:** No strict bounds on loss, delay, or jitter. Bandwidth is "best effort" — whatever is available.

🎯 **Exam Important Points:**
- Voice: most strict on loss (≤ 1%), delay (≤ 150 ms), jitter (≤ 30 ms)
- Interactive video: similar to voice requirements
- Streaming video: can tolerate more loss (≤ 5%) because frames can be recovered by averaging
- Data: best effort, no strict QoS bounds
- This table is from Cisco and is very likely to appear in exam questions

⚠️ **Common Confusion:**
- Streaming video can tolerate MORE loss than voice/interactive video because it is pre-recorded, so lost frames can be estimated from neighboring frames.

---

## Concept 12: Formal Definition of QoS (Cisco Definition)

📌 **Concept Name:** Formal Definition of Quality of Service

🧠 **Simple Explanation:**

The formal definition from Cisco states:

**Quality of Service (QoS) refers to the capability of a network to provide better service to selected network traffic over various technologies, including Frame Relay, Asynchronous Transfer Mode (ATM), Ethernet and 802.1 networks, SONET, and IP-routed networks that may use any or all of these underlying technologies.**

**The primary goal of QoS is to provide priority including dedicated bandwidth, controlled jitter and latency (required by some real-time and interactive traffic), and improved loss characteristics.**

Key words to understand:
- **Capability of a network to provide better service** — QoS is about the network's ability
- **Selected network traffic** — Not ALL traffic gets QoS. Only selected traffic like voice, video that need it
- **Over various technologies** — Different links in a path may use different technologies (wireless, Ethernet, optical/SONET). QoS must work across all of them
- **Primary goal: provide priority** — Give dedicated bandwidth, control jitter and latency (one-way delay), and improve loss for certain classes of traffic
- **Latency** = one-way delay required by real-time and interactive traffic

🎯 **Exam Important Points:**
- QoS = capability of a network to provide better service to SELECTED network traffic
- It works over various underlying technologies: Frame Relay, ATM, Ethernet, 802.1, SONET, IP-routed networks
- Primary goal: dedicated bandwidth, controlled jitter and latency, improved loss
- Latency means one-way delay
- Between two end hosts, there may be multiple different types of links (wireless, Ethernet, optical)

---

## Concept 13: Ensuring QoS Over a Packet Switching Network — Four Questions

📌 **Concept Name:** Four Requirements to Ensure QoS

🧠 **Simple Explanation:**

To ensure Quality of Service over a packet switched network, the transcript says we need to answer four fundamental questions:

1. **What applications need from the network** — What type of QoS the application is expecting (how much bandwidth, how much delay can it tolerate, etc.)

2. **How to regulate the traffic that enters the network** — How to control and manage the traffic entering the network so that QoS can be maintained

3. **How to reserve resources at routers to guarantee performance** — You need end-to-end dedicated resources to ensure certain classes of QoS. Resources must be reserved at every intermediate router.

4. **Whether the network can safely accept more traffic** — Can the network take in more traffic without violating the QoS of existing traffic flows?

🎯 **Exam Important Points:**
- Four things needed for QoS in packet switching networks
- Application requirements, traffic regulation, resource reservation at routers, and admission control (whether to accept more traffic)
- These are covered in detail in subsequent lectures

---

## Concept 14: Concept of Flow

📌 **Concept Name:** Flow

🧠 **Simple Explanation:**

In QoS, we frequently use the term **Flow**. A flow is a **stream of packets from a source to a destination**.

Now, "source to destination" can be defined at different levels:
- **Machine to Machine** — QoS between two physical machines
- **Process to Process** — QoS between two specific processes on different machines
- **Application to Application** — QoS between two applications
- **Socket to Socket** — QoS between two specific sockets

The important point is: **Different flows require different levels of QoS.** A voice flow needs very strict delay and loss requirements, while a file transfer flow can work with best effort.

🎯 **Exam Important Points:**
- Flow = stream of packets from source to destination
- Source to destination can mean: machine-to-machine, process-to-process, application-to-application, socket-to-socket
- Different flows require different levels of QoS
- QoS is provided to individual flows based on their requirements

---

## Concept 15: Why QoS is Considered at the Network Layer

📌 **Concept Name:** QoS at the Network Layer

🧠 **Simple Explanation:**

Why do we implement QoS at the Network Layer and not at other layers? The transcript gives a clear reason:

Maintaining QoS requires both **per-hop behavior** and **end-to-end behavior**.

When you want to ensure QoS between two end hosts, you have multiple intermediate routers/switches between them. You need to monitor and ensure the end-to-end performance:
- End-to-end delay
- End-to-end bandwidth
- End-to-end jitter
- Total end-to-end data loss

But to guarantee these end-to-end requirements, you must **reserve resources at every individual hop** in the path. If even one hop fails to provide its share, the end-to-end guarantee breaks.

Now, where does the Network Layer sit?
- **Transport Layer** → gives end-to-end information
- **Data Link Layer** → handles per-hop behavior
- **Network Layer** → sits in between, bridges both!

The network layer can get feedback from the transport layer (end-to-end) and apply things to the data link layer (per-hop). That is why QoS is implemented at the network layer — it bridges the end-to-end (Transport) and per-hop (Data Link Layer).

**Key point:** Resource reservation needs to be on a **per-hop basis** — otherwise end-to-end requirements cannot be guaranteed.

🎯 **Exam Important Points:**
- QoS requires both per-hop and end-to-end behavior
- End-to-end performance parameters: delay, bandwidth, jitter, loss
- Resource reservation must be on per-hop basis
- Network layer bridges end-to-end (Transport Layer) and per-hop (Data Link Layer)
- That is why QoS is considered at the Network Layer
- If resource reservation is not per-hop, end-to-end requirements CANNOT be guaranteed

⚠️ **Common Confusion:**
- QoS is NOT purely a transport layer thing (transport only gives end-to-end). QoS is NOT purely a data link layer thing (data link only gives per-hop). QoS is at the network layer because it needs BOTH.

---

## Concept 16: Application Classes Based on QoS

📌 **Concept Name:** Four Application Classes Based on QoS

🧠 **Simple Explanation:**

Based on the quality of service requirements, applications are classified into four classes:

### Class 1: Constant Bit Rate (CBR)
- Example: **Telephone applications — Voice over IP (VoIP)**
- Requires a constant streaming of data bits
- The bit rate stays the same throughout

### Class 2: Real-Time Variable Bit Rate
- Example: **Video conferencing**
- The bit rate can be variable depending on the frames being transferred
- But it MUST be in real-time (no delay tolerance)

### Class 3: Non Real-Time Variable Bit Rate
- Example: **On-demand video streaming — IPTV (television service over IP)**
- The bit rate is variable
- But it does NOT need to be in real-time (some delay is acceptable because the content is pre-recorded)

### Class 4: Available Bit Rate / Best Effort
- Example: **File transfer**
- Uses whatever bandwidth is available
- No QoS guarantees — just best effort service

🎯 **Exam Important Points:**
- Four classes: Constant Bit Rate, Real-Time Variable Bit Rate, Non Real-Time Variable Bit Rate, Available Bit Rate (Best Effort)
- CBR → VoIP (telephone), Real-Time VBR → video conferencing, Non Real-Time VBR → IPTV/on-demand streaming, Best Effort → file transfer
- Know the example for each class — very likely exam question

⚠️ **Common Confusion:**
- Real-Time Variable Bit Rate vs Non Real-Time Variable Bit Rate: The difference is whether it needs to be live (real-time) or can be pre-recorded (non real-time). Both have variable bit rates, but the timing requirement is different.

---

## Summary Table: Four QoS Parameters at a Glance

| Parameter | What It Measures | Depends On | Impact of Congestion |
|---|---|---|---|
| **Bandwidth** | Amount of data transmitted per unit time | Link capacity | Reduces (shared among users) |
| **Delay** | Time for packet to go from source to destination | Transmission + Propagation + Queuing | Increases (especially queuing delay) |
| **Jitter** | Variation in delay across packets | Congestion variation at routers | Increases (packets get uneven delays) |
| **Loss** | Ratio of packets lost vs. total sent | Network capacity vs. demand | Increases (buffer overflow) |

---

## Summary Table: Application QoS Classes

| Class | Bit Rate Type | Real-Time? | Example |
|---|---|---|---|
| Constant Bit Rate | Constant | Yes | VoIP (telephone) |
| Real-Time Variable Bit Rate | Variable | Yes | Video Conferencing |
| Non Real-Time Variable Bit Rate | Variable | No | IPTV, On-demand streaming |
| Available Bit Rate / Best Effort | Whatever available | No | File Transfer |

---

## Summary Table: Delay Components

| Delay Component | Depends On | Analogy |
|---|---|---|
| Transmission Delay | Capacity (width) of the pipe/link | How fast you can pour water into the pipe |
| Propagation Delay | Length (distance) of the pipe/link | How long it takes water to travel through the pipe |
| Queuing Delay | Traffic load at intermediate routers | Waiting in line at a ticket counter |

---

# 10 MCQs — Lecture 31: Internet QoS – I (What is QoS)

**(All questions strictly from this lecture transcript only)**

---

**Q1. What does TCP congestion control do when it detects congestion in the network?**

(A) It prevents congestion from ever happening  
(B) It increases the sending rate to push through congestion  
(C) It detects congestion and then reduces the sending rate  
(D) It switches to a different network path  

**Answer: (C)**  
**Explanation:** As per the transcript, TCP congestion control does NOT prevent congestion. It works reactively — it first detects congestion (by observing packet loss), and THEN reduces the sender's rate. Congestion still happens; TCP only responds after the fact.

---

**Q2. Which of the following is NOT one of the four primary QoS parameters mentioned in the lecture?**

(A) Bandwidth  
(B) Throughput  
(C) Jitter  
(D) Loss  

**Answer: (B)**  
**Explanation:** The four primary QoS parameters discussed in this lecture are Bandwidth, Delay, Jitter, and Loss. Throughput is not listed as one of the four primary QoS parameters in this lecture.

---

**Q3. Jitter is defined as:**

(A) The total delay experienced by a packet  
(B) The maximum delay in the network  
(C) The variation in end-to-end delay among different packets  
(D) The time to push all bits into the network  

**Answer: (C)**  
**Explanation:** The transcript clearly defines jitter as the "variation in end-to-end delay." It is the variance of delay — different packets experience different delays due to varying congestion levels at intermediate routers.

---

**Q4. Transmission delay depends on which of the following?**

(A) Length of the link  
(B) Capacity (bandwidth) of the channel  
(C) Number of intermediate routers  
(D) Type of application  

**Answer: (B)**  
**Explanation:** As per the transcript, transmission delay is the time to push all packet bits into the network. It depends on the capacity (bandwidth) of the channel. The transcript uses the pipe analogy — transmission delay depends on the WIDTH of the pipe.

---

**Q5. If a network has 8 Mbps bandwidth and the packet size is 1 MB (including headers), what is the transmission delay?**

(A) 0.5 seconds  
(B) 1 second  
(C) 2 seconds  
(D) 8 seconds  

**Answer: (B)**  
**Explanation:** From the transcript example: 1 MB = 8 Megabits. Bandwidth = 8 Mbps. Transmission Delay = 8 Megabits / 8 Mbps = 1 second.

---

**Q6. Which delay component is generally the largest in a network?**

(A) Transmission Delay  
(B) Propagation Delay  
(C) Queuing Delay  
(D) All three are equal  

**Answer: (C)**  
**Explanation:** The transcript explicitly states: "In general, Queuing delay >> Transmission delay + Propagation delay." Queuing delay is the major delay component that dominates in the network, especially during congestion.

---

**Q7. The assumption "if capacity > demand, then loss ≈ 0" is NOT true for which type of network?**

(A) Fiber optic networks  
(B) Ethernet networks  
(C) Wireless networks  
(D) SONET networks  

**Answer: (C)**  
**Explanation:** The transcript specifically notes this: "This assumption is not true for wireless networks." In wireless networks, loss can occur even without congestion because of interference in the open wireless medium — like many people talking in one room creating noise.

---

**Q8. According to the application QoS table from the lecture, which application can tolerate the MOST packet loss?**

(A) Voice  
(B) Interactive Video  
(C) Streaming Video  
(D) All have same loss tolerance  

**Answer: (C)**  
**Explanation:** From the QoS table in the transcript: Voice ≤ 1%, Interactive Video ≤ 1%, Streaming Video ≤ 5%. Streaming video can tolerate more loss because lost frames can be recovered by averaging neighboring frames (since it is pre-recorded video).

---

**Q9. Why is QoS considered at the Network Layer?**

(A) Because the network layer is the fastest layer  
(B) Because QoS only needs per-hop behavior  
(C) Because the network layer bridges end-to-end (Transport) and per-hop (Data Link) behavior  
(D) Because the transport layer cannot handle packets  

**Answer: (C)**  
**Explanation:** The transcript explains that maintaining QoS requires both per-hop and end-to-end behavior. The network layer sits between the transport layer (which provides end-to-end information) and the data link layer (which handles per-hop behavior). The network layer bridges both, making it the right place for QoS.

---

**Q10. "Constant Bit Rate" QoS class is used for which type of application?**

(A) Video conferencing  
(B) File transfer  
(C) On-demand video streaming (IPTV)  
(D) Voice over IP (VoIP) / Telephone applications  

**Answer: (D)**  
**Explanation:** The transcript states that Constant Bit Rate class is for telephone applications like Voice over IP (VoIP), which require a constant streaming of data bits at a fixed rate. Video conferencing uses Real-Time Variable Bit Rate, IPTV uses Non Real-Time Variable Bit Rate, and file transfer uses Available Bit Rate / Best Effort.

---

*End of Lecture 31 Study Guide*
