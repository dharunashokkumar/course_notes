# Lecture 15: Transport Layer – V (Sliding Window Protocols)

**Course:** Computer Networks and Internet Protocol  
**Professor:** Prof. Sandip Chakraborty, IIT Kharagpur  

---

## Topics Covered in This Lecture

1. Problem with Stop and Wait Protocol (Recap)
2. Introduction to Sliding Window Protocols (Pipelining)
3. Sending Window and Receiving Window
4. Sliding Window with 3-bit Sequence Number (Example)
5. Window Size = 1 is same as Stop and Wait
6. How Sliding Window Protocol Works in Noisy Channels
7. Go Back N ARQ — Concept
8. Go Back N ARQ — Sender Window Control (base pointer, next sequence number, window size)
9. Go Back N ARQ — Working Example in Noisy Channel
10. Go Back N ARQ — Sender Side Implementation (FSM)
11. Go Back N ARQ — Receiver Side Implementation (FSM)
12. Go Back N ARQ — Bound on Window Size
13. Go Back N ARQ — Why Window Size = MAX_SEQ (not MAX_SEQ + 1) — with Example
14. Selective Repeat (SR) ARQ — Concept
15. Selective Repeat ARQ — Window Control (Sender & Receiver)
16. Selective Repeat ARQ — Working Example with Cumulative Acknowledgement and NAK
17. Selective Repeat ARQ — Bound on Window Size
18. Selective Repeat ARQ — Why Window Size = (MAX_SEQ + 1) / 2 — with Example

---

## Concept 1: Problem with Stop and Wait Protocol (Recap)

📌 **Concept Name:** Why we need Sliding Window

🧠 **Simple Explanation:**

In the previous lecture, you learned about the Stop and Wait protocol. In Stop and Wait, the sender sends **one packet** and then **waits** for the acknowledgement (ACK) before sending the next packet. This means at any time, there is only **1 packet outstanding** in the network.

The problem? You are **not using the full capacity** of the link. The link could carry more data, but you are sitting idle waiting for ACK most of the time. This wastes bandwidth.

🛠 **Real-world Example:**

Imagine you are passing letters one by one to a friend across a river using a boat. You send one letter, wait for the boat to come back with confirmation, then send the next. Very slow! What if you could send 5 letters at once on the boat?

🎯 **Exam Important Points:**
- Stop and Wait allows only 1 packet outstanding at a time
- It does not utilize the full capacity of the link
- This is the motivation for sliding window protocols

⚠️ **Common Confusion:**
- Students confuse "outstanding" with "total packets sent." Outstanding means sent but NOT yet acknowledged.

---

## Concept 2: Introduction to Sliding Window Protocols (Pipelining)

📌 **Concept Name:** Sliding Window Protocol — Basic Idea

🧠 **Simple Explanation:**

Sliding Window Protocol is a **pipelined** version of the flow control protocol. Instead of sending 1 packet and waiting, you can send **multiple packets all together** in a pipeline fashion. You send multiple packets without waiting for each individual acknowledgement.

How does it work? You start by sending packets. As you receive acknowledgements, you **slide** your window forward and can send more packets. The window keeps sliding — that is why it is called "Sliding Window."

At the same time, you receive acknowledgements in parallel and accordingly control your transmission rate.

🎯 **Exam Important Points:**
- Sliding window = pipelining (send multiple packets without waiting for ACK of each one)
- The window "slides" forward as ACKs are received
- It improves link utilization compared to Stop and Wait

⚠️ **Common Confusion:**
- Sliding window does NOT mean you can send unlimited packets. You can only send as many as your window size allows.

---

## Concept 3: Sending Window and Receiving Window

📌 **Concept Name:** Sending Window & Receiving Window

🧠 **Simple Explanation:**

Every outbound segment in the network contains a **sequence number**. The sequence number goes from **0 to some maximum value**. If you are using an **n-bit sequence number**, the sequence number space is from **0 to 2ⁿ - 1**.

Now there are two important windows:

**Sending Window:** The sender maintains a set of sequence numbers corresponding to the frames it is **permitted to send**. This is the sending window. It tells you: "You can send these frames without waiting for ACK."

**Receiving Window:** The receiver maintains a set of frames it is **permitted to accept**. This is the receiving window. It tells you: "I can accept these frames right now."

How the sliding works:
- The sender sends frames within its sending window
- When an ACK comes back for a frame, the sender **shifts (slides)** its window to the right by 1
- Similarly, when the receiver accepts a frame and sends ACK, the receiver also **shifts its window**

**Example from transcript:** Sender window covers frames 0 to 6. Sender sends frame 0, 1, 2 in parallel. When ACK for frame 0 arrives, the sending window shifts from {0-6} to {1-7}. When ACK for frame 1 arrives, it shifts again to {2-7, 0} (because sequence numbers repeat in circular fashion).

🎯 **Exam Important Points:**
- Sending window = set of frames sender can send without waiting for ACK
- Receiving window = set of frames receiver can accept
- When ACK arrives, window slides forward by 1
- Sequence numbers wrap around (circular fashion) — after the maximum, it goes back to 0
- If all frames in the sending window are sent and no ACK received, sender **cannot send** any more frames until an ACK comes

⚠️ **Common Confusion:**
- Students forget that sequence numbers are **circular** — they wrap around after the maximum
- The window slides only when ACK is received, not just when frames are sent

---

## Concept 4: Sliding Window with 3-bit Sequence Number (Example)

📌 **Concept Name:** 3-bit Sequence Number Example

🧠 **Simple Explanation:**

If you have a **3-bit sequence number**, your sequence number space is from **0 to 2³ - 1 = 7**. So you have 8 sequence numbers: 0, 1, 2, 3, 4, 5, 6, 7.

These are arranged in a **circular queue**. After sequence number 7, the next frame gets sequence number 0 again.

Now consider **window size = 1**:
- Sender sends frame 0, then it is blocked (cannot send more because window size is 1)
- Receiver is expecting frame 0
- Once receiver gets frame 0, it sends ACK, and now expects frame 1
- Once sender gets ACK for frame 0, it can now send frame 1
- This behaves **exactly like Stop and Wait!**

**Key insight:** A sliding window protocol with **window size = 1** is the same as Stop and Wait protocol.

Now if you increase window size to 2:
- Sender can send 2 frames in parallel without waiting for ACK
- Once both are sent, sender waits for ACK
- When ACK comes, window slides forward and sender can send the next frame

This is where you get the feel of **parallelism** — and that is the power of sliding window.

🎯 **Exam Important Points:**
- 3-bit sequence number → sequence numbers 0 to 7 (total 8 numbers)
- n-bit sequence number → 2ⁿ sequence numbers (0 to 2ⁿ - 1)
- Sequence numbers used in circular fashion
- Window size = 1 → same as Stop and Wait
- Larger window size → more parallelism → better link utilization

⚠️ **Common Confusion:**
- 3-bit sequence number gives 8 distinct numbers (0 to 7), but the **window size is NOT always 8**. Window size depends on the protocol (Go Back N or Selective Repeat).

---

## Concept 5: Sliding Window Protocols in Noisy Channels

📌 **Concept Name:** Handling Loss — Timeout Mechanism

🧠 **Simple Explanation:**

In a noisy channel, packets can get **lost** or acknowledgements can get **lost**. Similar to Stop and Wait, the sliding window protocol uses a **timeout mechanism**.

If you have sent all the frames in your sending window and you are waiting for acknowledgement but it does not arrive, a **timeout** occurs.

The question now is: **which frames do you retransmit?**

There are **two different mechanisms** to handle this:

1. **Go Back N ARQ** — If a timeout happens, retransmit **ALL** the frames from the start of the current sliding window up to the lost frame. Basically, retransmit everything in your current window.

2. **Selective Repeat (SR) ARQ** — Only retransmit the **specific lost packet**. Do not retransmit the ones that were received correctly.

For Selective Repeat to work, the receiver needs to tell the sender **which specific packet was lost**. This is done using:
- **NAK (Negative Acknowledgement)** — tells sender which packet needs retransmission
- **SACK (Selective Acknowledgement)** — used in TCP, tells sender which packets need to be retransmitted

🎯 **Exam Important Points:**
- Noisy channel → packets or ACKs can get lost → timeout occurs
- Two mechanisms: Go Back N ARQ and Selective Repeat ARQ
- Go Back N = retransmit ALL frames in current window
- Selective Repeat = retransmit ONLY the lost frame
- NAK / SACK helps identify which packets were lost in Selective Repeat

⚠️ **Common Confusion:**
- In Go Back N, you retransmit ALL frames in the window, even those that the receiver may have already received correctly. This is wasteful but simpler.
- In Selective Repeat, the receiver must be able to accept **out-of-order** frames and buffer them.

---

## Concept 6: Go Back N ARQ — Sender Window Control

📌 **Concept Name:** Go Back N ARQ Sender Side

🧠 **Simple Explanation:**

In Go Back N ARQ, the sender maintains **three important parameters**:

1. **Base Pointer:** Points to the **start** of the current window. All frames before the base have already been acknowledged. So base pointer tells you: "Everything before me is done."

2. **Next Sequence Number:** Points to the frame which you can **send next** without waiting for ACK. All frames from base up to (next sequence number - 1) have been **sent but not yet acknowledged** (they are "outstanding"). The next sequence number itself is the frame you can still send.

3. **Window Size (N):** This is the maximum number of frames you can have outstanding (sent but not yet acknowledged) at any time.

**How it works:**
- You keep sending frames using the next sequence number
- Each time you send, you increment the next sequence number
- If next sequence number reaches **base + window size**, you CANNOT send more — your window is full. You must wait for ACK.
- When an ACK arrives, the base pointer moves forward, which opens up room to send more frames

**Condition to stop sending:**
If next sequence number = base + N (window size), you have transmitted all frames in your window. No more sending until ACK comes.

🎯 **Exam Important Points:**
- Three parameters: base, next sequence number, window size N
- Frames before base = already ACKed
- Frames from base to (next seq num - 1) = sent, waiting for ACK
- Next seq num = frame to send next
- If next seq num ≥ base + N → window full, cannot send more
- ACK received → base moves forward → window slides

⚠️ **Common Confusion:**
- "Next sequence number" is NOT the same as "base." Base is the start of the window; next sequence number is the next frame to be sent.

---

## Concept 7: Go Back N ARQ — Working in Noisy Channel (Example)

📌 **Concept Name:** Go Back N in Action with Window Size = 3

🧠 **Simple Explanation:**

Let's say window size = 3:

**Step 1:** Sender sends frame 0, frame 1 (parallel, within window)
**Step 2:** Receiver gets frame 0, sends ACK 0
**Step 3:** Sender receives ACK 0 → resets timer for 0 → window slides → can now send frame 3 (because window was {0,1,2} and now becomes {1,2,3})
**Step 4:** Sender sends frame 2, frame 3
**Step 5:** Receiver sends ACK 1 → sender receives it → resets timer for 1 → sends frame 4

**Now what if ACK gets lost?**
- Say sender has sent frames 2, 3, 4 and is waiting for ACKs
- ACK for frame 2 gets lost in the channel
- Sender keeps waiting... window is full (size 3, frames 2, 3, 4 are outstanding)
- After some time, **timeout for frame 2 occurs**
- In Go Back N: sender retransmits **ALL frames in current window** → retransmit frames 2, 3, and 4
- Receiver gets them, sends ACKs for 2, 3, 4
- Sender receives these ACKs → window shifts to {5, 6, 7}

**Key idea:** On timeout, Go Back N retransmits **ALL** frames that were in the current window, not just the lost one.

🎯 **Exam Important Points:**
- On timeout, Go Back N retransmits ALL frames from base to (next seq num - 1)
- Each frame has its own timer
- When ACK received → reset timer for that frame
- When timeout → retransmit all outstanding frames

---

## Concept 8: Go Back N ARQ — Sender Side Implementation (FSM)

📌 **Concept Name:** Go Back N Sender FSM (Finite State Machine)

🧠 **Simple Explanation:**

Initially, the sender is in the **waiting state** with base = 1 and next sequence number = 1.

**When application wants to send data (rdt_send call from upper layer):**
- Check: Is next sequence number < base + N?
- YES → You have room in window → Construct packet (append sequence number + data + checksum) → Send via unreliable channel → Start timer → Increment next sequence number
- NO → Window is full → Refuse the data (cannot send right now)

**When timeout occurs:**
- Start the timer again
- Retransmit ALL packets from base to (next sequence number - 1)
- That means retransmit all frames currently in the window

**When ACK is received (not corrupted):**
- Check the acknowledgement number
- Update base pointer to (ACK number + 1)
- This means the window slides forward
- If base = next sequence number → no outstanding frames → go back to waiting
- If packet is corrupted → ignore it, stay in wait loop

🎯 **Exam Important Points:**
- Send condition: next seq num < base + N
- On timeout: retransmit all frames from base to next seq num - 1
- On ACK: base = ACK number + 1 (window slides)
- Corrupted packet → ignore

---

## Concept 9: Go Back N ARQ — Receiver Side Implementation (FSM)

📌 **Concept Name:** Go Back N Receiver FSM

🧠 **Simple Explanation:**

The receiver side for Go Back N is **simple**. The receiver starts with expected sequence number = 1.

**When a packet arrives:**
- Check: Is it NOT corrupted AND has the expected sequence number?
- YES → Extract data → Deliver to application → Send ACK with expected sequence number → Increment expected sequence number
- NO (default case — corrupted or unexpected sequence number) → Just send ACK for the last correctly received frame → Stay in wait state

**Key point:** The receiver in Go Back N does NOT accept out-of-order frames. If it is expecting frame 3 and receives frame 5, it will discard frame 5 and re-send ACK for whatever it last received correctly.

🎯 **Exam Important Points:**
- Receiver only accepts frames in order
- Out-of-order frames are discarded
- Receiver always sends ACK for last correctly received frame
- Receiver is simple — no buffering of out-of-order frames needed

⚠️ **Common Confusion:**
- In Go Back N, the **receiver does NOT buffer out-of-order frames**. It simply discards them. This is different from Selective Repeat.

---

## Concept 10: Go Back N ARQ — Bound on Window Size

📌 **Concept Name:** Window Size Limit in Go Back N

🧠 **Simple Explanation:**

This is a very important concept for exams.

**Definitions:**
- **Outstanding Frames** = Frames that have been transmitted but not yet acknowledged
- **MAX_SEQ** = Maximum sequence number. If you use n-bit sequence number, MAX_SEQ = 2ⁿ - 1
- You have (MAX_SEQ + 1) distinct sequence numbers: 0, 1, 2, ..., MAX_SEQ

**The rule for Go Back N:**

**Window Size (W) = MAX_SEQ = 2ⁿ - 1**

That means the window size is **one less** than the total number of distinct sequence numbers.

**Example:**
- 3-bit sequence number → sequence numbers are 0, 1, 2, 3, 4, 5, 6, 7
- MAX_SEQ = 7
- Total distinct sequence numbers = 8
- **Window size = 7 (NOT 8!)**

**But WHY not 8?** Why can't window size be equal to the total number of distinct sequence numbers (MAX_SEQ + 1)?

🎯 **Exam Important Points:**
- For Go Back N: **W = 2ⁿ - 1** (where n = number of bits for sequence number)
- Window size = MAX_SEQ, NOT MAX_SEQ + 1
- This is one less than the total sequence number space

---

## Concept 11: Go Back N — Why Window Size ≠ MAX_SEQ + 1 (Critical Example)

📌 **Concept Name:** Why Window Size Must Be 2ⁿ - 1 (Not 2ⁿ) in Go Back N

🧠 **Simple Explanation:**

This is explained through an example in the transcript. Let's understand it carefully.

**Scenario 1: MAX_SEQ = 3, Window Size = 4 (WRONG — Window = MAX_SEQ + 1)**

Sequence numbers: 0, 1, 2, 3 (4 distinct numbers)
Window size: 4

What happens:
- Sender sends frames 0, 1, 2, 3 (entire window)
- Receiver correctly receives all 4 frames
- Receiver sends ACK for each frame
- **But ALL acknowledgements get lost in the channel!**
- Receiver has shifted its window and is now expecting frame 0 (because after 3, it wraps back to 0 — it is expecting the **next** group of frames starting from 0)
- Sender gets a timeout (no ACKs received)
- Sender retransmits frame 0 (the OLD frame 0, not a new one)
- **Problem:** The receiver is also expecting frame 0 — but it thinks this is the NEW frame 0 (next round)!
- The receiver **cannot tell the difference** between the old frame 0 (retransmission) and the new frame 0 (next group)
- **This creates confusion and errors!**

**Scenario 2: MAX_SEQ = 3, Window Size = 3 (CORRECT — Window = MAX_SEQ)**

Sequence numbers: 0, 1, 2, 3 (4 distinct numbers)
Window size: 3

What happens:
- Sender sends frames 0, 1, 2 (only 3, not 4)
- Receiver receives all 3, sends ACKs — all ACKs get lost
- Receiver is now expecting frame 3 (NOT frame 0!)
- Sender gets timeout, retransmits frames 0, 1, 2
- Receiver gets frame 0 — but it is expecting frame 3
- **Receiver correctly identifies** that frame 0 is NOT what it expected → discards it → sends ACK saying "I already have up to frame 2, I need frame 3"
- **No confusion!** The receiver can correctly distinguish between retransmissions and new frames.

**Conclusion:** By keeping window size = MAX_SEQ (one less than total sequence numbers), the receiver always has at least one "gap" sequence number that helps it tell apart retransmitted frames from new frames.

🎯 **Exam Important Points:**
- If window size = MAX_SEQ + 1 → receiver cannot distinguish between retransmission and new frame when all ACKs are lost
- If window size = MAX_SEQ → receiver CAN correctly distinguish → no confusion
- **Formula: Go Back N Window Size = 2ⁿ - 1**

⚠️ **Common Confusion:**
- This is one of the most asked exam questions! Students often think window size = total sequence numbers. Remember: it is **one less**.

📝 **Possible Exam Question:** "If 3-bit sequence number is used, what is the maximum window size for Go Back N?" Answer: **7** (not 8).

---

## Concept 12: Selective Repeat (SR) ARQ — Concept

📌 **Concept Name:** Selective Repeat ARQ

🧠 **Simple Explanation:**

In Go Back N, when a timeout happens, you retransmit ALL frames in the window. This is wasteful — many of those frames may have already been received correctly.

Selective Repeat ARQ solves this problem. In Selective Repeat:
- You **only retransmit the specific lost packet**
- You do NOT retransmit frames that were received correctly
- This is more efficient than Go Back N

But how does the sender know WHICH specific packet was lost? The receiver uses:
- **NAK (Negative Acknowledgement):** The receiver sends a NAK telling the sender exactly which packet it has not received
- **SACK (Selective Acknowledgement):** Used in TCP, it tells the sender which packets need retransmission

**Key difference from Go Back N:** In Selective Repeat, the **receiver CAN accept out-of-order frames** and store them in a buffer. In Go Back N, out-of-order frames are simply discarded.

🎯 **Exam Important Points:**
- Selective Repeat retransmits ONLY the lost frame (not all)
- Uses NAK or SACK to identify lost frames
- Receiver can accept out-of-order frames (buffers them)
- More efficient than Go Back N but more complex

⚠️ **Common Confusion:**
- In Go Back N, receiver discards out-of-order frames. In Selective Repeat, receiver **buffers** them.

---

## Concept 13: Selective Repeat ARQ — Window Control (Sender & Receiver)

📌 **Concept Name:** SR Window Control

🧠 **Simple Explanation:**

In Selective Repeat, **BOTH sender and receiver have windows**.

**Sender Side:**
- Similar to Go Back N — has send_base, next sequence number
- Some intermediate frames may be acknowledged while earlier ones are not
- The sender window can have "holes" — some frames acknowledged, some not

**Receiver Side:**
- The receiver also maintains a window with a **base pointer**
- The base pointer indicates the next expected frame
- If the receiver gets an out-of-order frame (say it got frame 3 but not frame 2), it puts frame 3 in the **buffer** and sends ACK for frame 3
- Frame 2 is still missing — receiver uses NAK to inform sender
- Sender retransmits frame 2 only

**Cumulative Acknowledgement:** In the transcript, the concept of cumulative acknowledgement is discussed. If you receive ACK 2, it means frames 0 and 1 have both been correctly received, and the receiver is now expecting frame 2.

🎯 **Exam Important Points:**
- Both sender and receiver maintain windows in Selective Repeat
- Receiver can accept out-of-order frames and buffer them
- Receiver sends individual ACK for each frame
- NAK tells sender which specific frame to retransmit
- Cumulative ACK: ACK N means all frames before N are received

---

## Concept 14: Selective Repeat ARQ — Working Example

📌 **Concept Name:** Selective Repeat in Action

🧠 **Simple Explanation:**

Here is how Selective Repeat works step-by-step:

**Step 1:** Sender transmits frame 0, then frame 1
**Step 2:** Receiver gets both frames 0 and 1. Sends **cumulative acknowledgement 2** (meaning: "I got 0 and 1, now send me frame 2")
**Step 3:** Sender gets ACK 2, shifts window forward
**Step 4:** Sender transmits frame 2, but **frame 2 gets lost** in the channel
**Step 5:** Sender transmits frame 3
**Step 6:** Receiver gets frame 3 but NOT frame 2 → frame 3 is **out of order**
**Step 7:** Receiver puts frame 3 in buffer and sends a **NAK (Negative Acknowledgement)** for frame 2 → "I didn't get frame 2, please resend it"
**Step 8:** Sender gets NAK → retransmits ONLY frame 2 (NOT frame 3)
**Step 9:** Receiver gets frame 2 → now it has both 2 and 3 (3 was in buffer) → delivers both to application

**Key takeaway:** Only the lost frame (frame 2) was retransmitted. Frame 3 was NOT retransmitted because it was already received and buffered.

🎯 **Exam Important Points:**
- Out-of-order frames are buffered at receiver
- NAK is sent for missing frames
- Only the missing frame is retransmitted
- Cumulative ACK is used

---

## Concept 15: Selective Repeat — Bound on Window Size

📌 **Concept Name:** Window Size Limit in Selective Repeat

🧠 **Simple Explanation:**

For Selective Repeat, the window size formula is different from Go Back N.

**The rule for Selective Repeat:**

**Window Size (W) = (MAX_SEQ + 1) / 2 = 2ⁿ / 2 = 2ⁿ⁻¹**

That means the window size is **half** of the total sequence number space.

**Example:**
- 3-bit sequence number → sequence numbers 0 to 7 (total 8)
- MAX_SEQ = 7
- Window size = (7 + 1) / 2 = **4**

Compare with Go Back N for the same 3-bit sequence:
- Go Back N window size = 7
- Selective Repeat window size = 4

🎯 **Exam Important Points:**
- For Selective Repeat: **W = 2ⁿ⁻¹** (half the sequence number space)
- For 3-bit: W = 4
- This is smaller than Go Back N window size

---

## Concept 16: Selective Repeat — Why Window Size = (MAX_SEQ + 1)/2 (Critical Example)

📌 **Concept Name:** Why Window Size Must Be Half the Sequence Space in Selective Repeat

🧠 **Simple Explanation:**

This is proven by example, similar to the Go Back N proof.

**Scenario 1: MAX_SEQ = 3, Window Size = 3 (WRONG — too large)**

Sequence numbers: 0, 1, 2, 3
Correct window size should be (3+1)/2 = 2, but we are using 3 (which is wrong).

What happens:
- Sender sends frames 0, 1, 2
- Receiver gets all three correctly
- **All ACKs get lost!**
- Receiver has shifted its window and is now expecting frames **3, 0, 1** (remember, Selective Repeat receiver can accept out-of-order frames!)
- Sender gets timeout → retransmits frame 0
- **Problem:** Receiver is expecting frame 0 as the NEW frame 0 (next cycle). But sender is sending the OLD frame 0 (retransmission).
- Receiver **cannot tell the difference** → accepts wrong data → **Confusion!**

**Scenario 2: MAX_SEQ = 3, Window Size = 2 (CORRECT)**

Sequence numbers: 0, 1, 2, 3
Window size = (3+1)/2 = 2

What happens:
- Sender sends frames 0, 1 (window size = 2)
- Receiver gets both correctly
- Receiver is now expecting frames **2 and 3** (its window shifted)
- All ACKs get lost
- Sender times out → retransmits frame 0
- Receiver is expecting frame 2 and 3, NOT frame 0 → **correctly identifies** frame 0 as old/wrong frame → discards it
- **No confusion!**

**Why does half the space work?** Because with window size = half, the sender's window and receiver's window NEVER overlap after the receiver shifts. This means the receiver can always tell if a retransmitted frame is from the old group or the new group.

🎯 **Exam Important Points:**
- If window size > (MAX_SEQ + 1)/2 → confusion when all ACKs are lost
- If window size = (MAX_SEQ + 1)/2 → no confusion
- **Formula: Selective Repeat Window Size = 2ⁿ⁻¹**
- The reason: receiver can accept out-of-order frames, so the window size must be smaller to avoid overlap

⚠️ **Common Confusion:**
- Students mix up Go Back N and Selective Repeat window size formulas. Remember:
  - **Go Back N: W = 2ⁿ - 1**
  - **Selective Repeat: W = 2ⁿ⁻¹**

---

## Summary Comparison Table

| Feature | Go Back N ARQ | Selective Repeat ARQ |
|---------|--------------|---------------------|
| On timeout | Retransmit ALL frames in window | Retransmit ONLY lost frame |
| Receiver accepts out-of-order? | NO (discards them) | YES (buffers them) |
| Receiver complexity | Simple | Complex (needs buffer) |
| Window Size formula | W = 2ⁿ - 1 | W = 2ⁿ⁻¹ |
| 3-bit example window size | 7 | 4 |
| Uses NAK/SACK? | No | Yes |
| Bandwidth efficiency | Less (retransmits correct frames too) | More (only retransmits lost frames) |

---

## Key Formulas to Remember

| Formula | Description |
|---------|-------------|
| Sequence number space = 0 to 2ⁿ - 1 | For n-bit sequence number |
| MAX_SEQ = 2ⁿ - 1 | Maximum sequence number |
| Total distinct sequence numbers = 2ⁿ | Also = MAX_SEQ + 1 |
| Go Back N Window Size = 2ⁿ - 1 | = MAX_SEQ |
| Selective Repeat Window Size = 2ⁿ⁻¹ | = (MAX_SEQ + 1) / 2 |

---

# 10 MCQs from Lecture 15

---

**Q1. In Stop and Wait protocol, how many packets can be outstanding in the network at a time?**

(A) 0  
(B) 1  
(C) 2  
(D) Depends on window size  

**Answer: (B) 1**

Explanation: In Stop and Wait, the sender sends exactly 1 packet and waits for ACK before sending the next. So only 1 packet is outstanding at a time. This is the main limitation that sliding window protocols solve.

---

**Q2. If a 3-bit sequence number is used, what is the range of sequence numbers?**

(A) 0 to 3  
(B) 0 to 7  
(C) 0 to 8  
(D) 1 to 8  

**Answer: (B) 0 to 7**

Explanation: For n-bit sequence number, the range is 0 to 2ⁿ - 1. For 3 bits: 0 to 2³ - 1 = 0 to 7. That gives 8 distinct sequence numbers.

---

**Q3. A sliding window protocol with window size = 1 behaves as which protocol?**

(A) Go Back N  
(B) Selective Repeat  
(C) Stop and Wait  
(D) CSMA/CD  

**Answer: (C) Stop and Wait**

Explanation: As explained in the transcript, when window size = 1, the sender can only send 1 frame and must wait for ACK before sending the next. This is exactly how Stop and Wait works.

---

**Q4. In Go Back N ARQ, when a timeout occurs, what does the sender do?**

(A) Retransmit only the lost frame  
(B) Retransmit all frames in the current window  
(C) Stop transmission permanently  
(D) Send a NAK to the receiver  

**Answer: (B) Retransmit all frames in the current window**

Explanation: In Go Back N, on timeout, the sender retransmits ALL frames from base to (next sequence number - 1) — meaning all outstanding frames in the current window.

---

**Q5. For Go Back N with a 3-bit sequence number, what is the maximum window size?**

(A) 8  
(B) 7  
(C) 4  
(D) 3  

**Answer: (B) 7**

Explanation: Go Back N window size = 2ⁿ - 1 = 2³ - 1 = 7. It is NOT 8 (which is the total sequence number space). Using 8 would cause confusion when all ACKs are lost.

---

**Q6. For Selective Repeat with a 3-bit sequence number, what is the maximum window size?**

(A) 8  
(B) 7  
(C) 4  
(D) 3  

**Answer: (C) 4**

Explanation: Selective Repeat window size = 2ⁿ⁻¹ = 2³⁻¹ = 2² = 4. Alternatively, (MAX_SEQ + 1)/2 = 8/2 = 4.

---

**Q7. In Selective Repeat ARQ, what does the receiver do when it receives a frame out of order?**

(A) Discards it  
(B) Sends NAK and discards it  
(C) Buffers it and sends ACK for it  
(D) Retransmits the previous frame  

**Answer: (C) Buffers it and sends ACK for it**

Explanation: In Selective Repeat, the receiver can accept out-of-order frames. It puts them in a buffer and sends individual ACK. It also sends NAK for the missing frame. This is different from Go Back N where out-of-order frames are discarded.

---

**Q8. Which of the following is used in Selective Repeat ARQ to inform the sender about lost packets?**

(A) Cumulative ACK only  
(B) NAK (Negative Acknowledgement) or SACK  
(C) Timeout only  
(D) Piggybacking  

**Answer: (B) NAK (Negative Acknowledgement) or SACK**

Explanation: In Selective Repeat, NAK (Negative Acknowledgement) or SACK (Selective Acknowledgement) is used to inform the sender about which specific packets need to be retransmitted.

---

**Q9. In Go Back N ARQ, does the receiver accept out-of-order frames?**

(A) Yes, and buffers them  
(B) Yes, and delivers them immediately  
(C) No, it discards them  
(D) No, it sends them back to sender  

**Answer: (C) No, it discards them**

Explanation: In Go Back N, the receiver only accepts frames in order. If it receives an out-of-order frame, it simply discards it and sends ACK for the last correctly received frame.

---

**Q10. If the maximum sequence number (MAX_SEQ) is 3, how many distinct sequence numbers are available?**

(A) 3  
(B) 4  
(C) 7  
(D) 8  

**Answer: (B) 4**

Explanation: If MAX_SEQ = 3, the distinct sequence numbers are 0, 1, 2, 3 — which is (MAX_SEQ + 1) = 4. This would correspond to a 2-bit sequence number since 2² = 4.

---

## End of Lecture 15 Explanation

**What was covered:** This lecture covered sliding window protocols at the transport layer — the motivation (Stop and Wait's limitation), the basic mechanism (sending window, receiving window, sliding), and the two important variants: Go Back N ARQ and Selective Repeat ARQ, along with their window size bounds and the reasoning behind those bounds.

**What comes next (Lecture 16):** The next lecture will look into performance aspects of the transport layer protocol and how these flow control algorithms are actually implemented in TCP.
