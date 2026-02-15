# Lecture 32 – Internet QoS – II (Basic QoS Architecture)

**Course:** Computer Networks and Internet Protocol  
**Instructor:** Prof. Sandip Chakraborty, IIT Kharagpur  
**Topic:** Basic QoS Architecture — Application Classes, QoS Pipeline Stages, and Traffic Management

---

## Recap from Previous Lecture

In the previous lecture (Lecture 31), we discussed what Quality of Service (QoS) means in the internet. We learned that QoS refers to how well the network performs for a particular application. We also identified 4 key parameters that impact QoS: **bandwidth, delay, jitter, and loss**. To ensure QoS over the internet, we need to control these 4 parameters.

In this lecture (Lecture 32), we go deeper and learn what support the TCP/IP network needs to provide to ensure QoS. We look at the **Basic QoS Architecture** — a step-by-step pipeline that every packet passes through inside the network.

---

## Concept 1: Application Classes Based on QoS

📌 **Concept Name:** Four Application Classes Based on QoS Requirements

🧠 **Simple Explanation:**

Not all applications need the same kind of network performance. Some need very strict timing, some are flexible. Based on their QoS requirements, applications are divided into **4 classes**:

**Class 1 — Constant Bit Rate (CBR):**  
This is for applications that need data to arrive at a **fixed, steady rate** — no faster, no slower. Think of a telephone call over the internet (VoIP). When you speak, your voice is converted to digital data, and this digital data must travel at a constant bit rate. The receiver expects data at a steady speed to properly play back your voice. This is also called a "severe" application because the QoS requirement is very strict.

**Class 2 — Real-Time Variable Bit Rate:**  
Here, the bit rate can change (it is variable), but the data **must** arrive in real time. "Real time" means there is a **maximum deadline** by which data must be delivered. If it misses that deadline, the transfer is considered a failure. Example: video conferencing or live streaming. The video quality might change (variable bit rate), but it must arrive on time.

**Class 3 — Non-Real-Time Variable Bit Rate:**  
Here, you do **not** need to send data in strict real time. There is **no hard deadline**. Buffering is supported — you can load data ahead of time. But you still need **some level of QoS** — there is a loose bound on delay and on how much packet loss is acceptable. Example: on-demand video streaming like IPTV. You can buffer a few seconds ahead, so strict real-time delivery is not needed, but the video should still play smoothly.

**Class 4 — Available Bit Rate / Best Effort:**  
This is the most relaxed class. Whatever bandwidth is currently available in the network, you use that to transfer data. There are **no strict QoS requirements**. Example: file transfer (FTP). You just want to move a file from one place to another — it does not matter if it arrives a bit slow or fast.

🎯 **Exam Important Points:**
- There are exactly 4 classes of applications based on QoS.
- CBR = constant speed needed (VoIP, telephone).
- Real-time variable = deadline-based delivery (video conferencing).
- Non-real-time variable = loose delay/loss bounds, buffering OK (IPTV, on-demand streaming).
- Available bit rate = best effort, no guarantees (file transfer).

⚠️ **Common Confusions:**
- Students confuse "real-time variable" and "non-real-time variable." The key difference is: real-time has a **hard deadline** (miss it = failure), while non-real-time has a **loose bound** (some delay and loss are tolerated).
- "Best effort" does NOT mean poor quality — it just means no QoS guarantee. If the network is free, file transfer can be very fast.

---

## Concept 2: Basic QoS Architecture — The End-to-End Pipeline

📌 **Concept Name:** The Basic QoS Architecture (Pipeline Stages)

🧠 **Simple Explanation:**

To ensure quality of service in the internet, packets go through a **pipeline of filters/stages** as they move from source to destination. At every stage, certain checks and actions are taken on the packets. These stages are implemented inside **Layer 3 devices (routers)**.

The pipeline has **6 stages** in order:

1. **Admission Control**
2. **Classification and Marking**
3. **Policing and Markdown**
4. **Scheduling (Queuing and Dropping)**
5. **Traffic Shaping**
6. **Link-Specific Mechanisms**

Think of it like a security checkpoint system at an airport: first you check if the passenger can enter, then you tag their luggage, then you check if they follow the rules, then you put them in the right queue, then you regulate how they board the plane, and finally any airline-specific rules are applied.

🎯 **Exam Important Points:**
- All these filters are implemented inside routers (Layer 3 devices).
- The architecture is an **end-to-end pipeline** — packets pass through each stage sequentially.
- Remember all 6 stages and their correct order.

⚠️ **Common Confusions:**
- These stages are NOT at the application layer. They are inside network devices (routers).
- All stages work together — no single stage alone can guarantee QoS.

---

## Concept 3: Admission Control

📌 **Concept Name:** Admission Control

🧠 **Simple Explanation:**

Admission control is the **first stage** of the QoS architecture. Its job is very simple: **decide whether to allow a new flow (data stream) into the network or not.**

Here is how it works: When a new flow wants to enter the network, the admission control module checks — "If I allow this new flow, can I still maintain the quality of service for ALL existing flows PLUS this new flow?" If yes, the new flow is admitted (allowed in). If no, the new flow is **blocked** (rejected).

**Real-world example from the transcript:** Think about cellular networks. When you make a phone call and hear "All lines are busy, please dial after some time," that is admission control in action. The cellular service provider does not have enough resources to guarantee quality for your call without hurting existing calls, so it blocks your call.

The key principle is: **The network does not allow new flows if all resources are already occupied in servicing existing flows based on their QoS requirements.**

🧠 **How does the network know what QoS level is expected?** Through something called a **Service Level Agreement (SLA)** — which we will discuss in detail later in this lecture. The SLA tells the network what quality of service the flow needs.

🎯 **Exam Important Points:**
- Admission control = first stage of QoS architecture.
- It admits a new flow ONLY IF quality of service for all existing flows + the new flow can be satisfied.
- If resources are insufficient, the new flow is rejected/blocked.
- Real-world example: "All lines are busy" on cellular networks.

⚠️ **Common Confusions:**
- Admission control does NOT drop packets that are already flowing. It only decides about **new flows** entering the network.
- It does NOT improve the quality of existing flows — it only prevents overloading.

---

## Concept 4: Classification and Marking

📌 **Concept Name:** Classification and Marking

🧠 **Simple Explanation:**

After a flow is admitted into the network, the next step is **classification and marking**. This stage answers the question: "What type of traffic class does this packet belong to?"

Since there are 4 classes of applications (CBR, real-time variable, non-real-time variable, best effort), the classification module looks at each packet and **identifies which class it belongs to**. Then it **marks** the packet with that class information.

Think of it like putting colored stickers on packages at a warehouse: a blue sticker for voice packets, a red sticker for video packets, a green sticker for streaming, and a yellow sticker for regular data. Once marked, every other stage in the pipeline knows how to treat that packet.

**How is marking done in IP?** Using the **IP Type of Service (ToS) field** in the IP header. This is an **8-bit field** inside every IP packet header. Within this 8-bit field:

- The **first 3 bits** are called **IP Precedence**. These 3 bits define 8 different classes of traffic (since 2³ = 8).
- The **next 4 bits** define priority **within** a class. For example, if both voice and video are in the "critical" class, these 4 bits can give voice higher priority than video.
- The **last 1 bit** is unused/reserved.

**IP Precedence Values (from the transcript):**

| IP Precedence (Decimal) | Binary | Name |
|---|---|---|
| 0 | 000 | Routine |
| 1 | 001 | Priority |
| 2 | 010 | Immediate |
| 3 | 011 | Flash |
| 4 | 100 | Flash Override |
| 5 | 101 | Critical |
| 6 | 110 | Internetwork Control |
| 7 | 111 | Network Control |

So, by looking at the ToS field in the IP header, routers know exactly what class of traffic a packet belongs to and can treat it accordingly.

🎯 **Exam Important Points:**
- Classification = identifying what QoS class a packet belongs to.
- Marking = tagging/labeling the packet with its class information.
- Done using the **IP Type of Service (ToS) field** — an 8-bit field in the IP header.
- First 3 bits = IP Precedence (8 traffic classes: 0 to 7).
- Next 4 bits = priority within a class.
- Precedence 0 = Routine (lowest), Precedence 7 = Network Control (highest).
- All filtering is done inside Layer 3 devices (routers).

⚠️ **Common Confusions:**
- IP Precedence uses only 3 bits, not the full 8 bits of the ToS field.
- The 4 bits after IP Precedence are for sub-priority within the same class — they are NOT separate classes.
- Precedence value 0 is the LOWEST priority (Routine), not the highest.

---

## Concept 5: Traffic Policing and Markdown

📌 **Concept Name:** Traffic Policing and Markdown

🧠 **Simple Explanation:**

The third filter in the QoS pipeline is **policing and markdown**. The job of this stage is to **monitor the flow of traffic** and check whether packets are following the QoS rules or violating them.

**What does traffic policing do?** It looks at whether certain flows or packets are **violating the quality of service requirements**. If a packet is found to be violating the QoS requirement, the router takes action — it can **drop the packet** or **reduce its priority** (markdown).

**Example from the transcript:** Imagine your end-to-end delay requirement is 10 milliseconds. A packet travels from the source to an intermediate router. When it reaches the router, the router finds that the packet has already experienced 9 milliseconds of delay. The router knows that it is impossible to deliver this packet to the final destination within just 1 more millisecond. So, what does the router do? It **drops the packet**.

**Why drop a packet?** Because if you cannot satisfy the QoS for that packet, there is no point in sending it further. Forwarding it would waste bandwidth, clog the link, and the destination application would still reject it because it arrived too late. So dropping it early is actually better for the network.

**What is a Service Level Agreement (SLA)?** SLA is an agreement or contract between the **customer and the service provider** to maintain the quality of service of an application. For example, if you want VoIP service, you go to your service provider (like Airtel or Vodafone) and sign a contract that says: "For VoIP traffic, I need this level of quality. What do I need to pay?"

**Real-world SLA example from the transcript:** When you subscribe to a broadband connection, you see multiple packages: 1 Mbps leased line for 1 month, 256 Kbps for 15 days, etc. Sometimes there is even a difference between uplink and downlink rates (e.g., 256 Kbps uplink and 1 Mbps downlink). These packages are a form of service level agreements.

**How is SLA configured?** SLA parameters are written into the edge routers or gateway routers of the service provider. The transcript shows an example of configuring SLA in a router: "track 10 IP SLA 11 reachability, delay down 15 up 15" — meaning the downlink can tolerate up to 15 milliseconds of delay and the uplink can also tolerate up to 15 milliseconds.

**Traffic Policing action:** Traffic policing monitors the flow of traffic and marks packets to take appropriate actions like reducing priority or dropping packets that violate the SLA.

🎯 **Exam Important Points:**
- Policing = monitoring whether flows violate QoS requirements.
- If a packet has already experienced too much delay and cannot meet its deadline, the router drops it.
- Dropping saves bandwidth and prevents useless processing at the destination.
- SLA = Service Level Agreement = contract between customer and service provider for QoS.
- SLA is configured in edge/gateway routers of the network service provider.
- Traffic policing can take actions: drop packets, reduce priority (markdown).

⚠️ **Common Confusions:**
- Policing is NOT the same as shaping (explained later). Policing **drops** violating packets. Shaping **smooths** the traffic rate.
- SLA is not just about bandwidth — it can include delay, jitter, and loss guarantees too.

---

## Concept 6: Scheduling (Queuing and Dropping)

📌 **Concept Name:** Traffic Scheduling Based on Queuing

🧠 **Simple Explanation:**

The fourth stage in the QoS pipeline is **scheduling**. After the traffic policer has decided which packets are valid and which should be dropped, the remaining packets need to be sent out on the outgoing link. But packets from different classes have different priorities. Scheduling decides **which packet to send first**.

**How does scheduling work?** The router maintains **multiple queues** at the output interface. Each queue corresponds to a different traffic class. When a packet arrives, based on its class/marking, it is placed in the appropriate queue. Then the scheduling mechanism decides which queue to serve next based on the **scheduling policy**.

**Example: Priority Queuing (from the transcript):**

Imagine you have 3 queues:
- **Red queue** (priority 1 — highest): For voice packets
- **Blue queue** (priority 2): For video packets
- **Yellow queue** (priority 3 — lowest): For data packets

When packets arrive on the input link:
- A voice packet (red) goes into the red queue.
- A video packet (blue) goes into the blue queue.
- A data packet (yellow) goes into the yellow queue.

The priority queuing rule says: **Always serve the highest priority queue first.** So:
1. If the red queue has packets → send those first.
2. Only when the red queue is empty → go to the blue queue.
3. Only when BOTH red and blue queues are empty → serve the yellow queue.

This way, voice traffic (which has the strictest delay and jitter requirements) always gets transmitted first.

**Other types of scheduling mentioned:** Priority queuing is just one type. The transcript also mentions other mechanisms like custom queuing, weighted fair queuing, and fair weighted fair queuing — but says these will be discussed in later lectures.

🎯 **Exam Important Points:**
- Scheduling = deciding which packet to send first based on traffic class priority.
- Multiple queues are maintained at the output interface of the router.
- Priority Queuing: highest priority queue is always served first; lower priority queues only served when higher queues are empty.
- In the example: Red (voice) = highest priority, Blue (video) = second, Yellow (data) = lowest.
- Other scheduling types exist: custom queuing, weighted fair queuing, etc.

⚠️ **Common Confusions:**
- Priority queuing can cause **starvation** — if the high priority queue always has packets, the low priority queue may never get served. This is a known problem.
- Scheduling is NOT the same as classification. Classification marks packets; scheduling decides the order of transmission.

---

## Concept 7: Traffic Shaping

📌 **Concept Name:** Traffic Shaping

🧠 **Simple Explanation:**

The fifth stage in the QoS pipeline is **traffic shaping**. Its job is to **control the outgoing traffic rate** so that it is smooth and steady, regardless of how the incoming traffic arrived.

**The problem:** On the internet, traffic does not arrive at a nice, steady rate. It arrives in **bursts** — sometimes a lot of packets come in a short time, then nothing for a while, then another burst. This creates **jitter** (variation in delay).

**What traffic shaping does:** The shaper sits between the input and the output. Packets arrive at random, bursty intervals. The shaper holds them in a buffer and releases them at a **constant rate** on the outgoing link. So the input is bursty, but the output is smooth and regulated.

Think of it like a water tank: water flows in at irregular rates (sometimes a lot, sometimes a little). But the tap at the bottom releases water at a steady, constant flow. The tank absorbs the variation.

**Key characteristics from the transcript:**
- Input traffic is **bursty**.
- Output traffic has a **constant packet rate** — this reduces jitter.
- Traffic shaping controls the outgoing rate **irrespective of** the incoming traffic rate.
- The output can be constant bit rate from the interface buffer, or it can have certain controlled delay/jitter based on application requirements.

🎯 **Exam Important Points:**
- Traffic shaping = smoothing bursty input into constant-rate output.
- It reduces jitter in the network.
- It controls the outgoing traffic rate irrespective of incoming rate.
- Acts like a traffic regulator.

⚠️ **Common Confusions:**
- Traffic shaping does NOT drop packets — it buffers and smooths them.
- This is different from traffic policing, which drops violating packets. (Detailed comparison in next concept.)

---

## Concept 8: Traffic Policing vs. Traffic Shaping — The Key Difference

📌 **Concept Name:** Difference Between Traffic Policing and Traffic Shaping

🧠 **Simple Explanation:**

This is a very important distinction that the transcript emphasizes you must remember:

**Traffic Policing:**
- It **monitors** whether certain flows or packets are violating the QoS service requirements.
- If a flow exceeds the expected traffic rate (crosses the limit), the policer simply **drops the excess packets** — it chops off the peaks.
- The expected traffic rate acts like a boundary line. Anything above that line gets dropped.
- Result: traffic rate comes down, but it is still **bursty/irregular** — just with the peaks cut off.

**Traffic Shaping:**
- It takes bursty, irregular traffic and **smooths it out** to a regulated, constant rate.
- Instead of dropping excess packets, it **buffers** them and releases them at a steady rate.
- Result: the output is a **smooth, constant flow** — jitter is reduced.

**When do we use which?**
- Both are needed in the network.
- Traffic shaping alone may **not always work**. If the average incoming rate is higher than the expected rate, the shaper's buffer will keep growing and eventually overflow. In that case, you first need **traffic policing** to drop packets that violate the SLA.
- After policing drops the violating packets, the remaining traffic (whose average rate is now within the expected bound) can be passed through the **traffic shaper** to smooth it out.
- So: **Policing first (drop violators) → then Shaping (smooth the rest).**

🎯 **Exam Important Points:**
- Policing = drops packets that exceed the limit. Output is still bursty but with peaks removed.
- Shaping = smooths traffic to constant rate. Output is regular.
- Both are needed: policing handles rate violations, shaping handles jitter.
- If average rate > expected rate → policing needed first; shaping alone won't work.
- If average rate < expected rate → shaping can work to regulate the output.

⚠️ **Common Confusions:**
- The biggest confusion is thinking policing and shaping are the same thing. They are NOT. Policing drops; shaping smooths.
- Shaping does NOT drop packets; it delays them in a buffer and outputs at constant rate.
- Policing does NOT smooth traffic; it just removes the excess.

---

## Concept 9: Link-Specific Mechanisms

📌 **Concept Name:** Link-Specific QoS Mechanisms

🧠 **Simple Explanation:**

The sixth and final stage in the QoS architecture is **link-specific mechanisms**. This stage applies QoS features that are specific to the type of link being used.

For example, if the link is a **Wi-Fi link**, then Wi-Fi has its own QoS provisioning services — like prioritizing certain traffic at the link layer, controlling channel access, etc. These are specific to the Wi-Fi protocol and are applied at this stage.

Different link technologies (Ethernet, Wi-Fi, LTE, etc.) may have their own built-in QoS mechanisms. This final stage ensures those technology-specific features are also applied.

🎯 **Exam Important Points:**
- This is the last stage of the QoS pipeline.
- It applies QoS features specific to the underlying link technology (e.g., Wi-Fi QoS).
- Examples include link-layer traffic prioritization and channel access control.

⚠️ **Common Confusions:**
- This stage is at the link layer, not the network layer. But it works together with the network-layer QoS stages.
- Not all links have special QoS mechanisms — some just use best-effort delivery at the link layer.

---

## Concept 10: Summary — Complete QoS Pipeline Flow

📌 **Concept Name:** End-to-End QoS Architecture Summary

🧠 **Simple Explanation:**

Let's put everything together. When a packet moves through the network, it passes through these stages at every router:

**Stage 1 — Admission Control:** Can we accept this new flow without hurting existing flows? If yes, admit it. If no, block it.

**Stage 2 — Classification and Marking:** What class does this packet belong to? Mark it using the IP ToS field (IP Precedence — 3 bits for class, 4 bits for sub-priority).

**Stage 3 — Policing and Markdown:** Is this packet violating the SLA? If yes, drop it or reduce its priority. (Example: if a packet has already experienced 9ms delay out of a 10ms limit, and the remaining path clearly needs more than 1ms, drop the packet.)

**Stage 4 — Scheduling:** Place the packet in the right priority queue. Serve queues based on scheduling policy (e.g., priority queuing — serve highest priority first).

**Stage 5 — Traffic Shaping:** Smooth out bursty traffic into a constant-rate output stream. Reduce jitter.

**Stage 6 — Link-Specific Mechanisms:** Apply any QoS features specific to the underlying link technology (Wi-Fi, Ethernet, etc.).

**Key takeaway from the transcript:** In the next lecture, the professor will go deeper into 3 of these components — traffic shaping, traffic policing, and traffic scheduling — as they are the most important from the QoS perspective.

🎯 **Exam Important Points:**
- Know all 6 stages in correct order.
- Each stage has a distinct purpose — no two stages do the same thing.
- All stages are implemented in Layer 3 devices (routers).
- The architecture is an end-to-end pipeline.
- SLA is the key contract that defines what QoS level a flow expects.

---

## 10 MCQs — Lecture 32 (Strictly from Transcript)

### Q1. Which of the following is NOT one of the 4 application classes based on QoS?

A) Constant Bit Rate  
B) Real-Time Variable Bit Rate  
C) Guaranteed Bandwidth Rate  
D) Available Bit Rate / Best Effort  

**Answer: C**  
**Explanation:** The 4 classes from the transcript are: Constant Bit Rate, Real-Time Variable Bit Rate, Non-Real-Time Variable Bit Rate, and Available Bit Rate (Best Effort). "Guaranteed Bandwidth Rate" is not one of them.

---

### Q2. Which application class is best suited for VoIP (Voice over IP)?

A) Real-Time Variable Bit Rate  
B) Constant Bit Rate  
C) Non-Real-Time Variable Bit Rate  
D) Best Effort  

**Answer: B**  
**Explanation:** VoIP requires data to be transferred at a constant, steady bit rate. The voice is digitized and must arrive at a constant rate for proper playback. This makes it a Constant Bit Rate application.

---

### Q3. What is the first stage in the Basic QoS Architecture pipeline?

A) Classification and Marking  
B) Traffic Shaping  
C) Admission Control  
D) Scheduling  

**Answer: C**  
**Explanation:** Admission Control is the first stage. It checks whether a new flow can be admitted without violating the QoS of existing flows.

---

### Q4. How many bits are used for IP Precedence in the IP Type of Service (ToS) field?

A) 4 bits  
B) 8 bits  
C) 3 bits  
D) 6 bits  

**Answer: C**  
**Explanation:** The ToS field is 8 bits total. The first 3 bits are for IP Precedence (defining 8 traffic classes), the next 4 bits define priority within a class, and the last 1 bit is unused.

---

### Q5. In priority queuing, when is the low priority queue served?

A) When it has more packets than the high priority queue  
B) Only when all higher priority queues are empty  
C) In a round-robin fashion with other queues  
D) When the timer for the low priority queue expires  

**Answer: B**  
**Explanation:** In priority queuing, the highest priority queue is always served first. The low priority queue is served ONLY when all higher priority queues are empty.

---

### Q6. What does traffic policing do when a packet violates the QoS requirement?

A) Buffers the packet for later delivery  
B) Drops the packet or reduces its priority  
C) Increases the bandwidth for that flow  
D) Reshapes the traffic to constant rate  

**Answer: B**  
**Explanation:** Traffic policing monitors flows and if packets violate the QoS requirements (like exceeding the expected rate or delay), it drops those packets or reduces their priority (markdown).

---

### Q7. What is the key difference between traffic shaping and traffic policing?

A) Shaping drops packets; policing smooths traffic  
B) Shaping smooths bursty traffic to constant rate; policing drops violating packets  
C) Both do the same thing  
D) Shaping works at Layer 2; policing works at Layer 3  

**Answer: B**  
**Explanation:** Traffic shaping takes bursty input and produces smooth, constant-rate output. Traffic policing checks if flows violate the SLA and drops the excess packets. They are complementary mechanisms.

---

### Q8. What is a Service Level Agreement (SLA)?

A) A protocol used for routing  
B) An agreement between customer and service provider to maintain QoS of an application  
C) A type of scheduling algorithm  
D) A field in the IP header  

**Answer: B**  
**Explanation:** SLA is a contract between the customer and the service provider that defines the expected quality of service for an application. For example, a broadband package specifying 1 Mbps download speed is a form of SLA.

---

### Q9. In the traffic policing delay example from the transcript, if the end-to-end delay requirement is 10 ms and a packet has already experienced 9 ms delay at an intermediate router, what does the router do?

A) Forward the packet with best effort  
B) Buffer the packet and wait  
C) Drop the packet  
D) Increase the packet's priority  

**Answer: C**  
**Explanation:** The router knows it is impossible to deliver the packet to the destination within the remaining 1 ms. Since the QoS cannot be satisfied, the router drops the packet to save bandwidth and avoid unnecessary processing at the destination.

---

### Q10. Which IP Precedence value represents "Routine" (lowest priority) traffic?

A) 7  
B) 1  
C) 0  
D) 5  

**Answer: C**  
**Explanation:** IP Precedence 0 (binary 000) represents "Routine" traffic, which is the lowest priority. Precedence 7 (binary 111) represents "Network Control," which is the highest priority.

---

*End of Lecture 32 — Internet QoS II (Basic QoS Architecture)*
