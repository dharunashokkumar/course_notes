# Lecture 30: IPv6 Addressing

## 📚 Course: Computer Networks and Internet Protocol
### Prof. Sandip Chakraborty — IIT Kharagpur

---

## Concept 1: Why Do We Need IPv6?

### 🧠 Simple Explanation

In the previous lectures, we studied IPv4, which uses a **32-bit address** to identify each host on the internet. But the internet has grown enormously. Earlier, only desktop computers connected to the internet. Now, every person has multiple devices — desktops, laptops, mobile phones. On top of that, we are moving into the era of **Internet of Things (IoT)**, where tiny sensors are mounted on fridges, ACs, washing machines, doors, smart lights, and all these devices also need to connect to the internet.

Every device that connects to the internet **must have a unique IP address**. Because of this explosive growth, the demand for IP addresses has increased **exponentially**. But IPv4 with its 32-bit address space can only theoretically support **2^32 addresses** (about 4.3 billion). And not all of these are usable because many are reserved (broadcast addresses, loopback addresses, private addresses, subnet addresses, etc.).

So we have reached a point where the IPv4 address space is **getting saturated** — the addresses are running out. We tried techniques like **NAT (Network Address Translation)** to manage, but the fundamental problem remains: **we simply do not have enough IPv4 addresses** for the huge number of devices getting connected.

### 🎯 Exam Important Points

- IPv4 uses 32-bit addresses → limited to ~2^32 addresses
- Not all 2^32 addresses are usable (reserved, broadcast, loopback, subnetting overhead)
- Growth of IoT devices massively increases address demand
- NAT was a temporary workaround, not a permanent solution
- Address space saturation is the **primary reason** for IPv6

### ⚠️ Common Confusions

- Don't think IPv4 addresses ran out suddenly — it was a gradual saturation over years
- NAT helps reuse addresses but does NOT create new addresses; it is a patch, not a solution

---

## Concept 2: Problems with IPv4 (Why a New IP Structure?)

### 🧠 Simple Explanation

The need for IPv6 is not just about address shortage. IPv4 has **multiple other problems** too:

**Problem 1: Address Space Not Sufficient Even with CIDR**
Even after introducing CIDR (Classless Inter-Domain Routing), the address space is still not enough for the growing internet.

**Problem 2: No Mobility Support**
When IPv4 was designed, people only had fixed desktop computers. Nobody imagined mobile phones moving from one network to another. IPv4 **does not natively support mobility**. There is a patch called "Mobile IP" for IPv4, but it does not work well in real applications.

**Problem 3: No Direct Security Support**
During the early days of the internet, the prime requirement was **connectivity**, not security. The paper by David Clark on the history of DARPA internet protocol explains that connectivity was the topmost goal, while security, auditing, and logging were secondary. So IPv4 has **no built-in security** — security was added later as patches (like TLS/SSL at the transport layer, IPSec on top of IPv4).

**Problem 4: Quality of Service (QoS) Vaguely Defined**
Initially, the internet was only for data traffic. But now we transmit multimedia — voice over VoIP (like Skype), video streaming (YouTube, Netflix, Hotstar), live video calls. These applications need data delivered in a **delay-sensitive way**. If packets take too much time, the video/call breaks. In IPv4, QoS was **not a prime goal** and was vaguely defined.

**Problem 5: Unmanageable Complexity**
Because IPv4 did not have built-in support for QoS, security, and mobility, all these were added as separate patches. This made the overall protocol **too complex and unmanageable** for a large internet.

### 🎯 Exam Important Points

- Four main problems: insufficient address space, no mobility, no security, poor QoS
- Mobile IP is a "patch" on IPv4, not a native feature
- Security in IPv4 works through external add-ons (TLS/SSL, IPSec)
- QoS was vaguely defined because IPv4 was designed for plain data, not multimedia
- Connectivity was the prime goal when IPv4 was originally designed

### ⚠️ Common Confusions

- IPv4 *can* handle security and mobility, but only through patches/add-ons — it is not **built-in**
- QoS being "vaguely defined" does not mean it is absent; it means it was not well-designed for modern needs

---

## Concept 3: IPv6 Features

### 🧠 Simple Explanation

IPv6 was proposed in draft form around **December 1998** and became fully standardized around **July 2017** — it took about 10 years to finalize. Here are the key features of IPv6:

**Feature 1: Larger Address Space**
IPv6 uses **128-bit addresses** (compared to 32-bit in IPv4). This gives an astronomically large number of addresses — enough for every device on the planet and far beyond.

**Feature 2: Globally Unique and Hierarchical Addressing**
IPv6 is designed so that the entire internet can be structured **hierarchically**. In IPv4, the hierarchical structure broke down because we moved from classful to classless addressing and used NAT with private IPs. IPv6 restores a proper global hierarchy.

**Feature 3: Optimized Routing Table Using Prefixes**
Instead of routing based on address classes (like IPv4 classful addressing), IPv6 uses **prefix-based routing**. You look at the initial few bits of the address to determine where in the hierarchy the device belongs and route accordingly. This makes routing tables more efficient.

**Feature 4: Auto-Configuration of Network Interfaces**
When a device comes alive, it can **automatically get an IPv6 address** without needing a separate protocol. In IPv4, we needed DHCP (Dynamic Host Configuration Protocol) as a separate protocol. In IPv6, auto-configuration is **built into the protocol itself**.

**Feature 5: Support for Encapsulation**
IPv6 supports encapsulation of packets.

**Feature 6: Service Class Support for QoS**
IPv6 provides proper mechanisms to manage **quality of service classes** — supporting real-time multimedia traffic properly.

**Feature 7: Built-in Authentication and Encryption**
Security is **not a patch** in IPv6 — it is a built-in part of the protocol itself.

**Feature 8: Backward Compatibility with IPv4**
IPv6 is designed so that you can **gradually migrate** from IPv4 to IPv6 without breaking existing systems.

### 🎯 Exam Important Points

- IPv6 = 128-bit addresses (IPv4 = 32-bit)
- Key features: larger address space, hierarchical addressing, prefix-based routing, auto-configuration, encapsulation, QoS support, built-in security, IPv4 compatibility
- Auto-configuration in IPv6 replaces the role of DHCP in IPv4
- Security is **built-in** (not a patch like in IPv4)
- IPv6 draft: December 1998; Standardized: ~July 2017

### ⚠️ Common Confusions

- Auto-configuration does NOT mean DHCP is used inside IPv6 — it is a different mechanism that is part of IPv6 itself
- IPv6 does not just solve the address problem; it also fixes QoS, security, and mobility issues

---

## Concept 4: IPv6 Header Format

### 🧠 Simple Explanation

The IPv6 packet has a **mandatory header** (also called the base header). Let us understand each field:

| Field | Description |
|-------|-------------|
| **Version (4 bits)** | Protocol version — will be 6 for IPv6 |
| **Traffic Class (8 bits)** | Used for supporting **Quality of Service** — tells the network how to handle this packet based on its priority/class |
| **Flow Label (20 bits)** | Labels every flow based on its QoS classes — helps identify packets belonging to the same flow (like a video call) so they can be treated consistently |
| **Payload Length (16 bits)** | Length of the data (payload) carried after the header |
| **Next Header (8 bits)** | Points to the **next extension header** (or the transport layer header if no extension headers). This is how IPv6 chains multiple headers together |
| **Hop Limit (8 bits)** | How many hops (routers) the packet can traverse before being discarded. This is similar to **TTL (Time to Live)** in IPv4 |
| **Source Address (128 bits)** | IPv6 address of the sender |
| **Destination Address (128 bits)** | IPv6 address of the receiver |

### 🎯 Exam Important Points

- IPv6 header has: Version, Traffic Class, Flow Label, Payload Length, Next Header, Hop Limit, Source Address (128 bits), Destination Address (128 bits)
- **Hop Limit** in IPv6 = equivalent of TTL in IPv4
- **Next Header** field is used to point to extension headers (chaining mechanism)
- **Traffic Class** and **Flow Label** are for QoS support
- Source and Destination addresses are each **128 bits** (not 32 bits like IPv4)

### ⚠️ Common Confusions

- "Next Header" does NOT mean there is always another header — if there is no extension header, it points to the transport layer header (like TCP)
- Hop Limit is NOT time-based — it counts hops (routers), just like TTL in IPv4 counts hops

---

## Concept 5: Extension Headers in IPv6

### 🧠 Simple Explanation

Every IPv6 packet has **one mandatory header** (the base header we just studied). But in addition to that, the packet **can have multiple extension headers**. These extension headers carry additional/special information.

The base header has a **"Next Header" field** that acts as a pointer — it points to the first extension header. Each extension header then has its own "Next Header" field pointing to the next one, forming a **chain**.

Here are the types of extension headers mentioned in the transcript:

**1. Hop-by-Hop Options Header:** Tells every router along the path how to treat/handle the packet at each hop.

**2. Routing Header:** If you are using **source routing** (where the source decides the complete route), the entire routing information can be embedded in this header. The source puts the full route inside the IP packet itself.

**3. Fragment Header:** If an IPv6 packet gets **fragmented** into multiple pieces, the fragmentation information is stored in this header.

**4. Authentication Header:** Since security is built into IPv6, the authentication information (to verify the identity of the sender) can be placed in this optional header.

After all extension headers, you have the **TCP header** and then the actual **data**.

### 🎯 Exam Important Points

- IPv6 = 1 mandatory header + multiple optional extension headers
- Extension headers are **chained** using the "Next Header" field
- Types: Hop-by-Hop Options, Routing, Fragment, Authentication
- Routing header is used for **source routing** — source puts full route in the header
- Authentication header makes security **built-in** to IPv6

### ⚠️ Common Confusions

- Extension headers are **optional** — not every IPv6 packet has them
- The "chain" means each header points to the next; the base header points to the first extension header, which points to the second, and so on
- In IPv4, fragmentation is done by intermediate routers; in IPv6, fragmentation info goes in a separate extension header

---

## Concept 6: IPv6 Address Representation

### 🧠 Simple Explanation

An IPv6 address is **128 bits** long. Writing 128 bits in binary would be very long, so IPv6 uses **hexadecimal notation**. The 128 bits are divided into **8 groups of 16 bits each**, and each group is written as a **4-digit hexadecimal number**, separated by **colons**.

**Example:**
```
FE80:0000:0000:0000:0001:0800:23E7:F5DB
```

This is 8 groups, each with 4 hex digits, separated by colons.

### Shortening Rules

**Rule 1: Remove Leading Zeros**
Within any group, you can remove leading zeros. For example, `0001` becomes just `1`.

**Rule 2: Replace Consecutive Groups of All-Zeros with `::`**
If you have one or more consecutive groups that are all zeros, you can replace them with a **double colon (::)**. 

**But this rule can only be used ONCE in an address.** Why? Because if you use `::` in two places, you cannot figure out how many zero groups each `::` represents.

**Example of shortening:**
```
Full:      FE80:0000:0000:0000:0001:0800:23E7:F5DB
Step 1:    FE80:0:0:0:1:800:23E7:F5DB     (removed leading zeros)
Step 2:    FE80::1:800:23E7:F5DB           (replaced consecutive zeros with ::)
```

To expand back: You know there must be 8 groups total. Count the visible groups (FE80, 1, 800, 23E7, F5DB = 5 groups). So `::` represents 8 - 5 = 3 groups of zeros.

### 🎯 Exam Important Points

- IPv6 address = 128 bits = 8 groups of 16 bits each, written in hexadecimal, separated by colons
- Leading zeros within a group can be removed
- Consecutive all-zero groups can be replaced with `::` (double colon)
- **Double colon (`::`) can be used only ONCE** in an address
- Reason: if used twice, you cannot determine how many zeros each `::` represents

### ⚠️ Common Confusions

- `::` replaces **one or more consecutive groups** of all-zeros, not just one group
- A single `0` in a group means `0000` (all 16 bits are zero)
- You must count total groups to expand `::` back to full form

### 📝 Possible NPTEL-style Question

*Which of the following is the correct shortened form of FE80:0000:0000:0000:0001:0800:23E7:F5DB?*
Answer: FE80::1:800:23E7:F5DB

---

## Concept 7: IPv6 Address Space Allocation Based on Prefix

### 🧠 Simple Explanation

Just like IPv4 had classes (Class A, B, C, D, E), IPv6 divides its address space into **groups based on the prefix** (the starting bits of the address). The prefix determines what type of address it is.

Here are the key address types from the transcript:

| Prefix Bits | Prefix Notation | Address Type | Fraction of Total Space |
|-------------|----------------|--------------|------------------------|
| First 8 bits = 00000000 | 0000::/8 | **Reserved** | 1/256 |
| First 7 bits = 0000001 | 0200::/7 | **Reserved for NSAP** | 1/128 |
| First 7 bits = 0000010 | 0400::/7 | **Reserved for IPX** | 1/128 |
| First 3 bits = 001 | 2000::/3 | **Aggregatable Global Unicast** | **1/8** |
| First 10 bits = 1111111010 | FE80::/10 | **Link-Local Unicast** | 1/1024 |
| First 10 bits = 1111111011 | FEC0::/10 | **Site-Local Unicast** | 1/1024 |
| First 8 bits = 11111111 | FF00::/8 | **Multicast** | 1/256 |

The most important one is the **Aggregatable Global Unicast Address** (prefix 001, starting with 2000::). This is the address assigned to individual hosts in the network — the "regular" IPv6 address that devices use. It takes up **1/8th** of the total IPv6 address space, which is still an enormous number of addresses.

### 🎯 Exam Important Points

- IPv6 uses **prefix-based allocation** (similar to CIDR concept)
- Global Unicast Address starts with prefix **001** (hex range starting 2000::/3) — takes 1/8 of total space
- Link-Local Unicast starts with **FE80::/10**
- Multicast starts with **FF00::/8** (first 8 bits all 1s)
- The prefix length (e.g., /3, /7, /8, /10) tells how many initial bits define the address type

### ⚠️ Common Confusions

- Don't confuse IPv6 prefix-based allocation with IPv4 classful addressing — they are conceptually similar but IPv6 is more structured
- "1/8 of address space" sounds small but it is 1/8 of 2^128, which is astronomically large

---

## Concept 8: Global Unicast Address Format

### 🧠 Simple Explanation

The **Global Unicast Address** is the most commonly used IPv6 address type — this is what individual devices get. Its 128 bits are divided into **three parts**:

```
|  Global Routing Prefix (n bits)  |  Subnet ID (m bits)  |  Interface ID (128 - n - m bits)  |
```

**Part 1: Global Routing Prefix (n bits)**
This is a value assigned to a **site** (an organization or a group of subnets/links). The routing agencies design this prefix so that the entire internet can be structured **hierarchically**. By looking at the global routing prefix, routers can quickly determine which part of the global hierarchy the packet should go to.

**Part 2: Subnet ID (m bits)**
Within a site, you can have multiple subnets. The subnet ID identifies a specific subnet within that site.

**Part 3: Interface ID (128 - n - m bits)**
This identifies a specific **network interface** within a subnet — essentially the individual device.

The **prefix** in CIDR notation (like /48, /64 etc.) represents the combined length of the Global Routing Prefix + Subnet ID.

### 🎯 Exam Important Points

- Global Unicast = Global Routing Prefix + Subnet ID + Interface ID
- Global Routing Prefix provides **hierarchical structure** to the entire internet
- Subnet ID identifies a subnet within a site
- Interface ID identifies a specific interface/device within a subnet
- The prefix notation (e.g., /48) denotes Global Routing Prefix + Subnet ID length

### ⚠️ Common Confusions

- This is very similar to IPv4's Network ID + Host ID concept, but with an additional layer (Global Routing Prefix) for global hierarchy
- The Interface ID is NOT the same as a MAC address, though it can be derived from one

---

## Concept 9: ICMPv6 — Neighbor Discovery Protocol

### 🧠 Simple Explanation

In IPv4, when a device knows the IP address of another device on the same network but needs its **MAC address**, it uses **ARP (Address Resolution Protocol)**. ARP broadcasts a query to the entire network asking "Who has this IP?"

In IPv6, ARP is **replaced** by the **Neighbor Discovery Protocol**, which is part of **ICMPv6** (ICMP version 6). The purpose is the same: to find the MAC address corresponding to an IPv6 address.

Neighbor Discovery enables a node to:
- Identify other hosts and routers on its link (its local network)
- Know at least one router so it can forward packets to non-local destinations

### 🎯 Exam Important Points

- **Neighbor Discovery** in IPv6 replaces **ARP** in IPv4
- It is part of ICMPv6
- Purpose: map IPv6 address → MAC address (just like ARP maps IPv4 → MAC)
- A node needs to know at least one router to forward packets outside its local link

### ⚠️ Common Confusions

- ARP is IPv4 only; IPv6 uses Neighbor Discovery instead
- Neighbor Discovery is NOT a broadcast mechanism (unlike ARP) — this is explained in the next concept

---

## Concept 10: Neighbor Solicitation Message

### 🧠 Simple Explanation

When a node (say Node A) wants to find the MAC address of another node (say Node B), Node A sends a **Neighbor Solicitation** message.

The structure of this message includes:
- **Source Address:** IPv6 address of Node A
- **Destination Address:** Address of the **solicited node** (NOT a broadcast — this is a key difference from ARP!)
- **ICMPv6 Type:** 135 (this identifies it as a Neighbor Solicitation)
- **Target Address:** The IPv6 address whose MAC address you want to find (Node B's IPv6 address)
- **Source Link-Layer Address:** The MAC address of the sender (Node A's MAC)

**Key Difference from ARP:**
In IPv4 ARP, the query is **broadcast** to everyone on the network. In IPv6 Neighbor Discovery, the query is sent to a **specific solicited node**, NOT broadcast. Every node has an associated **solicited node** whose information is already known to the source. So instead of flooding the entire network, the query goes to a targeted node.

**Example from transcript:** If Nodes A, B, C, D are on a network, and A wants B's MAC address, A does NOT broadcast. Instead, A sends the Neighbor Solicitation to Node C (if C is the solicited node of A). Node C then helps find the path to B and informs A.

### 🎯 Exam Important Points

- Neighbor Solicitation = ICMPv6 Type **135**
- Sent to the **solicited node**, NOT broadcast (unlike ARP)
- Contains: source address, destination (solicited node), target IPv6 address, source MAC
- Each node has an associated **solicited node** — this avoids broadcast
- This is a major improvement over IPv4 ARP which uses broadcast

### ⚠️ Common Confusions

- "Solicited node" is a pre-known special node, NOT the destination node (Node B) itself
- IPv6 avoids broadcast for address resolution — this reduces network traffic compared to ARP

---

## Concept 11: Neighbor Advertisement Message

### 🧠 Simple Explanation

The **Neighbor Advertisement** message is the **response** to a Neighbor Solicitation. When the solicited node finds the answer (the MAC address of the target), it sends back a Neighbor Advertisement containing:
- **Target Address:** The IPv6 address that was queried
- **Target Link-Layer Address:** The MAC address corresponding to that IPv6 address

**Important:** Neighbor Advertisements are NOT only sent as responses. Every node also **periodically sends** Neighbor Advertisement messages on its own. This helps maintain and update the link connectivity — nodes keep their neighbors' information fresh.

**Three Flags in Neighbor Advertisement:**

| Flag | Meaning |
|------|---------|
| **R (Router)** | The sender of this advertisement is a **router** |
| **S (Solicitation)** | This advertisement is a **response to a solicitation** (not a periodic one) |
| **O (Override)** | The receiver must **update its cache** with this new information |

### 🎯 Exam Important Points

- Neighbor Advertisement = response to Neighbor Solicitation
- Contains target IPv6 address + target MAC address
- Also sent **periodically** (not only as a response) — to maintain link connectivity
- Three flags: **R** (router), **S** (response to solicitation), **O** (override/update cache)

### ⚠️ Common Confusions

- Periodic advertisements happen even without any solicitation — they keep neighbor information updated
- The O (Override) flag tells the receiver to replace old cached information with the new one

---

## Concept 12: IPv6 Mobility Support

### 🧠 Simple Explanation

One of the biggest advantages of IPv6 over IPv4 is **built-in mobility support**. Here's how it works:

When a mobile node **moves away** from its home network (home location), it uses a **temporary address** at the new location. But it stores its **home address** in the IPv6 Destination Optional Header. This way, the node remembers where it originally belonged.

The mobile station can use the **Routing Header** to list all the routing information for packets to follow a particular path. This helps establish a connection with the service provider network even after moving.

Packets sent to a mobile node can be **tunneled** (wrapped inside another header) using IPv6 routing headers to reach the node at its new location.

**Key advantage over IPv4 mobility:**
In IPv4, you needed a **Foreign Agent** — a designated router in the new network that creates a mapping between the original address and the new address. In IPv6, **no Foreign Agent is needed**. The **Neighbor Discovery Protocol** and **Address Auto-Configuration** together allow a node to directly connect to any new subnet and get a new IPv6 address automatically.

### 🎯 Exam Important Points

- Mobile node uses a **temporary address** when away from home
- Home address stored in **IPv6 Destination Optional Header**
- Routing header lists routing info for packet forwarding
- Packets can be **tunneled** using IPv6 routing headers
- **No Foreign Agent needed** in IPv6 (unlike IPv4's Mobile IP)
- Neighbor Discovery + Auto-Configuration handle mobility directly

### ⚠️ Common Confusions

- In IPv4, Foreign Agent is mandatory for mobility; IPv6 eliminates this need entirely
- Tunneling in this context means wrapping the packet with routing headers to redirect it to the mobile node's new location

---

## Concept 13: Migrating from IPv4 to IPv6 — Dual Stack

### 🧠 Simple Explanation

You cannot switch the entire internet from IPv4 to IPv6 in a single day. Most machines currently support IPv4, and the migration has to be **gradual**. There are **three methods** to migrate.

**Method 1: Dual Stack**

In dual stack, a single machine has support for **both IPv4 and IPv6** in its protocol stack. Think of it as having two engines in one car — you choose which one to use based on the road.

- If the machine needs to communicate with an **IPv4 host**, it uses the IPv4 part of its stack
- If the machine needs to communicate with an **IPv6 host**, it uses the IPv6 part of its stack

This means the machine must maintain **both** the IPv4 stack and the IPv6 stack simultaneously.

### 🎯 Exam Important Points

- Dual stack = both IPv4 and IPv6 supported in the same machine
- Machine chooses which stack to use based on the destination
- Simple but requires maintaining two full protocol stacks

### ⚠️ Common Confusions

- Dual stack does NOT convert addresses — it runs both protocols side by side
- Both stacks need to be fully functional — it is not just about addresses, but the entire protocol processing

---

## Concept 14: Migrating from IPv4 to IPv6 — Tunneling

### 🧠 Simple Explanation

**Method 2: Tunneling**

Tunneling means you **wrap (encapsulate)** one type of header inside another.

If you have an IPv4 packet that needs to travel through an IPv6 network, you add an **IPv6 header** around the IPv4 header. When the packet reaches an IPv4 host, the IPv4 part is read. When it reaches an IPv6 host, the IPv6 part is read.

This works in both directions — you can tunnel IPv4 through IPv6 and vice versa.

Think of it like putting a letter (IPv4 packet) inside a larger envelope (IPv6 header) to send it through a postal system that only handles large envelopes (IPv6 network). At the destination, you open the large envelope and read the original letter.

### 🎯 Exam Important Points

- Tunneling = encapsulating one IP header inside another
- IPv4 header can be wrapped inside IPv6 header and vice versa
- Allows packets to traverse networks that use a different IP version
- The appropriate header is read based on the destination type

### ⚠️ Common Confusions

- Tunneling does NOT change the original packet — it adds an extra header around it
- The original header is preserved and read at the appropriate destination

---

## Concept 15: Migrating from IPv4 to IPv6 — Header Translation

### 🧠 Simple Explanation

**Method 3: Header Translation**

Header translation means you actually **convert** an IPv4 header into an IPv6 header (or vice versa). You take the values from the IPv4 header and create a corresponding IPv6 header by converting those values into IPv6 format.

An important requirement is **address translation** — you must be able to convert between IPv4 and IPv6 addresses.

**IPv6 to IPv4 conversion:**
Take the **lower-order 32 bits** of the IPv6 address to create the IPv4 address.

**IPv4 to IPv6 conversion:**
You have 32 bits from IPv4 and need 128 bits for IPv6. You add **96 bits** before the 32-bit address. These 96 bits consist of all zeros followed by FFFF. The prefix used is **::FFFF/96**.

### Address Translation Example (from transcript)

**IPv4 to IPv6:**
IPv4 address: `202.141.80.20` (decimal)

Convert each pair of bytes to hex:
- 202 = CA, 141 = 8D → first hex group: CA8D
- 80 = 50, 20 = 14 → second hex group: 5014

IPv6 address: `::FFFF:CA8D:5014`
(all zeros for the first 80 bits, then FFFF, then the IPv4 address in hex)

**IPv6 to IPv4:**
Take the last 32 bits of the IPv6 address and convert to decimal.
If the last portion is `FE80:2381`:
- FE = 254, 80 = 128 → 254.128
- 23 = 35, 81 = 129 → 35.129

IPv4 address: `254.128.35.129`

### 🎯 Exam Important Points

- Header Translation = convert IPv4 header to IPv6 header or vice versa
- Address translation is mandatory during header translation
- **IPv6 → IPv4:** Take lower-order 32 bits
- **IPv4 → IPv6:** Append ::FFFF/96 prefix (96 bits of 0s and Fs before the 32-bit address)
- Conversion between decimal (IPv4) and hexadecimal (IPv6) is needed

### ⚠️ Common Confusions

- **Tunneling** wraps the header (both headers exist); **Header Translation** replaces the header (only one header exists after conversion)
- The ::FFFF prefix is specific to IPv4-mapped IPv6 addresses
- Remember: IPv4 uses decimal notation, IPv6 uses hexadecimal notation

---

## Concept 16: Summary of Three Migration Methods

### 🧠 Simple Explanation

Here is a quick comparison of all three migration approaches:

| Method | How It Works | Key Feature |
|--------|-------------|-------------|
| **Dual Stack** | Machine runs both IPv4 and IPv6 stacks | Uses the appropriate stack based on destination |
| **Tunneling** | Wraps one header inside another | Both headers preserved, appropriate one read at destination |
| **Header Translation** | Converts one header type to another | Only one header remains after conversion; address translation required |

### 🎯 Exam Important Points

- Three migration methods: Dual Stack, Tunneling, Header Translation
- Dual Stack = both stacks on same machine
- Tunneling = encapsulation (both headers exist)
- Header Translation = conversion (one header replaces the other)
- Address translation (IPv6 ↔ IPv4) is needed for Header Translation

---

## Concept 17: Important References Mentioned

### 🧠 Simple Explanation

The transcript mentions several RFCs and resources for deeper study of IPv6:

- **RFC 2460** — Internet Protocol, Version 6 (IPv6) — December 1998
- **RFC 4291** — IP Version 6 Addressing Architecture — February 2006
- **RFC 3587** — IPv6 Global Unicast Address Format — August 2003
- **IANA Documentation** — IPv6 Multicast Addresses
- **6NET Project** — Website with white papers and documentation about IPv6 design and development

The professor notes that this lecture is a **brief introduction** to IPv6 and there are many more details in these RFCs.

### 🎯 Exam Important Points

- Know the RFC numbers: RFC 2460 (IPv6 base), RFC 4291 (addressing architecture), RFC 3587 (global unicast format)
- 6NET is a project that worked on IPv6 design and development
- This lecture covers only the basics — the full IPv6 specification is much more detailed

---

# 📝 10 MCQs — Lecture 30: IPv6 Addressing

---

**Q1. What is the size of an IPv6 address?**

(a) 32 bits
(b) 64 bits
(c) 128 bits
(d) 256 bits

**Answer: (c) 128 bits**
Explanation: IPv4 uses 32-bit addresses. IPv6 uses 128-bit addresses, providing a much larger address space. This is explicitly stated in the transcript as one of the key features of IPv6.

---

**Q2. Which of the following is NOT a problem with IPv4 as mentioned in the transcript?**

(a) Address space is insufficient even with CIDR
(b) No built-in mobility support
(c) IPv4 does not support any form of routing
(d) No direct security support

**Answer: (c) IPv4 does not support any form of routing**
Explanation: IPv4 certainly supports routing — that is its core function. The problems with IPv4 mentioned in the transcript are: insufficient address space, no native mobility, no built-in security, and vague QoS definition.

---

**Q3. How is an IPv6 address represented?**

(a) 4 decimal numbers separated by dots
(b) 8 hexadecimal groups of 16 bits each, separated by colons
(c) 6 hexadecimal groups separated by hyphens
(d) 16 binary groups separated by colons

**Answer: (b) 8 hexadecimal groups of 16 bits each, separated by colons**
Explanation: The transcript states that 128-bit IPv6 addresses are represented in 8 hexadecimal numbers (each 16 bits), separated by colons. Example: FE80:0000:0000:0000:0001:0800:23E7:F5DB.

---

**Q4. How many times can the double colon (::) be used in a single IPv6 address?**

(a) Unlimited times
(b) Twice
(c) Only once
(d) Three times

**Answer: (c) Only once**
Explanation: The transcript clearly states that the double colon (::) can be used only once. If used twice, it would be impossible to determine how many groups of zeros each `::` represents.

---

**Q5. What replaces ARP in IPv6?**

(a) DHCP
(b) NAT
(c) Neighbor Discovery Protocol (ICMPv6)
(d) RARP

**Answer: (c) Neighbor Discovery Protocol (ICMPv6)**
Explanation: The transcript says Neighbor Discovery in IPv6 is the replacement of ARP in IPv4. It enables a node to identify other hosts and routers on its links and find the MAC address corresponding to an IPv6 address.

---

**Q6. What is the ICMPv6 type number for a Neighbor Solicitation message?**

(a) 128
(b) 130
(c) 135
(d) 140

**Answer: (c) 135**
Explanation: The transcript explicitly states that in the ICMP message, type 135 means it is a Neighbor Solicitation message.

---

**Q7. Which of the following flags in a Neighbor Advertisement message indicates that the sender is a router?**

(a) S flag
(b) O flag
(c) R flag
(d) F flag

**Answer: (c) R flag**
Explanation: As per the transcript, the three flags are: R (sender is a router), S (advertisement is a response to a solicitation), and O (override — must update cached information).

---

**Q8. In IPv6 migration, what does "Dual Stack" mean?**

(a) Using two separate machines for IPv4 and IPv6
(b) A single machine supports both IPv4 and IPv6 protocol stacks
(c) Converting IPv4 headers to IPv6 headers
(d) Wrapping IPv4 packets inside IPv6 packets

**Answer: (b) A single machine supports both IPv4 and IPv6 protocol stacks**
Explanation: The transcript says in dual stack support, you have support for both IPv4 and IPv6 in the same protocol stack. The machine uses IPv4 stack to communicate with IPv4 hosts and IPv6 stack for IPv6 hosts.

---

**Q9. When converting an IPv4 address to an IPv6 address using header translation, what prefix is appended?**

(a) ::FF/64
(b) ::FFFF/96
(c) ::0000/96
(d) ::AAAA/96

**Answer: (b) ::FFFF/96**
Explanation: The transcript states that to convert from IPv4 to IPv6, you have 32 bits and need 96 more bits. You append ::FFFF/96 prefix — zeros for the first 80 bits followed by FFFF, then the 32-bit IPv4 address.

---

**Q10. Which of the following is TRUE about IPv6 mobility support?**

(a) It requires a Foreign Agent like IPv4
(b) The mobile node uses its home address everywhere
(c) It does not require Foreign Agents — Neighbor Discovery and auto-configuration handle it
(d) IPv6 does not support mobility

**Answer: (c) It does not require Foreign Agents — Neighbor Discovery and auto-configuration handle it**
Explanation: The transcript explicitly states that IPv6 does not require Foreign Agents like IPv4. Instead, Neighbor Discovery protocol and address auto-configuration can be used to connect a node with any network directly.

---

*End of Lecture 30 Study Guide*
