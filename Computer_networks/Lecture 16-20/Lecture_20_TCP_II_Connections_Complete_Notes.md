# Lecture 20: Transmission Control Protocol - II (Connections)

**Course:** Computer Networks and Internet Protocol  
**Professor:** Prof. Sandip Chakraborthy, IIT Kharagpur  
**Topic:** TCP Connection Establishment, Connection Release, and TCP State Transition Diagram

---

## Overview of This Lecture

This lecture covers three major topics:

1. How TCP establishes a connection using the **3-way handshake**
2. How TCP **chooses the initial sequence number** (and why it matters)
3. How TCP **releases (closes) a connection**
4. The complete **TCP State Transition Diagram** — how TCP moves between different states during connection setup, data transfer, and connection closure

---

## Concept 1: TCP Connection Establishment (3-Way Handshake)

### Simple Explanation

TCP is a **connection-oriented** protocol. Before two computers (let us call them Host A and Host B) can exchange data, they must first set up a connection. This setup process is called the **3-way handshake**.

Think of it like a phone call: you dial (step 1), the other person picks up and says hello (step 2), and you confirm you can hear them (step 3). Only then does the actual conversation begin.

TCP uses a special message called **SYN** (short for **Synchronization**) to start a connection.

### Step-by-Step Process

**Step 1 — SYN from Host A:**  
Host A wants to talk to Host B. Host A sends a **SYN message** with an **initial sequence number = x**. This tells Host B: "I want to connect, and I will start numbering my data bytes from x."

**Step 2 — SYN + ACK from Host B:**  
Host B receives the SYN. Host B replies with a combined **SYN + ACK message**. In this message:
- Host B sends its own **initial sequence number = y** (because TCP connection is **bidirectional** — both sides can send data)
- Host B sends an **acknowledgement number = x + 1** (acknowledging Host A's SYN)

This is called **piggybacking** — combining two messages (SYN and ACK) into one. In the TCP header, both the SYN flag bit and the ACK flag bit are set to 1.

**Step 3 — ACK from Host A:**  
Host A receives the SYN+ACK. Host A checks if the acknowledgement number matches its SYN (it should be x+1). If yes, it accepts this as a valid response. Host A then sends a final **ACK message** with:
- Sequence number = x + 1
- Acknowledgement number = y + 1

Now the connection is **established** and both sides can start sending data.

### Why is it Bidirectional?

TCP connection is **bidirectional** — Host A can send data to Host B, and at the same time Host B can send data to Host A. That is why both sides exchange their own initial sequence numbers during the handshake. The sequence number x is used for A-to-B data transfer, and sequence number y is used for B-to-A data transfer.

### Exam Important Points

- TCP uses a **3-way handshake** for connection establishment
- SYN stands for **Synchronization**
- The SYN+ACK message uses **piggybacking** (combining SYN and ACK in one message)
- Both SYN and ACK flag bits are set to 1 in the SYN+ACK message
- If Host A sends SYN with Seq = x, Host B acknowledges with ACK = x + 1
- If Host B sends SYN with Seq = y, Host A acknowledges with ACK = y + 1
- The connection is bidirectional — both sides choose their own initial sequence numbers

### Common Confusions

- Students often confuse "who sends what." Remember: the **initiator** (Host A) sends SYN first. The **responder** (Host B) sends SYN+ACK. Then Host A sends the final ACK.
- The SYN+ACK is a **single message**, not two separate messages. It is piggybacked.
- The acknowledgement number is always **sequence number + 1** of the SYN it is acknowledging.

### Possible NPTEL-style Questions

- Q: In TCP 3-way handshake, if Host A sends SYN with Seq = 100, what will be the ACK number in Host B's response?  
  A: 101 (x + 1)

- Q: Which flags are set in the second message of the 3-way handshake?  
  A: Both SYN and ACK flags are set to 1.

---

## Concept 2: How TCP Chooses the Initial Sequence Number

### Simple Explanation

When TCP starts a connection, it needs to pick a starting number for the sequence numbers. This starting number is called the **Initial Sequence Number (ISN)**. The big question is: how does TCP decide what this number should be?

The answer is important because TCP needs to **avoid delayed duplicates** — old packets from a previous connection that are still floating around in the network should not be confused with packets from a new connection.

### The Problem: Forbidden Region

Imagine you had a connection (Connection 1) that was using certain sequence numbers. That connection crashes. Now you start a new connection (Connection 2) on the same port. If Connection 2 picks an initial sequence number that falls in the **forbidden region** of Connection 1 (the range of sequence numbers that were being used by Connection 1 and whose packets might still exist in the network), then delayed duplicate packets from Connection 1 could be mistakenly accepted as valid packets for Connection 2.

So the rule is: **the forbidden regions of Connection 1 and Connection 2 must not overlap**.

### Two Options to Avoid Overlap

**Option 1 — Wait (Time-based shift):**  
After Connection 1 crashes, wait for enough time so that all old packets from Connection 1 die off in the network. Only then start Connection 2. This creates a time gap between the two connections.

**Option 2 — Shift in Sequence Number Space:**  
Choose the initial sequence number for Connection 2 high enough so that there is a gap between the sequence numbers used by Connection 1 and those used by Connection 2. This way, even if old packets exist, they will not fall in the range of Connection 2's sequence numbers.

### The Clock-Based Approach (Original TCP Implementation)

TCP uses the **second option** — shifting in the sequence number domain. The original implementation used a **clock-based approach**:

- TCP had an internal clock that **ticked every 4 microseconds**
- At each tick, a new sequence number was generated
- The clock value cycles from **0 to 2^32 - 1** (because TCP uses a 32-bit sequence number field, so the entire sequence number space is 0 to 2^32 - 1)
- The **current value of the clock** gives the initial sequence number for a new connection

This way, if a connection crashes and restarts later, the clock has moved forward. The new initial sequence number will be higher than what was used before, ensuring no overlap in the forbidden regions.

### The Security Problem with Clock-Based Approach

The clock-based method has a weakness: it is **deterministic** (predictable). An attacker can observe previous sequence numbers and figure out the clock tick rate. By calculating how much time has passed since the last connection crashed, the attacker can **predict the next initial sequence number**.

This enables the **TCP SYN Flood Attack**.

### Exam Important Points

- The goal of choosing the ISN is to **avoid delayed duplicates**
- The forbidden region of two consecutive connections should **not overlap**
- Original TCP used a **clock-based approach** ticking every **4 microseconds**
- The sequence number space is **0 to 2^32 - 1** (32-bit field)
- The clock-based approach is **deterministic** and therefore vulnerable to attack

### Common Confusions

- Students confuse the forbidden region concept. The forbidden region is simply the range of sequence numbers that a connection has used and whose packets could still be alive in the network.
- The clock is not a real wall clock — it is an internal counter that increments every 4 microseconds.

---

## Concept 3: TCP SYN Flood Attack

### Simple Explanation

Because the clock-based ISN generation is predictable, an attacker can exploit this. In a **SYN Flood Attack**, the attacker sends a large number of fake (spurious) SYN connection requests to a server. The server thinks each SYN is a genuine connection request because the predicted initial sequence number matches what the clock would produce. So the server accepts all these fake SYN messages and allocates resources for each one.

If the attacker sends many such fake SYNs from multiple computers, the server becomes so busy responding to these fake connection requests that it **cannot serve real users** anymore. This is a **Denial of Service (DoS) attack**.

### Solution: Cryptographic Function

The current version of TCP solves this by using a **cryptographic function** to generate the initial sequence number instead of just using the clock value directly.

How it works:
- The clock still gives a base value (say x)
- A **cryptographic hash function** is applied to x to produce a new value y, where y > x
- Since y is generated from a cryptographically secure function, the attacker **cannot predict y** even if they know x
- This ensures both: (a) the forbidden regions do not overlap (because y > x, sequence numbers keep moving forward), and (b) the ISN is **not predictable**, preventing SYN flood attacks

### Exam Important Points

- SYN Flood Attack sends **spurious SYN messages** to overwhelm a server
- It leads to a **Denial of Service (DoS)** attack
- The solution is to use a **cryptographic function** to generate the ISN
- The cryptographic function makes the ISN **unpredictable** while still ensuring forbidden regions don't overlap

### Common Confusions

- The cryptographic function does not replace the clock entirely — it uses the clock value as input and produces an unpredictable output
- SYN Flood is an attack on the **connection establishment** phase, not on data transfer

---

## Concept 4: TCP Connection Release

### Simple Explanation

Just like TCP has a formal process to open a connection, it also has a formal process to **close** a connection. TCP connection release also uses a handshake-like mechanism.

There are two roles during connection closure:
- **Active Close:** The host that **initiates** the closure (decides to close first)
- **Passive Close:** The host that **receives** the closure request

### Step-by-Step Process

**Step 1 — FIN from Host A (Active Close):**  
Host A decides to close the connection. It sends a **FIN (Finish) message** with its current sequence number (say M) and current acknowledgement number (say N). The FIN flag is set in the TCP header.

**Step 2 — FIN + ACK from Host B (Passive Close):**  
Host B receives the FIN. Host B sends back a **FIN + ACK message**:
- Sequence number = N
- Acknowledgement number = M + 1 (acknowledging Host A's FIN)
- Host B also sets the FIN flag because it wants to close the connection from its side too (remember, TCP is bidirectional)

**Step 3 — ACK from Host A:**  
Host A receives the FIN+ACK from Host B. Host A sends a final **ACK message**:
- Sequence number = M + 1
- Acknowledgement number = N + 1

### Why the Active Close Side Has a Timeout

After Host A sends the final ACK, it does **not** immediately close the connection. Instead, it enters a **timeout period**. Why?

Because TCP operates over an unreliable network, and **asymmetric closure is not possible** on an unreliable channel (this is related to the **Two Army Problem** discussed earlier in the course). The timeout ensures that:
- If there are still some packets in transit from Host B, Host A can still receive them
- After the timeout expires, Host A completely closes the connection and will not accept any more packets

### Why No Timeout on the Passive Close Side

The passive close side (Host B) does **not** need a timeout. The reason is simple: when Host B receives the final ACK from Host A, it already knows that Host A has closed its side. Host A will not send any more data. So Host B can safely close immediately — there is no need to wait.

Think of it like this: if your friend has already closed the door and walked away, you don't need to stand at the door waiting — you know they are gone.

### Can B Delay Its FIN?

Yes. If Host B is not ready to close immediately, it can first send **only an ACK** (without FIN). Later, when it is ready, it can send its own **FIN message** separately. This is also a valid scenario.

### Exam Important Points

- TCP connection release uses **FIN (Finish)** messages
- The host initiating closure performs **Active Close**
- The host receiving the FIN performs **Passive Close**
- The **active close side** has a **timeout** after the final exchange (to handle remaining packets)
- The **passive close side** does **not** need a timeout
- The timeout exists because of the impossibility of symmetric closure over an unreliable channel (Two Army Problem)
- Host B can choose to send ACK first and FIN later (does not have to close immediately)

### Common Confusions

- Students confuse active close and passive close. Remember: the one who **sends FIN first** is doing the active close.
- The timeout is only on the **active close side**, not on the passive close side.
- Connection release closes **both directions** of the bidirectional connection.

---

## Concept 5: TCP State Transition Diagram

### Simple Explanation

TCP behaves like a **state machine**. At any given time, a TCP connection is in one particular **state**. Events (like receiving a message or a user action) cause the connection to **transition** from one state to another.

The state transition diagram shows all possible states and all possible transitions. In the diagram:
- **Solid lines** represent the **Client** side transitions
- **Dashed lines** represent the **Server** side transitions
- Each transition is written as: **Event / Action** (what happened / what TCP does in response)

### All TCP States

Here is every state and what it means:

| State | Meaning |
|-------|---------|
| **CLOSED** | No connection is active or pending. This is the starting and ending state. |
| **LISTEN** | The server is waiting for an incoming connection request (SYN). |
| **SYN RCVD** (SYN Received) | A connection request (SYN) has arrived; server is waiting for ACK. |
| **SYN SENT** | The application (client) has started to open a connection by sending SYN. |
| **ESTABLISHED** | The normal data transfer state. Both sides can send and receive data. |
| **FIN WAIT 1** | The application has said it is finished (sent FIN). Waiting for acknowledgement. |
| **FIN WAIT 2** | The other side has agreed to release (sent ACK for our FIN). Waiting for their FIN. |
| **TIME WAIT** | Wait for all packets to die off in the network before fully closing. |
| **CLOSING** | Both sides have tried to close simultaneously. |
| **CLOSE WAIT** | The other side has initiated a release (sent FIN). Waiting for local application to close. |
| **LAST ACK** | Wait for all packets to die off (final ACK expected). |

### Connection Establishment — Client Side Path

1. **CLOSED → SYN SENT:** The client calls connect(). TCP sends a SYN message. Client moves to **SYN SENT** state.

2. **SYN SENT → ESTABLISHED:** The client receives SYN+ACK from the server. Client sends the final ACK. Client moves to **ESTABLISHED** state. Data transfer can now begin.

3. **SYN SENT → CLOSED:** If the client decides to abort (close) before getting a response, it goes back to CLOSED. Even if the server later sends SYN+ACK, the client will drop it.

### Connection Establishment — Server Side Path

1. **CLOSED → LISTEN:** The server calls listen(). It moves to **LISTEN** state, ready to accept incoming connections.

2. **LISTEN → SYN RCVD:** The server receives a SYN from a client. It sends back SYN+ACK. Server moves to **SYN RCVD** (SYN Received) state.

3. **SYN RCVD → ESTABLISHED:** The server receives the final ACK from the client (step 3 of the 3-way handshake). Server moves to **ESTABLISHED** state. Data transfer can begin.

4. **SYN RCVD → LISTEN:** If the server decides to reset the connection (using RST — Reset), it goes back to LISTEN state. This can happen if the server detects a SYN flood attack or if the application wants to abort.

### Simultaneous Open

There is a special case where **both the client and the server send SYN messages at the same time** before either receives the other's SYN. This is called a **simultaneous open**.

In this case:
- The client has sent a SYN and is in SYN SENT state
- Instead of receiving SYN+ACK, it receives just a SYN (from the server, which was also initiating)
- The client sends SYN+ACK and moves to **SYN RCVD** state
- When the acknowledgement arrives, it moves to **ESTABLISHED**

Both sides follow a similar path, and both eventually reach ESTABLISHED state.

### Connection Closure — Active Close (Client Side) Path

1. **ESTABLISHED → FIN WAIT 1:** The client calls close(). TCP sends a FIN message. Client moves to **FIN WAIT 1**.

2. From FIN WAIT 1, there are **two possibilities:**

   **Possibility A — Receives only ACK (no FIN yet):**  
   FIN WAIT 1 → **FIN WAIT 2:** The server sends only an ACK (no FIN). This means the server still has data to send. Client moves to FIN WAIT 2 and waits for the server's FIN.  
   FIN WAIT 2 → **TIME WAIT:** When the server's FIN finally arrives, client sends ACK and moves to TIME WAIT.

   **Possibility B — Receives FIN+ACK together:**  
   FIN WAIT 1 → **TIME WAIT:** The server sends FIN+ACK together. Client sends ACK and goes directly to TIME WAIT.

3. **TIME WAIT → CLOSED:** After the timeout expires, the connection is fully closed.

### Connection Closure — Simultaneous Close

There is another possibility: the client sends FIN, but before getting any response, it receives a FIN from the server (the server also wants to close at the same time). This is a **simultaneous close**.

In this case:
- From **FIN WAIT 1**, the client receives FIN (without ACK)
- Client sends ACK and moves to **CLOSING** state
- In CLOSING state, client is waiting for the ACK of its own FIN
- When that ACK arrives, client moves to **TIME WAIT**, then after timeout, to CLOSED

### Connection Closure — Passive Close (Server Side) Path

1. **ESTABLISHED → CLOSE WAIT:** The server receives a FIN from the client. It sends an ACK. Server moves to **CLOSE WAIT**. At this point, the server can still send remaining data.

2. **CLOSE WAIT → LAST ACK:** When the server is ready to close, it sends its own FIN message. Server moves to **LAST ACK**, waiting for the final acknowledgement.

3. **LAST ACK → CLOSED:** The server receives the final ACK from the client. The connection is fully closed, and the server goes back to CLOSED state.

### The TIME WAIT State — Why It Matters

The TIME WAIT state is one of the most important states in TCP:
- It exists only on the **active close** side
- After receiving the final FIN+ACK and sending the final ACK, the node **waits for a timeout duration** before fully closing
- This timeout ensures that any remaining packets in the network can be received or die off
- It prevents data loss as much as possible
- This is necessary because of the **impossibility of symmetric closure over an unreliable channel** (the Two Army Problem)

The passive close side does **not** have a TIME WAIT state because it already knows the other side has closed (it received the FIN), so it does not need to wait.

### Exam Important Points

- TCP has **11 states** in its state transition diagram
- Solid lines = Client, Dashed lines = Server
- **CLOSED** is the starting and ending state
- **LISTEN** is where the server waits for connections
- **ESTABLISHED** is the data transfer state
- **TIME WAIT** exists only on the active close side, with a timeout
- **Simultaneous open** is when both sides send SYN at the same time
- **Simultaneous close** (CLOSING state) is when both sides send FIN at the same time
- The passive close side goes through CLOSE WAIT → LAST ACK → CLOSED
- The active close side goes through FIN WAIT 1 → FIN WAIT 2 → TIME WAIT → CLOSED (normal case)
- RST (Reset) can be used from SYN RCVD to go back to LISTEN state

### Common Confusions

- TIME WAIT and CLOSE WAIT are different. TIME WAIT is on the active close side; CLOSE WAIT is on the passive close side.
- FIN WAIT 1 and FIN WAIT 2 are both on the active close side. FIN WAIT 1 means "I sent FIN, waiting for any response." FIN WAIT 2 means "I got ACK for my FIN, but still waiting for their FIN."
- The CLOSING state only happens during simultaneous close — both sides sent FIN at the same time.
- LAST ACK is on the passive close (server) side — it means "I sent my FIN, waiting for the final ACK."

---

## Summary of Key Takeaways from Lecture 20

1. TCP connection establishment uses a **3-way handshake**: SYN → SYN+ACK → ACK
2. The initial sequence number must avoid the **forbidden region** to prevent delayed duplicates
3. Original TCP used a **clock-based ISN** (tick every 4 microseconds, range 0 to 2^32 - 1)
4. Clock-based ISN is **predictable** → enables **SYN Flood (DoS) attack**
5. Modern TCP uses a **cryptographic function** to generate unpredictable ISNs
6. TCP connection release uses **FIN messages** with active close and passive close
7. The **active close** side has a **timeout (TIME WAIT)** to handle remaining packets
8. The **passive close** side does **not** need a timeout
9. TCP follows a **state transition diagram** with 11 states
10. The **ESTABLISHED** state is where actual data transfer happens

---

## 10 MCQs from Lecture 20

### Q1. TCP connection establishment uses which mechanism?

(A) 2-way handshake  
(B) 3-way handshake  
(C) 4-way handshake  
(D) No handshake

**Answer: (B)**  
**Explanation:** As taught in the transcript, TCP uses a **3-way handshake** for connection establishment: SYN → SYN+ACK → ACK.

---

### Q2. In TCP 3-way handshake, Host A sends SYN with Seq = 500. What acknowledgement number will Host B send in the SYN+ACK?

(A) 500  
(B) 499  
(C) 501  
(D) 502

**Answer: (C)**  
**Explanation:** The acknowledgement number is always the received sequence number + 1. So ACK = 500 + 1 = **501**.

---

### Q3. Why does the SYN+ACK message from Host B also contain a SYN (its own sequence number)?

(A) Because TCP connection is simplex  
(B) Because TCP connection is half-duplex  
(C) Because TCP connection is bidirectional (full-duplex)  
(D) Because TCP does not support data transfer from server to client

**Answer: (C)**  
**Explanation:** TCP connection is **bidirectional**. Both Host A and Host B can send data to each other. So Host B also needs to establish its own sequence number for B-to-A data transfer.

---

### Q4. In the original implementation of TCP, the initial sequence number was generated using:

(A) A random number generator  
(B) A clock-based approach that ticked every 4 microseconds  
(C) The IP address of the host  
(D) The port number of the application

**Answer: (B)**  
**Explanation:** The original TCP used a **clock-based approach** where the clock ticked every 4 microseconds, cycling from 0 to 2^32 - 1. The current clock value was used as the initial sequence number.

---

### Q5. What is the main purpose of choosing the initial sequence number carefully?

(A) To increase data transfer speed  
(B) To reduce packet size  
(C) To avoid delayed duplicates from a previous connection  
(D) To encrypt the data

**Answer: (C)**  
**Explanation:** The transcript explains that the main objective of choosing the ISN is to **avoid delayed duplicates** — so old packets from a crashed connection are not confused with packets of a new connection.

---

### Q6. A TCP SYN Flood Attack leads to:

(A) Data corruption  
(B) Faster connections  
(C) Denial of Service (DoS)  
(D) Encryption failure

**Answer: (C)**  
**Explanation:** In a SYN Flood Attack, the attacker sends many spurious SYN messages to a server. The server becomes busy responding to fake requests and cannot serve real users — this is a **Denial of Service (DoS)** attack.

---

### Q7. In TCP connection release, the host that initiates the closure is said to perform:

(A) Passive close  
(B) Active close  
(C) Forced close  
(D) Symmetric close

**Answer: (B)**  
**Explanation:** The host that **initiates** the connection closure (sends FIN first) performs an **active close**. The host that receives the FIN performs a passive close.

---

### Q8. Which of the following TCP states exists only on the active close side?

(A) CLOSE WAIT  
(B) LAST ACK  
(C) TIME WAIT  
(D) LISTEN

**Answer: (C)**  
**Explanation:** **TIME WAIT** is the state on the active close side where it waits for a timeout duration after the final exchange before fully closing the connection. CLOSE WAIT and LAST ACK are on the passive close side.

---

### Q9. Why does the active close side need a timeout (TIME WAIT) but the passive close side does not?

(A) Because the active close side has more data to send  
(B) Because the passive close side already knows the other end has closed, so no waiting is needed  
(C) Because timeout is only needed for servers  
(D) Because the passive close side uses a different protocol

**Answer: (B)**  
**Explanation:** The passive close side receives the FIN from the active side, so it **already knows** the other end has finished. There is no need to wait. The active close side, however, is forcefully closing and must give the other end time to send remaining data, hence the timeout.

---

### Q10. In the TCP state transition diagram, both the client and the server sending SYN at the same time is called:

(A) SYN flood  
(B) Passive open  
(C) Simultaneous open  
(D) Reset

**Answer: (C)**  
**Explanation:** When both client and server initiate SYN messages at the same time (before either receives the other's SYN), it is called a **simultaneous open**. In this case, both sides go through SYN SENT → SYN RCVD → ESTABLISHED.

---

*End of Lecture 20 Notes*
