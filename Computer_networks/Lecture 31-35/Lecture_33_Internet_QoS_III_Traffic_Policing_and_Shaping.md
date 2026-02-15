# Lecture 33: Internet QoS – III (Traffic Policing and Traffic Shaping)

**Course:** Computer Networks and Internet Protocol  
**Instructor:** Prof. Sandip Chakraborty, IIT Kharagpur  
**Topic:** Policing, Shaping, and Scheduling — Leaky Bucket & Token Bucket Algorithms

---

## Topics Covered in This Lecture

1. Traffic Policing vs Traffic Shaping (Recap & Combined View)
2. Leaky Bucket Algorithm
3. Leaky Bucket — How It Works (Queue Model)
4. Leaky Bucket — Output Rate Behavior
5. Leaky Bucket — Traffic Shaping + Policing Combined
6. Token Bucket Algorithm — Introduction
7. Token Bucket — How It Works
8. Key Difference: Leaky Bucket vs Token Bucket (Burst Support)
9. Token Bucket — Output Rate Formula
10. Token Bucket — Maximum Burst Size (MBS) Calculation
11. Leaky Bucket vs Token Bucket — Summary Comparison
12. Playout Buffer — Additional Smoothing

---

## Concept 1: Traffic Policing vs Traffic Shaping (Recap & Combined View)

### 📌 Concept Name
Traffic Policing vs Traffic Shaping — Why They Are Combined

### 🧠 Simple Explanation

In the previous lecture, the professor introduced these two ideas separately. Here is a quick recap and then the key new insight from this lecture:

**Traffic Policing** means you look at the incoming traffic and if any packets are exceeding (violating) the agreed-upon rate or quality of service, you simply **drop** those extra packets. Think of it like a strict security guard — if you don't have a pass, you are thrown out.

**Traffic Shaping** means you **smooth out** the traffic. Instead of allowing spikes and drops, you regulate the output to flow at a more constant rate. Think of it like a water tap — you control the flow so it comes out steadily.

**The key new point from this lecture:** In a real network, having a *perfectly* constant bit rate is very difficult because packets pass through multiple routers. So, in practice, we use mechanisms that **combine both policing and shaping together**. The mechanism controls the output rate (shaping) AND drops packets that exceed capacity (policing) — both happen at the same time.

### 🎯 Exam Important Points
- Traffic policing = cut/drop packets that violate QoS limits
- Traffic shaping = smooth out traffic to a constant or regulated rate
- In real networks, perfect constant bit rate is hard to achieve
- Practical mechanisms combine BOTH policing and shaping together
- The same algorithm (like leaky bucket) can do both jobs simultaneously

### ⚠️ Common Confusions
- Students often think policing and shaping are always separate — in this lecture, the professor clearly says the actual mechanism is a **combination** of both.
- Policing does NOT mean all traffic is dropped — only the **excess** traffic beyond the allowed limit is dropped.

### 📝 Possible NPTEL-style Questions
- "In a real network, traffic policing and traffic shaping are typically applied ____." → Answer: Together (combined)
- "Traffic policing drops packets while traffic shaping ____." → Answer: Smooths out the traffic rate

---

## Concept 2: Leaky Bucket Algorithm — Basic Idea

### 📌 Concept Name
Leaky Bucket Algorithm

### 🧠 Simple Explanation

The leaky bucket is the simplest mechanism for traffic regulation. To understand it, imagine a real bucket with a small hole at the bottom:

**Step 1:** You pour water (packets) into the bucket from the top. The water can come at any rate — sometimes fast, sometimes slow.

**Step 2:** From the small hole at the bottom, water leaks out at a **constant rate**. No matter how fast you pour water in, the hole only lets water out at a fixed speed.

**Step 3:** If you pour too much water and the bucket is already full, the extra water **overflows** and is lost.

Now replace "water" with "data packets":
- **Incoming packets** are put into a **packet queue** (the bucket)
- The queue has a **fixed capacity** (bucket size = τ, tau)
- A **single server** takes packets out of this queue at a **constant rate r** (packets per second or bits per second)
- If more packets arrive than the queue can hold, those **excess packets are dropped** (overflow)

### 🛠 Real-world Analogy (from transcript)
The professor uses the water bucket analogy directly — a bucket with a hole. Water pours in at varying rates, leaks out at a constant rate from the hole, and overflows if the bucket is full.

### 🎯 Exam Important Points
- Leaky bucket = single server queue with **constant service time**
- Output rate = constant rate **r** (maximum r packets/sec if there are packets; 0 if queue is empty)
- Queue capacity = **τ (tau)** — this is the bucket size
- If input exceeds τ → packets are **dropped** (this is the policing part)
- Output at constant rate → this is the **shaping** part
- Input rate can vary, but output rate remains constant

### ⚠️ Common Confusions
- The output rate r is the **maximum** output rate. If there are no packets in the queue, obviously the output is 0. So the output is "at most r, never more than r."
- The bucket size τ is NOT the output rate — it is the **maximum number of packets the queue can hold** at any instant.

### 📝 Possible NPTEL-style Questions
- "In a leaky bucket algorithm, the output rate is ____." → Answer: Constant (at most r)
- "What happens when the queue in a leaky bucket is full?" → Answer: Additional incoming packets are dropped

---

## Concept 3: Leaky Bucket — Output Rate Behavior in Practice

### 📌 Concept Name
Practical Output Behavior of Leaky Bucket

### 🧠 Simple Explanation

In theory, the leaky bucket should give a perfectly constant output rate (a flat horizontal line on a graph). But in practice, the output graph looks slightly different:

- When the application is **generating data continuously**, the output stays at the constant rate **r** — a flat line.
- When the application **stops generating data** (no new packets come), the output **drops to zero** or falls below r — because there's nothing left in the queue to send.
- When a **peak** of traffic arrives (many packets at once) and the queue becomes full, the **excess packets are dropped** — you see packet loss at those moments.

So the real output graph has:
- Flat portions at rate r (normal working)
- **Dips** when the application generates less data (drops below r)
- **Packet drops** when the queue overflows during peaks

The professor explains that this is slightly different from the "ideal" traffic shaping diagram (a perfectly flat line), but in a real network this is expected because the output depends on the input — if no packets come in, nothing goes out.

### 🎯 Exam Important Points
- Ideal output = perfectly constant rate r (flat line)
- Real output = mostly at rate r, but dips when application generates less data
- Packet drops happen when queue becomes full during traffic peaks
- The dips in output are because the **application stopped generating packets**, NOT because of a fault in the algorithm
- Leaky bucket applies **both** traffic shaping (constant rate output) AND traffic policing (dropping excess packets) together

### ⚠️ Common Confusions
- The output rate dipping below r does NOT mean the leaky bucket failed. It simply means the source stopped sending enough data.
- Packet drops happen only when the bucket is full — not every time there's a peak.

---

## Concept 4: Token Bucket Algorithm — Introduction

### 📌 Concept Name
Token Bucket Algorithm

### 🧠 Simple Explanation

The token bucket is a more flexible alternative to the leaky bucket. Its biggest advantage is that it **supports traffic bursts** — meaning it allows you to send a lot of data at once under certain conditions.

Here is how the token bucket works, step by step:

**Components:**
1. **Token Bucket** — a special bucket where **tokens** are generated and stored. Tokens are **virtual/logical** — not physical entities. They are just counters in the program.
2. **Packet Queue** — where incoming data packets wait.
3. **Scheduler** — the part that coordinates tokens and packets.

**How it works:**
- Tokens are generated at a constant rate: **r tokens per second**
- The token bucket has a maximum size: **b** (it can hold at most b tokens)
- Incoming packets are placed in the packet queue
- The scheduler checks: **Is there a token available in the bucket?**
  - If YES → take one token out, take one packet from the queue, and **send the packet out**
  - If NO → the packet must **wait** until a token is generated

**Key insight:** The tokens are like "permission slips." A packet can only be sent if it has a corresponding token. No token = no sending.

### 🎯 Exam Important Points
- Tokens are **virtual/logical** — they are not physical entities
- Token generation rate = r tokens/second
- Bucket size = b (maximum tokens that can be stored)
- A packet is sent ONLY if a token is available
- The output traffic rate is **bounded by** the token generation rate
- Token bucket supports **burstiness** (this is the key difference from leaky bucket)

### ⚠️ Common Confusions
- Tokens are NOT actual data — they are just logical counters that give "permission" to send a packet.
- The token bucket does NOT replace the packet queue — both exist separately.

---

## Concept 5: Key Difference — Leaky Bucket vs Token Bucket (Burst Support)

### 📌 Concept Name
Why Token Bucket Supports Bursts but Leaky Bucket Does Not

### 🧠 Simple Explanation

This is the **most important difference** and very likely to appear in exams:

**Leaky Bucket behavior:**
- Packets always go out **one by one** at a constant rate r
- Even if no packets arrived for a long time and the network was idle, when new packets arrive, they still go out one by one at rate r
- There is **no concept of burst** — you cannot send multiple packets at once
- Unused bandwidth in the past is **wasted** — you cannot use it later

**Token Bucket behavior:**
- When no packets are arriving, the tokens keep getting generated and **accumulate** in the token bucket (up to the maximum size b)
- Now, when a burst of packets suddenly arrives, you already have accumulated tokens
- You can use all those accumulated tokens to send **multiple packets at once** — this is called a **data burst** or **traffic burst**
- After the burst, you go back to the normal rate based on token generation

**Professor's example from transcript:** Suppose at some moment, 10 packets arrive and during the idle time, 6 tokens had accumulated in the token bucket. In this case, you can immediately send those 6 packets to the output right away (as a burst), rather than sending them one by one.

**Why is burst useful?** The professor gives the example of **buffered video streaming**. If you can send a burst of data to the play buffer, the video player gets sufficient data at once and can render/play the video smoothly. This is much better than sending data one packet at a time.

### 🎯 Exam Important Points
- Leaky bucket: **NO burst support** — always sends one by one at rate r
- Token bucket: **Supports burst** — accumulated tokens allow sending multiple packets at once
- During idle time (no incoming packets), tokens **accumulate** in the token bucket
- Maximum burst depends on how many tokens accumulated (up to bucket size b)
- After burst, output returns to normal regulated rate
- Burst is useful for applications like video streaming (fill play buffer quickly)

### ⚠️ Common Confusions
- In leaky bucket, even if the network was idle for 10 minutes, when packets come, they still go out at rate r one by one — there is NO accumulated "credit"
- In token bucket, the accumulated tokens ARE the "credit" — unused bandwidth from the past can be used now
- The burst size is **limited** by the token bucket size b — it cannot be infinite

### 📝 Possible NPTEL-style Questions
- "Which algorithm supports traffic burst?" → Token Bucket
- "In token bucket, what happens when no packets arrive?" → Tokens accumulate in the bucket
- "What is the advantage of token bucket over leaky bucket?" → It allows burst traffic while still regulating the average rate

---

## Concept 6: Token Bucket — Output Rate Formula

### 📌 Concept Name
Output Rate of Token Bucket: R(t) = min(Pt, b + rt)

### 🧠 Simple Explanation

The professor derives a formula for the output rate of the token bucket. Let's understand it simply:

**Variables:**
- **P** = incoming packet rate (how fast packets are arriving)
- **t** = time
- **Pt** = cumulative number of packets received up to time t
- **r** = token generation rate (tokens per second)
- **b** = token bucket size (initial number of tokens, assuming bucket starts full)
- **b + rt** = total number of tokens available up to time t (initial tokens + newly generated tokens)

**The formula:**
**Output rate R(t) = min(Pt, b + rt)**

This means:
- At any time t, the output is the **minimum** of two things:
  1. The number of packets that have arrived (Pt) — you cannot send more than what you received
  2. The number of tokens available (b + rt) — you cannot send more than the tokens allow

**When Pt < b + rt:** The incoming packet rate is less than the available tokens. So, the output equals the incoming rate (all packets go through). During this time, extra tokens are accumulating.

**When Pt > b + rt:** The incoming packet rate exceeds the available tokens. So, the output is limited to b + rt. The excess packets must wait.

**The crossover point** (where Pt = b + rt) is where the burst ends and the token bucket becomes the bottleneck.

### 🎯 Exam Important Points
- **Output = min(Pt, b + rt)** — this formula is very important for exams
- b = initial bucket size (bucket starts full)
- r = token generation rate
- When incoming rate < token availability → all packets pass through
- When incoming rate > token availability → output limited by tokens
- The crossover point determines the end of the burst period

### ⚠️ Common Confusions
- "b + rt" is NOT the output rate — it is the **cumulative number of tokens available** up to time t
- The assumption is that the token bucket starts **full** (b tokens initially)
- Pt represents **cumulative** packets, not instantaneous rate

---

## Concept 7: Token Bucket — Maximum Burst Size (MBS) Calculation

### 📌 Concept Name
Maximum Burst Size (MBS) Formula Derivation

### 🧠 Simple Explanation

This is a numerical/formula concept that the professor derives step by step. It is important for solving exam problems.

**Setup:**
- On a graph, X-axis = time, Y-axis = cumulative number of packets
- Two lines are drawn:
  1. **Y = Pt** (cumulative incoming packets — a straight line with slope P)
  2. **Y = b + rt** (cumulative tokens available — a straight line starting at b with slope r)
- These two lines **cross** at some time **t₁**

**Finding t₁ (the crossover time):**
At the crossover point, Pt₁ = b + rt₁

Solving for t₁:
- Pt₁ - rt₁ = b
- t₁(P - r) = b
- **t₁ = b / (P - r)**

**Finding Maximum Burst Size (MBS):**
Now substitute t₁ back into either equation (let's use Y = Pt):
- MBS = P × t₁
- MBS = P × b / (P - r)
- **MBS = Pb / (P - r)**

**What this means:** The maximum burst size tells you the maximum number of packets that can be sent in a burst before the token bucket runs out and you have to slow down to the regular token generation rate.

### 🎯 Exam Important Points
- **t₁ = b / (P - r)** — time at which burst ends
- **MBS = Pb / (P - r)** — Maximum Burst Size formula
- This formula assumes the token bucket is initially full (b tokens)
- P must be greater than r for burst to happen (if P ≤ r, there is no burst because tokens are always enough)
- After time t₁, the output follows the token generation rate r

### ⚠️ Common Confusions
- P is the **incoming packet rate** (not the output rate)
- r is the **token generation rate** (not the incoming rate)
- For MBS formula to work, **P > r** must hold (incoming rate must exceed token rate)
- MBS is measured in **packets** (or bits, depending on units used)
- t₁ is when the burst **ends**, not when it starts — the burst starts at time 0

### 📝 Possible NPTEL-style Questions
- "Given token generation rate r, bucket size b, and incoming rate P, what is the maximum burst size?" → Pb/(P-r)
- "At what time does the burst end in a token bucket?" → t₁ = b/(P-r)

---

## Concept 8: Leaky Bucket vs Token Bucket — Summary Comparison

### 📌 Concept Name
Complete Comparison: Leaky Bucket vs Token Bucket

### 🧠 Simple Explanation

| Feature | Leaky Bucket | Token Bucket |
|---|---|---|
| **Basic action** | Smooths out traffic | Smooths out traffic AND permits bursts |
| **Burst support** | NO — does not permit burstiness | YES — permits burstiness |
| **How output works** | Packets go out one by one at constant rate r | Packets can go out in burst if tokens accumulated |
| **When no packets arrive** | Nothing happens (no output, no accumulation) | Tokens keep accumulating in the bucket |
| **Unused bandwidth** | Wasted — cannot be used later | Saved as tokens — can be used later for burst |
| **Output rate** | Maximum r (constant) | Can exceed r temporarily during burst, then settles to r |
| **Queue/Bucket** | Single packet queue with capacity τ | Token bucket (size b) + separate packet queue |
| **Use case** | Strict constant rate applications | Applications needing burst (e.g., video streaming) |
| **Traffic policing** | Drops packets when queue full | Limits output to token availability |
| **Traffic shaping** | Constant rate output | Regulated output with burst allowance |

**Key statement from the transcript:** "Leaky bucket smooths out traffic but does not permit burstiness. Token bucket smooths out traffic and also permits burstiness — if there is no incoming packet, tokens are get added in the token bucket, and the burst traffic is permitted up to the amount of token accumulated."

### 🎯 Exam Important Points
- Both can be used for traffic shaping
- Leaky bucket = strict, constant output, no burst
- Token bucket = flexible, allows burst, tokens accumulate during idle time
- Both are mechanisms that combine policing and shaping
- The amount of burst in token bucket = amount of tokens accumulated (limited by bucket size b)

---

## Concept 9: Playout Buffer — Additional Smoothing

### 📌 Concept Name
Playout Buffer for Complete Traffic Smoothing

### 🧠 Simple Explanation

Even with leaky bucket or token bucket, the output rate can sometimes **dip below** the expected average rate r. This happens when the application generates less data than expected. In such cases, we see "dips" in the output graph.

If you need a **perfectly smooth** constant rate (for very strict applications), you can add an **additional buffer** called a **playout buffer** in front of the traffic shaper.

**What does the playout buffer do?**
- It introduces **additional delay** to packets that arrived **too early**
- Some packets arrive faster than needed. Instead of sending them immediately, the playout buffer holds them and releases them at the right time
- This way, the output becomes smoother and closer to the ideal constant rate

**Why is it needed?**
The professor explains that the dips in the output happen because some packets came faster (they arrived early), creating uneven output. By adding a small delay to those early packets, you can even out the flow.

### 🎯 Exam Important Points
- Playout buffer = an additional buffer added in front of the traffic shaper
- Purpose: introduce **additional delay** to packets that arrived too early
- Result: smoother, more constant output rate
- Used for very strict applications where constant rate is mandatory
- Both leaky bucket and token bucket can use a playout buffer for better smoothing

### ⚠️ Common Confusions
- The playout buffer is NOT part of the leaky bucket or token bucket itself — it is an **additional** component added separately
- The playout buffer adds **delay** — it does NOT drop packets
- It is used only when very strict smoothing is needed, not always

### 📝 Possible NPTEL-style Questions
- "What is the purpose of a playout buffer?" → To introduce additional delay to packets that arrived early, making the output smoother
- "A playout buffer is added to ____." → In front of the traffic shaper

---

## Concept 10: What Comes Next (Preview)

### 📌 Concept Name
Next Lecture Preview — Traffic Scheduling Algorithms

### 🧠 Simple Explanation

At the end of this lecture, the professor mentions that the next class will cover **traffic scheduling algorithms** using different queuing mechanisms. These include:
- Priority Queuing
- Weighted Fair Queuing
- Custom Queuing

These are NOT part of Lecture 33 — they will be covered in the next lecture. This is just a preview so you know what's coming.

---

## Quick Revision Table

| Concept | Key Formula / Point |
|---|---|
| Leaky Bucket | Output = constant rate r; drops packets when queue (size τ) is full |
| Token Bucket | Output = min(Pt, b + rt); supports burst |
| Token generation rate | r tokens/second |
| Token bucket size | b |
| Maximum Burst Size | MBS = Pb / (P - r) |
| Burst end time | t₁ = b / (P - r) |
| Playout Buffer | Adds delay to early packets for smoother output |
| Leaky vs Token | Leaky = no burst; Token = burst allowed |

---

## 10 MCQs — Strictly from Lecture 33

### Q1. What does the leaky bucket algorithm produce at its output?

(a) Variable rate traffic  
(b) Burst traffic  
(c) Constant rate traffic (at most rate r)  
(d) No traffic at all  

**Answer: (c)**  
**Explanation:** The leaky bucket uses a single server queue with constant service time. The output is always at a constant rate r (or less if no packets are in the queue). This is explicitly stated in the transcript.

---

### Q2. In a leaky bucket, what happens when the packet queue (bucket) becomes full?

(a) Packets are buffered elsewhere  
(b) The output rate increases  
(c) Additional incoming packets are dropped  
(d) Tokens are generated  

**Answer: (c)**  
**Explanation:** The transcript clearly states: "If the bucket overflows, the packets are discarded." This is the traffic policing aspect of the leaky bucket.

---

### Q3. What is the key advantage of the token bucket algorithm over the leaky bucket algorithm?

(a) It uses less memory  
(b) It supports traffic burst  
(c) It has a faster output rate  
(d) It does not require a queue  

**Answer: (b)**  
**Explanation:** The main difference stated in the transcript is that token bucket "supports something called traffic burst." Leaky bucket does not permit burstiness, but token bucket does.

---

### Q4. In the token bucket algorithm, tokens are best described as:

(a) Physical hardware components  
(b) Data packets  
(c) Virtual/logical entities (counters in software)  
(d) Special header fields in packets  

**Answer: (c)**  
**Explanation:** The professor explicitly says: "These tokens are kind of logical token, it is not a kind of physical entity." They are virtual counters implemented in the program.

---

### Q5. What is the output rate formula for the token bucket algorithm?

(a) R(t) = Pt + b + rt  
(b) R(t) = max(Pt, b + rt)  
(c) R(t) = min(Pt, b + rt)  
(d) R(t) = Pt - rt  

**Answer: (c)**  
**Explanation:** The transcript gives the formula as "Output rate R(t) = min(Pt, b + rt)" where Pt is the incoming packet rate and b + rt represents the cumulative token availability.

---

### Q6. What is the Maximum Burst Size (MBS) in a token bucket algorithm given incoming packet rate P, token generation rate r, and bucket size b?

(a) P(b + r)  
(b) Pb / (P + r)  
(c) b / (P - r)  
(d) Pb / (P - r)  

**Answer: (d)**  
**Explanation:** The transcript derives MBS = Pb / (P - r). The time at which burst ends is t₁ = b/(P-r), and MBS = P × t₁ = Pb/(P-r).

---

### Q7. In the token bucket algorithm, what happens when there are no incoming packets?

(a) The output rate increases  
(b) Tokens keep getting added/accumulated in the token bucket  
(c) The bucket shrinks  
(d) The algorithm stops working  

**Answer: (b)**  
**Explanation:** The transcript states: "if there is no incoming packet here, then the tokens are getting added right." Tokens accumulate during idle periods, enabling future burst.

---

### Q8. A playout buffer is used to:

(a) Drop packets that arrive late  
(b) Increase the output rate  
(c) Add additional delay to packets that arrived too early  
(d) Generate tokens faster  

**Answer: (c)**  
**Explanation:** The transcript defines the playout buffer as "a buffer to add some additional delay to the packets that arrived too early." Its purpose is to smooth out the output further.

---

### Q9. Which of the following is TRUE about the leaky bucket algorithm?

(a) It allows traffic bursts  
(b) Unused bandwidth is saved for later use  
(c) Packets are always sent one by one at constant rate  
(d) It uses tokens to regulate traffic  

**Answer: (c)**  
**Explanation:** In leaky bucket, there is no concept of burst. The transcript says "in case of leaky bucket you have to always send the packets one by one." Unused bandwidth is wasted, not saved.

---

### Q10. In practice, having a perfect constant bit rate guarantee in a network is:

(a) Very easy to achieve  
(b) Achieved using token bucket alone  
(c) Very difficult because packets pass through multiple routers  
(d) Not required for any application  

**Answer: (c)**  
**Explanation:** The transcript states: "in a typical network having perfect guarantee of a constant bit rate is very difficult because the packets are going via multiple routers one after another." This is why approximation through combined policing and shaping is used.

---

*End of Lecture 33 Study Guide*
