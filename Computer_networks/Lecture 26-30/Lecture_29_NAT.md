# Lecture 29: IP Addressing (IPv4) III – Network Address Translation (NAT)

**Course:** Computer Networks and Internet Protocol  
**Professor:** Prof. Sandip Chakraborty, IIT Kharagpur  
**Topic:** Network Address Translation (NAT) — The complete concept

---

## Concept 1: The Problem with IPv4 Addressing

### 🧠 Simple Explanation

Before understanding NAT, you need to understand **why** NAT was needed in the first place. The problem is simple — **IPv4 addresses are limited**.

IPv4 addresses are 32 bits long. We have three main classes used for normal communication: **Class A**, **Class B**, and **Class C**. Now, Class D is reserved for multicast data delivery, and Class E is reserved for future use. So we **cannot use** Class D and Class E addresses for regular internet communication.

Even within Class A, B, and C — we cannot use **all** addresses. Every network has a **network address** and a **broadcast address** that cannot be assigned to any host. This **further reduces** the number of usable addresses.

Now think about the real world: the number of devices (computers, phones, IoT devices) that need IP addresses is **increasing exponentially** — far more than what was imagined when IPv4 was first designed.

Also, the number of IP addresses needed is **not equal** to the number of devices. Many modern devices have **multiple network interfaces** (like Wi-Fi + Ethernet), and each interface needs its **own** IP address. This makes the shortage even worse.

### 🎯 Exam Important Points

- IPv4 has a limited address space (32-bit)
- Only Class A, B, C are used for normal data transfer
- Class D = Multicast (rarely used), Class E = Reserved → both are **wasted/underutilized**
- Broadcast address and network address in each network cannot be assigned to hosts
- Number of devices is increasing exponentially
- Each network interface card needs a separate IP address

### ⚠️ Common Confusions

- Students think "32-bit means 2^32 addresses are all usable" — **No!** Many addresses are wasted on network IDs, broadcast addresses, Class D, and Class E.
- "Class D is usable" — **No**, Class D is only for multicast and is rarely used in today's internet.

---

## Concept 2: The Solution — Making Addresses Reusable

### 🧠 Simple Explanation

The solution to the IPv4 shortage is to **make addresses reusable**. But wait — originally, IP addresses were designed to be **unique worldwide**. Every device must be uniquely identifiable on the internet. So how can we reuse them?

The transcript gives a beautiful real-world analogy: Think of a person named "Sandip Chakraborty." There can be a Sandip Chakraborty at **IIT Kharagpur** and another at **IIT Bombay**. To disambiguate them, we use their **location** — "Sandip Chakraborty at IIT Kharagpur" vs "Sandip Chakraborty at IIT Bombay."

The same idea is used in networking. We create two types of addresses:

1. **Private addresses** — These can be **reused** inside different organizations. The same private IP block can exist inside IIT Kharagpur, IIT Bombay, Stanford, etc.
2. **Public addresses** — These are **globally unique** and used to send packets across the internet.

The key insight is: **not all users connect to the internet at the same time**. Some are sleeping, some are offline. So, you only need a small number of public IPs — equal to the number of users who are online simultaneously. You can **dynamically map** private IPs to public IPs when a user wants to go online.

### 🎯 Exam Important Points

- Private IP addresses are **reusable** across organizations
- Public IP addresses must be **globally unique**
- The concept leverages the fact that not all users connect simultaneously
- Dynamic mapping between private IP and public IP allows reusability

### ⚠️ Common Confusions

- "Private IP can be used to send packets on the internet" — **No!** You need a public IP to communicate on the internet. Private IPs only work inside the local network.
- "Every device needs a unique public IP" — **No**, with NAT multiple devices can share a few public IPs.

---

## Concept 3: What is Network Address Translation (NAT)?

### 🧠 Simple Explanation

**NAT (Network Address Translation)** is the mechanism that makes address reusability possible. Here is what NAT does:

1. **Divides addresses** into two blocks: **reusable (private)** and **non-reusable (public)**
2. **Translates** internal (private) addresses to external (public) addresses when a packet goes out
3. **Hides** internal machines from external devices — the outside world only sees the public IP of the NAT device
4. **Allows internet access** to a large number of users using only a few public addresses

So, NAT acts as a **translator** sitting at the boundary between your private network and the public internet. When your machine sends a packet, NAT replaces your private IP with a public IP. When the reply comes back, NAT replaces the public IP back with your private IP.

### 🛠 Real-world Example (from transcript)

Think of NAT like the **postal center of IIT Kharagpur**. When you send a letter from inside IIT KGP, the outside world only sees "IIT Kharagpur" as the sender (the public identity). The postal center knows internally whether the letter came from Sandip Chakraborty or Soumukh K Ghosh. This "hiding" of internal identity is exactly what NAT does.

### 🎯 Exam Important Points

- NAT = translates private IP ↔ public IP
- NAT hides internal machines from external devices
- NAT allows many users to share few public IPs
- NAT sits at the boundary between private and public networks

### ⚠️ Common Confusions

- "NAT is a protocol" — NAT is more of a **mechanism** or **technique** implemented in a router or gateway.
- "NAT changes the destination IP for outgoing packets" — **No!** For outgoing packets, NAT changes the **source IP** (from private to public). The destination stays the same.

---

## Concept 4: IPv4 Private Address Ranges

### 🧠 Simple Explanation

IPv4 has designated specific blocks of addresses from each class as **private IP addresses**. These are the addresses that can be reused inside any organization. The three private address ranges are:

| Class | Private IP Range | 
|-------|-----------------|
| Class A | **10.0.0.0** to **10.255.255.255** |
| Class B | **172.16.0.0** to **172.32.255.255** |
| Class C | **192.168.0.0** to **192.168.255.255** |

So from each class (A, B, and C), one block (or a few blocks) of addresses have been taken out and designated as private. These addresses will **never** be assigned as public IPs on the internet. They are only for internal use.

### 🎯 Exam Important Points

- **Memorize** the three private IP ranges — this is very commonly asked
- Class A private range starts with **10.x.x.x**
- Class B private range starts with **172.16.x.x** to **172.32.x.x**
- Class C private range starts with **192.168.x.x**
- Private IPs are not routable on the public internet

### ⚠️ Common Confusions

- "Any address starting with 172 is private" — **No!** Only **172.16.0.0 to 172.32.255.255** is private. Addresses like 172.1.x.x are public.
- "192.168.1.1 is a public IP" — **No!** Anything in 192.168.x.x range is private.

### 📝 Possible NPTEL-style Question

*Which of the following is a valid private IP address?*  
(a) 11.0.0.1 (b) 172.15.0.1 (c) 192.168.1.1 (d) 128.0.0.1  
**Answer: (c)** — 192.168.1.1 falls in the Class C private range.

---

## Concept 5: Basic Operation of NAT — Step by Step

### 🧠 Simple Explanation

Let's understand how NAT works step-by-step using the example from the transcript:

**Setup:**
- You have a **private network** (like IIT KGP network)
- One machine inside has a private IP: **10.0.1.2**
- You want to send a packet to an outside machine with public IP: **213.168.112.3**
- The NAT device has a **pool of public IPs**, one of which is **128.143.71.21**

**Step 1 — Outgoing packet (Private → Public):**
- Machine 10.0.1.2 creates a packet with Source IP = 10.0.1.2, Destination IP = 213.168.112.3
- The packet reaches the NAT device
- NAT device **replaces** the source IP 10.0.1.2 with a public IP **128.143.71.21** from its pool
- NAT device **stores this mapping** in its NAT table: 10.0.1.2 ↔ 128.143.71.21
- The packet goes out into the public internet with Source = 128.143.71.21, Destination = 213.168.112.3

**Step 2 — Reply packet (Public → Private):**
- The destination machine 213.168.112.3 receives the packet
- It sends a reply back with Source = 213.168.112.3, Destination = 128.143.71.21
- This packet reaches the NAT device (because 128.143.71.21 belongs to the NAT's pool)
- NAT device **looks up** its NAT table and finds: 128.143.71.21 was assigned to 10.0.1.2
- NAT device **replaces** destination IP from 128.143.71.21 to **10.0.1.2**
- The packet is delivered to the internal machine

### 🎯 Exam Important Points

- NAT replaces **source IP** for outgoing packets (private → public)
- NAT replaces **destination IP** for incoming reply packets (public → private)
- NAT device maintains a **NAT table** that stores the mapping between private IP and public IP
- The NAT device has a **pool** of public IP addresses
- The outside world never sees the private IP — it only sees the NAT's public IP

### ⚠️ Common Confusions

- "NAT changes both source and destination" — **No!** For outgoing: only source changes. For incoming reply: only destination changes.
- "The destination machine knows the private IP" — **No!** It only knows the public IP of the NAT device.

---

## Concept 6: Working Principles of NAT

### 🧠 Simple Explanation

The transcript summarizes the working principles of NAT as follows:

1. **Organizations manage internal private network** — The organization (like IIT Kharagpur) sets up and manages the private IP addresses inside their network.

2. **NAT boxes manage a pool of public IP addresses** — The NAT device (which is basically a router or gateway) has a set of public IPs that it can assign to outgoing connections.

3. **For outgoing connections, NAT boxes select one IP from the pool** — When a private machine wants to send data to the internet, the NAT box picks one available public IP from its pool and uses that to forward the packet.

So the NAT box is like a **middleman** — it manages both sides. Inside it knows who is who (using private IPs), and outside it presents everything under its public IPs.

### 🎯 Exam Important Points

- NAT box = router or gateway device
- NAT box manages the pool of public IPs
- Organization manages the private network
- For each outgoing connection, one public IP is selected from the pool

---

## Concept 7: NAT for Migration Between ISPs

### 🧠 Simple Explanation

NAT has another very useful benefit: **easy migration between ISPs (Internet Service Providers)**.

An organization can connect to **multiple ISPs** for better reliability. This is called a **multi-home network**. For example, IIT Kharagpur is connected to both **ERNET** network and **NKN** network.

Now here's the advantage of NAT: When you change your ISP (or if one ISP fails), only the **public IP address pool changes** in the NAT box. The **private IPs of all internal machines remain the same**. You do NOT need to reconfigure every single computer inside the organization.

**Without NAT:** If you change ISP, every internal machine's IP address would need to be changed to reflect the new ISP's network — a huge headache!

**With NAT:** Only the NAT box's mapping changes. Internal machines are completely unaffected.

### 🛠 Real-world Example (from transcript)

- Initially NAT is connected to **ISP 1** → public IP pool: 128.143.71.21
- ISP 1 fails → NAT switches to **ISP 2** → public IP pool changes to: 128.195.4.120
- The private IP **10.0.1.2** of the internal machine **stays the same**
- No reconfiguration needed on any internal machine!

### 🎯 Exam Important Points

- NAT makes ISP migration easy
- Multi-home network = organization connected to multiple ISPs
- Only the NAT box's public IP pool changes, not the internal private IPs
- Without NAT, every internal system's address would need to change when switching ISPs
- NAT box can be configured to use alternative ISPs in case of failure

### ⚠️ Common Confusions

- "Changing ISP requires changing all machine IPs" — **Only true without NAT.** With NAT, only the public pool changes.

---

## Concept 8: IP Masquerading (Port-Based NAT / PNAT)

### 🧠 Simple Explanation

Now this is a very important extension of NAT. In basic NAT, each private IP gets mapped to **one separate public IP**. But what if you have very few public IPs and many internal machines? This is where **IP Masquerading** or **Port-based NAT (PNAT)** comes in.

The key idea: **A single public IP address can be mapped to multiple internal hosts** by using **port numbers** to differentiate them.

How does this work? Remember, communication on the internet is actually **process-to-process**. A process is identified by an **IP address + port number** combination. So instead of mapping just IPs, the NAT device maps **IP:Port pairs**.

**Example from transcript:**
- Machine A: Private IP = 10.0.1.2, running app on port 2001
- Machine B: Private IP = 10.0.1.3, running app on port 3020
- NAT has **one** public IP: 128.143.71.21

The NAT table now looks like:

| Private IP:Port | Public IP:Port |
|----------------|---------------|
| 10.0.1.2:2001 | 128.143.71.21:**2100** |
| 10.0.1.3:3020 | 128.143.71.21:**4444** |

Both machines share the **same public IP** (128.143.71.21), but they have **different port numbers** on the public side.

When a reply comes to 128.143.71.21:2100, NAT knows it should go to 10.0.1.2:2001.  
When a reply comes to 128.143.71.21:4444, NAT knows it should go to 10.0.1.3:3020.

### 🎯 Exam Important Points

- IP Masquerading = Port-based NAT = **PNAT**
- Single public IP is shared by **multiple** internal hosts
- Differentiation is done using **port numbers**
- The NAT table maps **IP:Port ↔ IP:Port** (not just IP ↔ IP)
- You have around **65,000 ports** available, so even with few public IPs, you can support tens of thousands of connections
- Even after removing reserved ports, you still have around **50,000 unique port numbers**

### ⚠️ Common Confusions

- "Each machine needs its own public IP in NAT" — **Not in PNAT!** Multiple machines can share one public IP using different ports.
- "Port numbers in PNAT are the same as the original application port" — **Not necessarily!** The NAT device may assign a **different** port number on the public side.

---

## Concept 9: Load Balancing of Servers Using NAT

### 🧠 Simple Explanation

NAT can also be used for **load balancing**. Here's the idea:

Suppose you have **multiple identical servers** (like web servers) inside your private network, and they are all accessible from the internet through a **single public IP address**. 

When requests come from the outside world to this single public IP, the NAT device can **distribute** (redirect) these requests to different internal servers. This balances the load among the servers.

**Example from transcript:**
- Outside world sends requests to public IP: **128.143.71.21**
- Inside the private network, you have two web servers:
  - Server 1: **10.0.1.2**
  - Server 2: **10.0.1.3**
- NAT device sends some requests to 10.0.1.2 and some to 10.0.1.3 based on load

The NAT table maps:
- Same public IP 128.143.71.21 → 10.0.1.2 (for some connections)
- Same public IP 128.143.71.21 → 10.0.1.3 (for other connections)

The internal servers are configured with **private addresses**, and the NAT handles all the translation.

### 🎯 Exam Important Points

- NAT can be used for **load balancing** across multiple identical servers
- Multiple identical servers are accessible from a **single public IP**
- NAT box distributes incoming connections to different internal IPs
- Internal systems are configured with **private addresses**
- This is commonly used for web servers

### ⚠️ Common Confusions

- "Load balancing means all requests go to one server" — **No!** The whole point of load balancing is to distribute requests across multiple servers.

---

## Concept 10: Limitation of NAT — Connection Initiation Problem

### 🧠 Simple Explanation

NAT has an important **limitation**: someone from the **outside** (public network) **cannot directly initiate a connection** to a machine that is behind a NAT.

Why? Because the outside machine only knows the **public IP** of the NAT box, not the private IP of the internal machine. And unless there is already a mapping in the NAT table (created when the internal machine sent a packet out first), the NAT device does not know which internal machine the packet should go to.

So the rule is: **The connection must be initiated from the private/internal network side.** When the internal machine sends the first packet, the NAT creates a mapping. After that, the outside machine can reply using that mapping. But the outside machine **cannot start a new connection** to the internal machine unless it has information about the NAT box's public IP.

### 🛠 Real-world Example (from transcript)

- Your machine is inside the private network behind NAT
- A machine on the public internet wants to send you a message directly
- It cannot reach your private IP (like 10.0.1.2) because that address doesn't exist on the internet
- It needs to know the NAT box's public IP to reach you
- But even knowing the public IP, unless a mapping already exists, NAT won't know which internal machine to forward the packet to

### 🎯 Exam Important Points

- **Major limitation**: Outside machines cannot directly initiate connections to machines behind NAT
- Connection must be initiated from **inside** (private network)
- The NAT mapping is created only when the internal machine sends a packet first
- Unless mapping exists, NAT doesn't know where to forward incoming packets

### ⚠️ Common Confusions

- "NAT allows two-way communication always" — **Only if** the internal machine initiates the connection first. The outside cannot start on its own.

---

## Concept 11: Using DNS to Solve NAT's Limitation (for Servers)

### 🧠 Simple Explanation

For scenarios like **web servers** that are behind NAT, how do outside users connect if they can't initiate a connection? The answer is **DNS**.

Using DNS, a **domain name** (like www.iitkgp.ac.in) is mapped to the **public IP** of the NAT box (say, 202.141.81.2). When someone types www.iitkgp.ac.in, DNS resolves it to this public IP. The request reaches the NAT box, and the NAT box forwards it to one of the internal web servers (like 10.0.1.2 or 10.0.1.3) based on load balancing.

So DNS combined with NAT solves the problem for servers. But in general, for arbitrary machines behind NAT, you still cannot initiate connections from outside unless you know the public IP of the NAT box and there is a pre-configured mapping.

### 🎯 Exam Important Points

- DNS maps domain name → public IP of NAT box
- NAT then forwards the request to internal servers
- This is how websites behind NAT are accessible
- DNS + NAT together solve the connection initiation problem for servers
- For general machines behind NAT, the limitation still exists

---

## Concept 12: NAT Packet Flow — Detailed Walkthrough

### 🧠 Simple Explanation

The transcript provides a detailed step-by-step walkthrough of how packets flow through NAT:

**Outgoing packet (Inside → Outside):**
1. Internal machine creates packet: Source IP = 10.0.1.2 (private), Destination IP = 202.141.81.3 (public)
2. Packet reaches NAT box
3. NAT box changes Source IP from 10.0.1.2 to **194.3.2.2** (public IP from pool)
4. NAT box records mapping: 10.0.1.2 ↔ 194.3.2.2
5. Packet goes out: Source = 194.3.2.2, Destination = 202.141.81.3

**Reply packet (Outside → Inside):**
1. Destination machine creates reply: Source = 202.141.81.3, Destination = **194.3.2.2** (the NAT's public IP)
2. Packet reaches NAT box
3. NAT box looks up its table: 194.3.2.2 was mapped to 10.0.1.2
4. NAT box changes Destination IP from 194.3.2.2 to **10.0.1.2**
5. Packet is forwarded to the internal machine

The key point is: **The source IP in the request becomes the destination IP in the reply**, and the NAT reverses the translation.

### 🎯 Exam Important Points

- Outgoing: NAT changes **Source IP** (private → public)
- Incoming reply: NAT changes **Destination IP** (public → private)
- NAT maintains a mapping table for reverse translation
- The destination machine's reply uses the **public IP** as destination (it never sees the private IP)

---

## Concept 13: Brief Mention of IPv6 as the Long-term Solution

### 🧠 Simple Explanation

At the end of the lecture, the professor briefly mentions that while NAT helps manage the IPv4 shortage, the **long-term solution** is **IPv6**. IPv6 provides a much larger address space compared to IPv4 and has better mechanisms for managing the IP protocol.

However, the professor notes that IPv6 has **not been very successful** in full global deployment yet. People have understood for a long time that IPv6 is required, but till now it has not been deployed globally for every purpose. Recently, IPv6 is being explored for **Internet of Things (IoT)** communication, and it is being used in some places in an "island-wise" manner.

The professor mentions that the next lecture will cover IPv6 basics and how people are trying to make IPv4 and IPv6 compatible with each other.

### 🎯 Exam Important Points

- IPv6 is the long-term solution to IPv4 address shortage
- IPv6 has a much larger address space than IPv4
- IPv6 deployment has not been globally successful yet
- IPv6 is being explored for IoT communication
- NAT is the current practical solution to manage IPv4 limitations

---

## Summary Table

| Concept | Key Point |
|---------|-----------|
| IPv4 Problem | Limited addresses, exponential device growth |
| Solution Idea | Make addresses reusable (private + public) |
| NAT Definition | Translates private IP ↔ public IP at network boundary |
| Private IP Ranges | 10.x.x.x, 172.16.x.x–172.32.x.x, 192.168.x.x |
| Basic NAT Operation | Outgoing: replace source IP; Incoming: replace destination IP |
| NAT Table | Stores mapping between private and public IPs |
| ISP Migration | NAT makes switching ISPs easy — only public pool changes |
| IP Masquerading (PNAT) | Single public IP shared via different port numbers |
| Load Balancing | NAT distributes requests to multiple internal servers |
| NAT Limitation | Outside cannot initiate connection to machines behind NAT |
| DNS + NAT | DNS maps domain to NAT's public IP for server access |
| IPv6 | Long-term solution but not fully deployed globally yet |

---

## 10 MCQs — Strictly from Lecture 29 Transcript

### Q1. What is the primary problem with IPv4 addressing that NAT solves?

(a) IPv4 packets are too large  
(b) IPv4 addresses are limited and devices are increasing exponentially  
(c) IPv4 does not support routing  
(d) IPv4 cannot handle multicast  

**Answer: (b)**  
**Explanation:** The transcript clearly states that the IPv4 address space is limited, devices are increasing exponentially, and many addresses (Class D, Class E) are wasted. NAT was designed to solve this address shortage.

---

### Q2. Which of the following is NOT a private IPv4 address range?

(a) 10.0.0.0 – 10.255.255.255  
(b) 172.16.0.0 – 172.32.255.255  
(c) 192.168.0.0 – 192.168.255.255  
(d) 128.0.0.0 – 128.255.255.255  

**Answer: (d)**  
**Explanation:** The transcript specifies three private ranges: 10.x.x.x (Class A), 172.16.x.x to 172.32.x.x (Class B), and 192.168.x.x (Class C). The range 128.x.x.x is a public address range.

---

### Q3. What does NAT do when an outgoing packet leaves the private network?

(a) Changes the destination IP from private to public  
(b) Changes the source IP from private to public  
(c) Changes both source and destination IPs  
(d) Does not change any IP address  

**Answer: (b)**  
**Explanation:** As described in the transcript, for outgoing packets, the NAT device replaces the source IP (which is private) with a public IP from its pool. The destination IP remains unchanged.

---

### Q4. What is IP Masquerading also known as?

(a) Static NAT  
(b) Dynamic NAT  
(c) Port-based NAT (PNAT)  
(d) Reverse NAT  

**Answer: (c)**  
**Explanation:** The transcript explicitly states that IP masquerading is "basically an extension of NAT which is sometime called as a port based NAT or PNAT." It maps a single public IP to multiple hosts using different port numbers.

---

### Q5. In PNAT, how is differentiation made between multiple internal machines sharing the same public IP?

(a) By MAC address  
(b) By port numbers  
(c) By subnet mask  
(d) By TTL value  

**Answer: (b)**  
**Explanation:** The transcript explains that in PNAT, the IP address along with port number is used as a pair. Different internal machines get different port numbers on the public side, allowing the same public IP to serve multiple hosts.

---

### Q6. What advantage does NAT provide when an organization switches ISPs?

(a) All internal machines automatically get new IPs  
(b) Only the NAT box's public IP pool changes; internal private IPs remain unchanged  
(c) The ISP reconfigures all machines remotely  
(d) NAT has no role in ISP migration  

**Answer: (b)**  
**Explanation:** The transcript explains that when changing ISPs, only the public address pool in the NAT box changes. Internal machines keep their fixed private IP addresses and do not need any reconfiguration.

---

### Q7. What is a major limitation of NAT as described in the lecture?

(a) NAT cannot handle TCP packets  
(b) NAT slows down the internet speed significantly  
(c) An outside machine cannot directly initiate a connection to a machine behind NAT  
(d) NAT only works with Class A addresses  

**Answer: (c)**  
**Explanation:** The transcript clearly states that unless the connection is initiated from inside the private network, someone from outside will not be able to directly connect to machines behind NAT because the NAT table needs a mapping to be created first.

---

### Q8. How does NAT assist in load balancing of servers?

(a) By assigning different public IPs to each server  
(b) By mapping a single public IP to multiple internal servers and distributing incoming requests  
(c) By blocking excess traffic  
(d) By increasing the bandwidth  

**Answer: (b)**  
**Explanation:** The transcript explains that NAT can balance load by mapping a single public IP to multiple identical internal servers (with private addresses) and distributing the incoming requests among them.

---

### Q9. What is the role of DNS in solving NAT's connection initiation limitation for web servers?

(a) DNS assigns private IPs to external users  
(b) DNS maps the domain name to the public IP of the NAT box, allowing external users to reach the server  
(c) DNS directly communicates with internal machines  
(d) DNS replaces NAT entirely  

**Answer: (b)**  
**Explanation:** The transcript explains that DNS maps a domain name (like www.iitkgp.ac.in) to the public IP of the NAT box. When requests arrive, the NAT box forwards them to internal web servers. This way, external users can reach servers behind NAT.

---

### Q10. What is a "multi-home network" as mentioned in the lecture?

(a) A network with multiple private subnets  
(b) A network connected to multiple ISPs for better reliability  
(c) A network that uses both IPv4 and IPv6  
(d) A network with multiple NAT devices  

**Answer: (b)**  
**Explanation:** The transcript mentions that organizations like IIT Kharagpur connect to multiple networks (like ERNET and NKN) for better reliability. Such a network with multiple outgoing ISP connections is called a multi-home network. NAT makes it easy to switch between these ISPs.

---

*End of Lecture 29 — Complete Study Guide*
