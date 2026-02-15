# Lecture 27: IP Addressing (IPv4) I — Classful Addressing

## Course: Computer Networks and Internet Protocol
### Prof. Sandip Chakraborty, IIT Kharagpur

---

## Concept 1: Need for IP Addressing

📌 **Concept Name:** Why Do We Need IP Addresses?

🧠 **Simple Explanation:**
In the previous lectures, the course covered autonomous systems and internet service providers. Now the question is: **how do you uniquely identify a specific host (computer/device) inside a network?** That is the job of an IP address.

Think of it like this — if someone wants to send you a letter, they need your full postal address. Similarly, if a computer wants to send data to another computer on the internet, it needs the destination computer's IP address.

The IP address has **two jobs**:
1. **Identify the network** the host belongs to (like the city/state in a postal address)
2. **Identify the specific host** inside that network (like the person's name + house number)

This is a **hierarchical addressing** scheme — just like postal mail goes from Country → State → City → Street → Person, IP addresses go from Network → Host.

🛠 **Real-world Example (from transcript):**
The professor gives the postal address analogy: India → West Bengal → Kharagpur → IIT Kharagpur → Sandip Chakraborty. This hierarchical approach uniquely identifies one person. Similarly, the IP address hierarchy uniquely identifies one host in the entire internet.

🎯 **Exam Important Points:**
- IP address identifies **both the network AND the host**
- It is a hierarchical addressing mechanism
- Two components: **Network address part** + **Host address part**

⚠️ **Common Confusions:**
- IP address is NOT just a host identifier — it also contains the network identity
- Do not confuse IP address with MAC address. IP address works at the Network Layer; MAC address works at Data Link Layer

---

## Concept 2: IPv4 Address Basics — 32 Bits

📌 **Concept Name:** IPv4 Address Structure

🧠 **Simple Explanation:**
IPv4 stands for **Internet Protocol version 4**. It is the traditional and most widely used version of IP addressing.

An IPv4 address is a **32-bit binary number**. These 32 bits are divided into two parts:
- **Network address part** — identifies which network
- **Host address part** — identifies which host inside that network

So the total address space is 32 bits, and the question is: how do we divide these 32 bits between the network part and the host part?

🎯 **Exam Important Points:**
- IPv4 address = **32 bits** total
- Divided into: Network address + Host address
- IPv4 is the most commonly used IP version
- IPv6 is the newer version (covered in later lectures)

⚠️ **Common Confusions:**
- IPv4 is **not outdated** — it is still the most used protocol
- The 32-bit size is fixed — what changes is how you divide those bits between network and host

---

## Concept 3: Classful Addressing — The Old Approach

📌 **Concept Name:** What is Classful Addressing?

🧠 **Simple Explanation:**
The old way of dividing the 32-bit address into network and host parts was called **classful addressing**. In classful addressing, you have a **fixed number of bits** for the network address part and the remaining bits are for the host address part.

The entire address space was divided into **5 classes**: Class A, Class B, Class C, Class D, and Class E.

Each class has a fixed boundary between the network bits and the host bits. Although classful addressing is **not used today**, understanding it is essential because modern classless addressing (CIDR) evolved from it.

🎯 **Exam Important Points:**
- Classful addressing = **fixed division** between network and host bits
- 5 classes: A, B, C, D, E
- This is the "old" approach — now replaced by classless addressing (CIDR)
- Still important to understand for conceptual clarity

---

## Concept 4: Identifying Classes Using First Few Bits

📌 **Concept Name:** How to Identify Which Class an IP Address Belongs To

🧠 **Simple Explanation:**
When you look at a 32-bit IP address, how do you know which class it belongs to? You look at the **first few bits** of the address:

| Class | Starting Bits | How to Identify |
|-------|--------------|-----------------|
| Class A | **0** | First bit is 0 |
| Class B | **10** | First two bits are 10 |
| Class C | **110** | First three bits are 110 |
| Class D | **1110** | First four bits are 1110 |
| Class E | **1111** | First four bits are 1111 |

The key property is: **none of these identifiers is a proper prefix of another**. This means you can identify the class by scanning bits one by one:
- If first bit = 0 → it's Class A (stop)
- If first bit = 1, check second bit:
  - If second bit = 0 → it's Class B (stop)
  - If second bit = 1, check third bit:
    - If third bit = 0 → it's Class C (stop)
    - If third bit = 1, check fourth bit:
      - If fourth bit = 0 → it's Class D
      - If fourth bit = 1 → it's Class E

This is done using a **bit shift operation** and **logical AND** operation.

🎯 **Exam Important Points:**
- **Memorize the starting bits for each class** — this is highly exam-important
- Class A: 0, Class B: 10, Class C: 110, Class D: 1110, Class E: 1111
- No prefix is a proper prefix of another → allows unique identification by scanning
- Identification uses bit shift and logical AND operations

⚠️ **Common Confusions:**
- Class D uses 4 bits (1110), NOT 3 bits
- Class E also uses 4 bits (1111)
- Don't confuse the starting bits pattern — Class B starts with "10" not "01"

---

## Concept 5: Class D — Multicast Addresses

📌 **Concept Name:** Multicast Addresses (Class D)

🧠 **Simple Explanation:**
Sometimes you don't want to send a packet to just one destination. You want to send it to a **group of destinations** at once. This is called **multicast**.

In multicast, a group of machines is identified by a **single address**. When you send a packet to that address, **all machines in the group receive it**.

Class D addresses are reserved for multicast. They are **not** divided into network and host parts.

**Range of Class D:** 224.0.0.0 to 239.255.255.255

🛠 **Real-world Example (from transcript):**
The professor explains: if you want to send a letter to all B.Tech students of the CSE department at IIT Kharagpur, you write the address as "To all B.Tech students, Dept. of CSE, IIT Kharagpur." A copy goes to every student in that group. That's multicast.

🎯 **Exam Important Points:**
- Class D = **Multicast addresses**
- Starting bits: **1110**
- Range: **224.0.0.0 to 239.255.255.255**
- NOT divided into network and host parts
- One address → packet delivered to all group members

---

## Concept 6: Class E — Reserved for Future Use

📌 **Concept Name:** Class E Addresses

🧠 **Simple Explanation:**
Class E addresses are **reserved for future use**. They are not normally used in regular networking.

**Range of Class E:** 240.0.0.0 to 255.255.255.255

Starting bits: **1111**

🎯 **Exam Important Points:**
- Class E = **Reserved for future use**
- Starting bits: **1111**
- Range: **240.0.0.0 to 255.255.255.255**
- Not used in normal networking

---

## Concept 7: Dotted Decimal Notation

📌 **Concept Name:** How IP Addresses Are Written — Dotted Decimal Format

🧠 **Simple Explanation:**
A 32-bit IP address is very long in binary form. So we use a more human-friendly representation called **dotted decimal notation**.

Here's how it works:
1. Take the 32-bit address
2. Divide it into **four chunks of 8 bits each** (called octets)
3. Convert each 8-bit chunk into its **decimal equivalent**
4. Separate the four decimal numbers with **dots**

**Example:** The binary `10111011.10001000.01100111.10101100` becomes `187.136.103.172` (approximately).

Since each chunk is 8 bits, each decimal number ranges from **0 to 255** (because 2^8 - 1 = 255).

So any IPv4 address in dotted decimal looks like: **X.X.X.X** where each X is between 0 and 255.

🎯 **Exam Important Points:**
- 32 bits divided into **4 octets (8 bits each)**
- Each octet is written as a decimal number (0 to 255)
- Separated by dots → called **dotted decimal notation**
- Example format: 203.110.30.42

⚠️ **Common Confusions:**
- Each octet goes from 0 to 255, NOT 0 to 256
- 255 = all 1's in 8 bits (11111111)
- 0 = all 0's in 8 bits (00000000)

---

## Concept 8: Network and Host Division for Classes A, B, and C

📌 **Concept Name:** How Classes A, B, and C Divide Network and Host Bits

🧠 **Simple Explanation:**
Only Class A, B, and C are used for regular unicast communication. Each has a different division between network bits and host bits:

### Class A:
- **Network address:** 8 bits (1 bit reserved for class identifier "0" + 7 usable bits)
- **Host address:** 24 bits
- Supports very **large number of hosts** per network (close to 2^24)

### Class B:
- **Network address:** 16 bits (2 bits reserved for class identifier "10" + 14 usable bits)
- **Host address:** 16 bits
- Supports close to **2^16 hosts** per network

### Class C:
- **Network address:** 24 bits (3 bits reserved for class identifier "110" + 21 usable bits)
- **Host address:** 8 bits
- Supports close to **2^8 hosts** per network

**Summary Table:**

| Class | Network Bits | Host Bits | Reserved Bits for Class ID | Usable Network Bits |
|-------|-------------|-----------|---------------------------|-------------------|
| A | 8 | 24 | 1 (0) | 7 |
| B | 16 | 16 | 2 (10) | 14 |
| C | 24 | 8 | 3 (110) | 21 |

🎯 **Exam Important Points:**
- Class A: 8 network bits, **24 host bits** → supports most hosts
- Class B: 16 network bits, **16 host bits**
- Class C: 24 network bits, **8 host bits** → supports fewest hosts
- The reserved bits for class identification reduce the usable network bits

⚠️ **Common Confusions:**
- The reserved class identifier bits are **part of the network bits**, not separate
- Class A has the most hosts but the fewest networks; Class C has the most networks but fewest hosts

---

## Concept 9: Network Address — All 0's in Host Part

📌 **Concept Name:** Network Address (Special Address)

🧠 **Simple Explanation:**
Every network has a special address called the **network address**. This address is used to **identify the network itself**, not any specific host.

How do you get the network address? **Put all 0's in the host address part.**

**Example — Class A:**
If the network part is `01111110` (which is 126 in decimal), and you put all 0's in the 24-bit host part:
- Binary: 01111110.00000000.00000000.00000000
- Dotted decimal: **126.0.0.0** — This is the network address

**Example — Class B:**
If the network part gives `189.233` and you put all 0's in the 16-bit host part:
- Dotted decimal: **189.233.0.0** — This is the network address

The network address **cannot be assigned to any host**. It is reserved.

🎯 **Exam Important Points:**
- Network address = **all 0's in the host part**
- Used to **uniquely identify a network**
- **Cannot be assigned** to any host
- The usefulness of network address is seen in routing

---

## Concept 10: Broadcast Address — All 1's in Host Part

📌 **Concept Name:** Broadcast Address (Special Address)

🧠 **Simple Explanation:**
Every network also has a **broadcast address**. If you send a packet with the broadcast address as the destination, the packet is delivered to **ALL hosts in that network**.

How do you get the broadcast address? **Put all 1's in the host address part.**

**Example — Class A:**
- Network address: 126.0.0.0
- Broadcast address: **126.255.255.255** (all 1's in 24-bit host part → 255.255.255)

**Example — Class B:**
- Network address: 189.233.0.0
- Broadcast address: **189.233.255.255** (all 1's in 16-bit host part → 255.255)

The broadcast address **cannot be assigned to any host**. It is reserved.

🎯 **Exam Important Points:**
- Broadcast address = **all 1's in the host part**
- Packet sent to broadcast address → **delivered to ALL hosts** in that network
- **Cannot be assigned** to any host
- All 1's in host part = 255 for each octet in the host portion

---

## Concept 11: Calculating Number of Valid Hosts

📌 **Concept Name:** How Many Valid Hosts Can a Class Support?

🧠 **Simple Explanation:**
Since two addresses are reserved in every network (network address with all 0's and broadcast address with all 1's), you must subtract 2 from the total possible addresses.

**Formula:** Valid hosts = **2^(number of host bits) − 2**

| Class | Host Bits | Total Possible | Valid Hosts |
|-------|-----------|---------------|-------------|
| A | 24 | 2^24 = 16,777,216 | **2^24 − 2 = 16,777,214** |
| B | 16 | 2^16 = 65,536 | **2^16 − 2 = 65,534** |
| C | 8 | 2^8 = 256 | **2^8 − 2 = 254** |

The "−2" accounts for:
1. **All 0's** → Network address (reserved)
2. **All 1's** → Broadcast address (reserved)

🎯 **Exam Important Points:**
- **Formula: 2^n − 2** (where n = number of host bits) — Very important for exam!
- Class A: 2^24 − 2 valid hosts
- Class B: 2^16 − 2 valid hosts
- Class C: 2^8 − 2 = **254** valid hosts
- The −2 is because all 0's (network) and all 1's (broadcast) are reserved

⚠️ **Common Confusions:**
- It is "−2" not "−1" — two addresses are reserved, not one
- Students often forget to subtract 2 and write 2^n as the answer

📝 **Possible NPTEL-style Question:**
"How many valid hosts can be there in a Class C IP address?" → Answer: 2^8 − 2 = 254

---

## Concept 12: The Problem with Classful Addressing

📌 **Concept Name:** Why Classful Addressing is Wasteful

🧠 **Simple Explanation:**
The professor gives a very important example to show why classful addressing fails:

**Scenario:** You have **255 hosts** in a network. Which class should you use?

**Try Class C:**
- Class C supports 2^8 − 2 = **254 valid hosts**
- But you need 255 hosts → **Class C is not enough!**

**Try Class B:**
- Class B supports 2^16 − 2 = **65,534 valid hosts**
- You only need 255 → You're only using 255 out of 65,534 possible addresses
- **Huge waste of address space!**

This is the **major problem with classful addressing**: you either don't have enough addresses (Class C) or you waste a huge number of addresses (Class B). There is no middle ground because the class boundaries are fixed at 8, 16, or 24 bits.

🎯 **Exam Important Points:**
- Classful addressing leads to **wastage of IP address space**
- Fixed boundaries (8, 16, 24 bits) are too rigid
- Example: 255 hosts → Class C can't handle it, Class B wastes too much
- This problem led to the development of **classless addressing (CIDR)**

⚠️ **Common Confusions:**
- The problem is not that classful addressing "doesn't work" — it works but is **inefficient**
- The jump from 254 hosts (Class C) to 65,534 hosts (Class B) is too big

---

## Concept 13: Introduction to CIDR — Classless Inter-Domain Routing

📌 **Concept Name:** What is CIDR?

🧠 **Simple Explanation:**
Because classful addressing wastes address space, a new approach was developed: **CIDR (Classless Inter-Domain Routing)**.

The core idea of CIDR is: **remove the fixed class boundaries**. Instead of being restricted to 8, 16, or 24 bits for the network part, you can use **any number of bits**.

CIDR achieves this through two mechanisms:
1. **Subnetting** — divide a large network into multiple smaller networks
2. **Supernetting** — combine multiple small networks into one larger network

So instead of being stuck with rigid classes, you can **customize the size** of the network to match your actual needs.

🎯 **Exam Important Points:**
- CIDR = **Classless Inter-Domain Routing**
- Removes fixed class boundaries
- Allows **variable-length** network/host division
- Two key concepts: **Subnetting** and **Supernetting**
- Solves the address wastage problem of classful addressing

---

## Concept 14: Subnetting

📌 **Concept Name:** Subnetting — Dividing a Large Network

🧠 **Simple Explanation:**
Subnetting means **breaking a large network into multiple smaller sub-networks (subnets)**.

In classful addressing, you had:
- Network Prefix | Host Number

With subnetting, the host part is further divided:
- Network Prefix | **Subnet Number** | Host Number

So some bits from the host part are "borrowed" to create the subnet number. The **original network prefix + subnet number** together form the **Subnet IP**.

This creates a **hierarchical structure**: multiple subnets combine to form a network, and that network can act as a subnet in an even larger network, and so on.

🎯 **Exam Important Points:**
- Subnetting = divide a large network into **smaller subnets**
- Bits are taken from the **host field** to create the subnet field
- Network Prefix + Subnet Number = **Subnet IP**
- Creates a hierarchical network structure

---

## Concept 15: Supernetting

📌 **Concept Name:** Supernetting — Combining Small Networks

🧠 **Simple Explanation:**
Supernetting is the **opposite of subnetting**. It means **combining multiple small networks into a single larger network**.

If you have several small networks that don't need many addresses each, you can merge them into one bigger network for efficient management and routing.

Subnetting and supernetting together form the basis of **CIDR**.

🎯 **Exam Important Points:**
- Supernetting = combine **multiple small networks into one large network**
- Opposite of subnetting
- Subnetting + Supernetting = Foundation of CIDR

---

## Concept 16: Subnet Mask

📌 **Concept Name:** What is a Subnet Mask?

🧠 **Simple Explanation:**
In classful addressing, you knew the boundary between network and host because the classes were fixed. But in CIDR, the boundary is **variable**. So how do you tell the system where the network part ends and the host part begins?

That's where the **subnet mask** comes in.

A subnet mask is a **32-bit binary number** where:
- **Consecutive 1's** on the left → represent the network/subnet part
- **Consecutive 0's** on the right → represent the host part

The number of 1's tells you exactly how many bits are used for the network/subnet address.

**Example:**
- Subnet mask: `11111111.11111111.00000000.00000000`
- This means: first 16 bits = network, remaining 16 bits = host
- In dotted decimal: **255.255.0.0**

In classful terms:
- Class A subnet mask would be: 255.0.0.0 (8 bits for network)
- Class B subnet mask would be: 255.255.0.0 (16 bits for network)
- Class C subnet mask would be: 255.255.255.0 (24 bits for network)

But with CIDR, you can have **any number** like 12 bits, 20 bits, etc.

🎯 **Exam Important Points:**
- Subnet mask = **32-bit number** with consecutive 1's followed by consecutive 0's
- 1's = network portion, 0's = host portion
- Tells the system where the network/host boundary is
- Also called **netmask** (in Linux)
- **subnet mask** (in Windows) and **netmask** (in Linux) are the same thing

⚠️ **Common Confusions:**
- Subnet mask is NOT the same as the IP address — it's a separate value
- The 1's in a subnet mask must be **consecutive** (you can't have 1's, then 0's, then 1's)

---

## Concept 17: CIDR Notation — Slash Notation

📌 **Concept Name:** CIDR Addressing Format (Slash Notation)

🧠 **Simple Explanation:**
CIDR introduces a compact way to write IP addresses with their subnet information using **slash notation**.

**Format:** IP_address **/** number

The number after the slash tells you **how many bits are used for the network part** (i.e., how many 1's are in the subnet mask).

**Example:** 191.180.83.235 **/12**
- The first **12 bits** are the network address
- The remaining **32 − 12 = 20 bits** are the host address
- The subnet mask would be: first 12 bits are 1's, remaining 20 bits are 0's
  - In binary: `11111111.11110000.00000000.00000000`
  - In dotted decimal: **255.240.0.0**

🎯 **Exam Important Points:**
- CIDR notation: **IP_address/n** where n = number of network bits
- /n means the subnet mask has **n consecutive 1's**
- Host bits = **32 − n**
- Example: /24 means 24 network bits, 8 host bits (like old Class C)
- Example: /12 means 12 network bits, 20 host bits

⚠️ **Common Confusions:**
- The /n is NOT part of the IP address — it describes the subnet mask
- /24 is not the same as Class C (though they use the same number of bits, CIDR is classless)

---

## Concept 18: CIDR Example — Determining Subnet IP from Address and Mask

📌 **Concept Name:** How to Find the Network/Subnet IP Using CIDR

🧠 **Simple Explanation:**
Given an IP address and a subnet mask, you can determine which subnet a host belongs to.

**Example from transcript:**
- IP address in 4 octets with subnet mask showing first 12 bits as 1's
- The subnet mask: `11111111.11110000.00000000.00000000`
- The first 12 bits of the IP address = **subnet IP**
- The remaining 20 bits = host address

In classful addressing, boundaries were at 8, 16, or 24 bits. In CIDR, the boundary can be **anywhere** (like 12 bits in this example).

🎯 **Exam Important Points:**
- To find subnet IP: Apply subnet mask to IP address (logical AND operation)
- Subnet mask with all 1's tells which bits belong to the network
- CIDR allows boundaries at **any bit position**, not just 8/16/24

---

## Concept 19: Manual IP Setting in Operating Systems

📌 **Concept Name:** Setting IP Address and Subnet Mask in OS

🧠 **Simple Explanation:**
When you manually configure an IP address on your computer (instead of getting it automatically), you need to provide two things:
1. **IP address** — e.g., 192.168.1.50
2. **Subnet mask** — e.g., 255.255.255.0

**Example from transcript:**
- IP address: 192.168.1.50
- Subnet mask: 255.255.255.0
- This means: first 24 bits = network IP, remaining 8 bits = host address
- Network IP = **192.168.1.0**
- Host number = **50**
- So this IP identifies **host number 50** in the **192.168.1.0 network**

**Terminology difference:**
- In **Windows**, it is called **subnet mask**
- In **Linux**, it is called **netmask**
- Both mean the same thing!

🎯 **Exam Important Points:**
- Manual IP configuration requires: IP address + Subnet mask
- Subnet mask 255.255.255.0 means /24 (24 network bits)
- Windows uses "subnet mask", Linux uses "netmask" — **same concept**
- From IP + mask, you can determine which network the host belongs to

---

## Summary Table: Classes at a Glance

| Feature | Class A | Class B | Class C | Class D | Class E |
|---------|---------|---------|---------|---------|---------|
| Starting Bits | 0 | 10 | 110 | 1110 | 1111 |
| Network Bits | 8 | 16 | 24 | N/A | N/A |
| Host Bits | 24 | 16 | 8 | N/A | N/A |
| Valid Hosts | 2^24 − 2 | 2^16 − 2 | 2^8 − 2 = 254 | N/A | N/A |
| Purpose | Unicast | Unicast | Unicast | Multicast | Reserved |
| Range Start | 0.0.0.0 | 128.0.0.0 | 192.0.0.0 | 224.0.0.0 | 240.0.0.0 |

---

## Key Formulas from This Lecture

| Formula | Meaning |
|---------|---------|
| Valid hosts = **2^n − 2** | n = number of host bits; subtract 2 for network address and broadcast address |
| Host bits = **32 − network bits** | In CIDR: if /n notation, host bits = 32 − n |

---

# 10 MCQs — Lecture 27

### Q1. An IPv4 address is how many bits long?
- (A) 16 bits
- (B) 24 bits
- (C) 32 bits
- (D) 64 bits

**Answer: (C) 32 bits**
Explanation: As stated in the transcript, IPv4 uses a 32-bit address to identify a host. This 32-bit space is divided into network and host parts.

---

### Q2. Which class of IP address starts with the bit pattern "110"?
- (A) Class A
- (B) Class B
- (C) Class C
- (D) Class D

**Answer: (C) Class C**
Explanation: Class identification is done by the first few bits. Class A starts with 0, Class B starts with 10, Class C starts with 110, Class D starts with 1110, and Class E starts with 1111.

---

### Q3. What is the purpose of Class D addresses?
- (A) Unicast communication
- (B) Reserved for future use
- (C) Multicast communication
- (D) Broadcast communication

**Answer: (C) Multicast communication**
Explanation: Class D addresses (range 224.0.0.0 to 239.255.255.255) are reserved for multicast, where a packet is sent to a group of machines identified by a single address.

---

### Q4. How many valid hosts can a Class C network support?
- (A) 256
- (B) 255
- (C) 254
- (D) 252

**Answer: (C) 254**
Explanation: Class C has 8 host bits. Valid hosts = 2^8 − 2 = 256 − 2 = 254. The two reserved addresses are the network address (all 0's in host part) and the broadcast address (all 1's in host part).

---

### Q5. The network address of a network is obtained by setting all bits in the host part to:
- (A) All 1's
- (B) All 0's
- (C) Alternating 0's and 1's
- (D) Same as the network part

**Answer: (B) All 0's**
Explanation: As per the transcript, the network address has all 0's in the host address part. It uniquely identifies the network and cannot be assigned to any host.

---

### Q6. If a Class A network has the network address 126.0.0.0, what is its broadcast address?
- (A) 126.0.0.255
- (B) 126.255.0.0
- (C) 126.255.255.255
- (D) 126.0.255.255

**Answer: (C) 126.255.255.255**
Explanation: The broadcast address is obtained by putting all 1's in the host part. Class A has 24 host bits, so all 24 bits become 1, giving 126.255.255.255. This is directly from the transcript example.

---

### Q7. You have 255 hosts in a network. Why can't you use a Class C address?
- (A) Class C does not support networking
- (B) Class C supports only 254 valid hosts, which is less than 255
- (C) Class C supports only 128 hosts
- (D) Class C is reserved for multicast

**Answer: (B) Class C supports only 254 valid hosts, which is less than 255**
Explanation: This is the exact example from the transcript. Class C has 8 host bits → 2^8 − 2 = 254. Since you need 255, Class C is not possible. Using Class B wastes a huge address space — this is the key problem with classful addressing.

---

### Q8. In CIDR notation, what does the "/12" mean in 191.180.83.235/12?
- (A) The first 12 bits are the host address
- (B) There are 12 hosts in the network
- (C) The first 12 bits are the network address
- (D) The subnet mask has 12 zeros

**Answer: (C) The first 12 bits are the network address**
Explanation: In CIDR slash notation, the number after the slash indicates how many bits form the network (subnet) address. So /12 means the first 12 bits are the network part and the remaining 20 bits are the host part.

---

### Q9. What is the main problem with classful addressing that CIDR solves?
- (A) Classful addressing is too slow
- (B) Classful addressing wastes IP address space due to fixed boundaries
- (C) Classful addressing does not support multicast
- (D) Classful addressing uses 64-bit addresses

**Answer: (B) Classful addressing wastes IP address space due to fixed boundaries**
Explanation: As explained in the transcript, classful addressing has rigid boundaries (8, 16, 24 bits). If you need slightly more than what one class supports, you must jump to the next class which wastes a huge number of addresses. CIDR removes this fixed boundary.

---

### Q10. In Windows, the subnet mask is called "subnet mask." What is the equivalent term used in Linux?
- (A) IP mask
- (B) Netmask
- (C) Network filter
- (D) Host mask

**Answer: (B) Netmask**
Explanation: The transcript explicitly states that the terms "subnet mask" (used in Windows) and "netmask" (used in Linux) are used interchangeably and mean the same thing.

---

## What Else to Expect After Lecture 27

The professor mentions that in the **next lecture (Lecture 28)**, he will cover:
- A **specific example of CIDR with subnetting and supernetting**
- Given an IP address pool, how to divide it into multiple subnets
- How to allocate IP addresses to different hosts inside those subnets

This lecture (27) covered the **foundations**: classful addressing, its limitations, and the introduction to CIDR. The next lecture will cover the **practical application** of CIDR.
