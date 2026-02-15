# Lecture 28: IP Addressing (IPv4) II – CIDR (Subnetting & Supernetting Examples)

**Course:** Computer Networks and Internet Protocol  
**Instructor:** Prof. Sandip Chakraborty, IIT Kharagpur  
**Topic:** Practical examples of Subnetting, Supernetting, and Variable Length Subnet Masking in CIDR

---

## What This Lecture Covers

This lecture is a **practical continuation** of Lecture 27. In Lecture 27, you learned the theory of classful addressing, classless addressing (CIDR), subnetting, and supernetting. Now in Lecture 28, the professor walks through **detailed numerical examples** showing how to actually divide an IP address pool into subnets using CIDR. The lecture covers:

1. Quick recap of CIDR addressing format
2. Why we avoid all-0s and all-1s subnets
3. Subnetting example — dividing a network into 3 subnets
4. IIT Kharagpur practical example — using subnetting + supernetting together
5. Variable Length Subnet Mask (VLSM)
6. Preview of next lecture topics

---

## Concept 1: Quick Recap — CIDR Addressing Format

### 📌 Concept Name
CIDR (Classless Inter-Domain Routing) Address Format

### 🧠 Simple Explanation
In CIDR, every IP address is written with a **slash notation** like this:

**IP Address / Subnet Mask Length**

For example: `203.110.0.0/16`

This means:
- The **first 16 bits** are the **network part** (also called the network prefix).
- The **remaining 16 bits** (32 − 16 = 16) are the **host part**.

The subnet mask tells you exactly where the **boundary** is between the network address and the host address. Everything before the boundary identifies "which network" and everything after identifies "which host inside that network."

Think of it like a postal address: the network part is your city name, and the host part is your house number.

### 🎯 Exam Important Points
- The number after the slash (like /16, /19, /21) tells you how many bits are used for the network prefix.
- Host bits = 32 − (subnet mask length).
- The subnet mask determines the **subnet boundary**.

### ⚠️ Common Confusions
- Students confuse "subnet mask length" with the actual decimal subnet mask. For example, /16 means the subnet mask in decimal is 255.255.0.0, but the number 16 itself just means "16 bits are for network."
- The slash notation does NOT mean dividing anything — it just indicates how many bits are for the network prefix.

---

## Concept 2: Why We Need 3 Bits (Not 2 Bits) for 3 Subnets

### 📌 Concept Name
Subnet Bit Allocation — Avoiding All-0s and All-1s Subnets

### 🧠 Simple Explanation
Suppose you have a network `203.110.0.0/16` and you want to create **3 subnets** inside it.

Your first thought might be: "I need 2 bits because 2² = 4, which is more than 3." But that is **wrong** in traditional subnetting. Here's why:

With 2 bits, the possible subnet IDs are:
- `00` → This is the **all-zeros subnet** (problematic!)
- `01` → Valid
- `10` → Valid
- `11` → This is the **all-ones subnet** (problematic!)

So out of 4 combinations, you can only safely use 2 (which is `01` and `10`). That's not enough for 3 subnets!

Therefore, you need **3 bits**. With 3 bits you get 2³ = 8 combinations. Remove all-zeros (`000`) and all-ones (`111`), and you still have **6 usable combinations** — more than enough for 3 subnets.

### 🎯 Exam Important Points
- For N subnets, you need enough bits so that (2^bits − 2) ≥ N.
- We subtract 2 because we avoid the all-0s and all-1s subnet IDs.
- With 2 bits: usable subnets = 4 − 2 = 2 (not enough for 3 subnets).
- With 3 bits: usable subnets = 8 − 2 = 6 (enough for 3 subnets).

### ⚠️ Common Confusions
- Some students think 2 bits can handle 3 subnets because 2² = 4. But you must remove 2 reserved combinations (all-0s and all-1s), leaving only 2 usable.

---

## Concept 3: Problem with All-Zeros Subnet (Subnet Zero)

### 📌 Concept Name
All-Zeros Subnet Problem

### 🧠 Simple Explanation
Let's say your original network is `192.168.0.0/16` and you take 1 bit for subnetting (making it /17).

If you use `0` as the subnet identifier:
- The subnet address becomes: 192.168.**0**.0 (because the subnet bit is 0, so the third octet stays 0).

But wait — the original network address is ALSO `192.168.0.0`!

So the **subnet network address becomes identical to the original network address**. This creates confusion — the router cannot tell whether `192.168.0.0` refers to the whole network or just this particular subnet.

This is why it's called **Subnet Zero**, and we traditionally avoid using it.

### 🎯 Exam Important Points
- All-zeros subnet → subnet network address = original network address.
- This causes **ambiguity** in identifying the network vs. the subnet.
- "Subnet Zero" is the name for the all-zeros subnet.

### ⚠️ Common Confusions
- Students think the issue is about host addresses. No — the problem is that the **network address of the subnet** clashes with the **network address of the entire network**.

---

## Concept 4: Problem with All-Ones Subnet

### 📌 Concept Name
All-Ones Subnet Problem

### 🧠 Simple Explanation
Now consider the other extreme. Using the same example `192.168.0.0/16`, if you take 1 bit for subnetting and use `1` as the subnet identifier:

- The subnet becomes: `192.168.128.0/17` (because the bit is 1, making the third octet start at 128).

Now, what is the **broadcast address** of this subnet? To find the broadcast address, you set all host bits to 1:
- Broadcast of this subnet = `192.168.255.255`

But what is the broadcast address of the **original network** `192.168.0.0/16`?
- Broadcast of original = `192.168.255.255`

They are **the same**! So the router cannot distinguish whether a broadcast is meant for the whole network or just this subnet.

### 🎯 Exam Important Points
- All-ones subnet → subnet broadcast address = original network broadcast address.
- This causes ambiguity in broadcast operations.
- Combined with the all-zeros problem, this is why we remove both extremes.

### ⚠️ Common Confusions
- Students mix up the all-zeros problem with the all-ones problem. Remember:
  - All-zeros → **network address** clash
  - All-ones → **broadcast address** clash

---

## Concept 5: Subnetting Example — Creating 3 Subnets

### 📌 Concept Name
Subnetting a /16 Network into 3 Subnets

### 🧠 Simple Explanation
**Given:** Network IP = `203.110.0.0/16`  
**Task:** Create 3 subnets.

**Step 1:** Determine bits needed.
- We need 3 subnets. Using the formula: 2^bits − 2 ≥ 3.
- 2 bits give 2 usable (not enough). 3 bits give 6 usable (enough).
- So, take **3 bits** from the host part for subnetting.

**Step 2:** New subnet mask.
- Original mask = /16. We borrow 3 more bits → New mask = /19.
- Host bits remaining = 32 − 19 = **13 bits** per subnet.

**Step 3:** Assign subnet IDs (avoiding 000 and 111).
- Subnet 1: `100` → 203.110.**128**.0/19
- Subnet 2: `101` → 203.110.**160**.0/19
- Subnet 3: `110` → 203.110.**192**.0/19

**How to convert to decimal (example for Subnet 1):**
The third octet = subnet bits followed by remaining zeros.
`100` followed by `00000` = `10000000` in binary = **128** in decimal.

**For Subnet 2:**
`101` followed by `00000` = `10100000` = **160** in decimal.

**For Subnet 3:**
`110` followed by `00000` = `11000000` = **192** in decimal.

### 🎯 Exam Important Points
- When borrowing bits for subnetting, the new mask = original mask + borrowed bits.
- Each subnet gets (2^host_bits − 2) usable host addresses.
- Here each subnet has 2^13 − 2 = 8190 usable host addresses.
- To find the network address of a subnet, place the subnet bits in the correct position and set all host bits to 0.

### ⚠️ Common Confusions
- Students forget to convert the binary subnet bits to decimal for the dotted notation. Always expand to full 8 bits of the octet before converting.
- The /19 applies to ALL three subnets because this is **fixed-length subnetting** (same mask for every subnet).

---

## Concept 6: IIT Kharagpur Example — The Core Practical Problem

### 📌 Concept Name
Real-World Subnetting Problem (IIT Kharagpur Network)

### 🧠 Simple Explanation
This is the main detailed example of the lecture. Here's the scenario:

**Given:**
- IIT Kharagpur's Computer & Informatics Center (CIC) gets an IP pool: `203.110.0.0/19` from PNIC (the Indian IP allocation authority).
- /19 means: 19 bits for network, **13 bits for hosts**.
- Total possible hosts = 2^13 = 8192.

**Requirement:** Divide this into 3 department subnets:
- **CSE** (Computer Science): needs 2000 hosts
- **VGSOM** (Management School): needs 500 hosts
- **EE** (Electrical Engineering): needs 500 hosts

### 🎯 Exam Important Points
- Total host bits available = 32 − 19 = 13 bits.
- This is a **realistic IP allocation problem** — the kind that appears in exams.

---

## Concept 7: Estimating Bits Needed Per Department

### 📌 Concept Name
Host Bit Estimation for Subnets

### 🧠 Simple Explanation
Before subnetting, you need to calculate how many bits each department needs:

**CSE (2000 hosts):**
- 2^10 = 1024 → Not enough (1024 < 2000).
- 2^11 = 2048 → Enough! (2048 > 2000).
- CSE needs **11 bits** for host addresses.

**VGSOM (500 hosts):**
- 2^9 = 512 → Enough! (512 > 500, usable = 512 − 2 = 510).
- VGSOM needs **9 bits**.

**EE (500 hosts):**
- Same calculation as VGSOM.
- EE needs **9 bits**.

### 🎯 Exam Important Points
- Formula: Find the smallest n such that 2^n ≥ required hosts (+ 2 for network and broadcast addresses).
- The largest department determines the minimum host bits if using fixed-length subnetting.
- CSE needs 11 bits → only 13 − 11 = **2 bits left** for subnet IDs.
- With only 2 bits, you can't create 3 subnets (after removing all-0s and all-1s, only 2 usable).

### ⚠️ Common Confusions
- Students forget to consider that 2 addresses are always reserved (network address and broadcast address) in each subnet.
- The "2 bits problem" is the key insight that leads to the supernetting solution.

---

## Concept 8: Why Simple Subnetting Fails Here — Need for Supernetting

### 📌 Concept Name
The 2-Bit Limitation and Introduction of Supernetting

### 🧠 Simple Explanation
Here's the core problem:

- CSE needs 11 bits for hosts.
- Total available bits = 13.
- Bits left for subnetting = 13 − 11 = **2 bits**.
- With 2 bits, avoiding all-0s and all-1s, you can only make **2 subnets**.
- But you need **3 subnets** (CSE, VGSOM, EE).

**Solution: Use Supernetting!**

The idea is: **combine VGSOM and EE into a single "super-subnet"** first. Then:
- You only need to create **2 subnets** at the first level (CSE and VGSOM+EE combined).
- 2 subnets are perfectly possible with 2 bits!
- Later, you can further divide the VGSOM+EE super-subnet into 2 individual subnets.

This is the hierarchical approach — divide in layers, not all at once.

### 🎯 Exam Important Points
- When direct subnetting is not possible (not enough bits), **supernetting** (combining smaller networks) is the solution.
- Supernetting + hierarchical subnetting = the practical approach to IP allocation.
- An additional router is placed between the main network and the combined subnet to manage this hierarchy.

### ⚠️ Common Confusions
- Students think subnetting and supernetting are opposite and cannot be used together. In reality, they **work hand in hand** — you use supernetting to combine networks when bits are insufficient, then subnet within those combined networks.

---

## Concept 9: Step-by-Step Solution — First Level of Subnetting

### 📌 Concept Name
First-Level Division: CSE vs. VGSOM+EE

### 🧠 Simple Explanation
**Original network:** `203.110.0.0/19` (13 host bits available)

**Step 1: Combine VGSOM and EE.**
- VGSOM (500) + EE (500) = **1000 hosts** combined.
- Bits needed for 1000 hosts: 2^10 = 1024 → **10 bits** is enough.

**Step 2: Allocate bits.**
- CSE needs 11 bits for hosts.
- VGSOM+EE combined needs 10 bits.
- Maximum host bits needed = 11 bits.
- Subnet bits = 13 − 11 = **2 bits**.
- New subnet mask = 19 + 2 = **/21**.

**Step 3: Assign subnet IDs using the 2 bits.**
- CSE gets subnet ID `10`:
  - Address = `203.110.0.0` with bits 20-21 set to `10`.
  - Binary: ...`10`followed by 11 host bits of zeros.
  - Result: **203.110.16.0/21**

- VGSOM+EE gets subnet ID `01`:
  - Address with bits 20-21 set to `01`.
  - Result: **203.110.8.0/21**

**How 16 and 8 are calculated:**
Looking at the 3rd octet (bits 17-24 of the IP address):
- For CSE (`10`): The two subnet bits sit at positions within the 3rd octet. `10` in those positions followed by zeros gives binary `00010000` = **16** in decimal.
- For VGSOM+EE (`01`): `01` in those positions followed by zeros gives binary `00001000` = **8** in decimal.

### 🎯 Exam Important Points
- An **additional router** is placed between the main network and the VGSOM+EE combined subnet. This router handles the further subdivision.
- CSE: `203.110.16.0/21` (has 11 host bits = 2046 usable hosts).
- VGSOM+EE: `203.110.8.0/21` (has 11 host bits for now, will be further divided).

### ⚠️ Common Confusions
- Students get confused calculating the decimal value of the third octet. Remember: the subnet bits don't start at the beginning of the octet — they are positioned after the original network prefix bits.
- In /19, the first 19 bits are network. Bits 20 and 21 become the subnet bits. Bits 20-21 are inside the 3rd octet.

---

## Concept 10: Step-by-Step Solution — Second Level of Subnetting

### 📌 Concept Name
Second-Level Division: VGSOM vs. EE

### 🧠 Simple Explanation
Now we take the VGSOM+EE combined subnet `203.110.8.0/21` and divide it further.

**Available bits:** The /21 means 21 bits are for the network prefix. Host bits = 32 − 21 = **11 bits** remaining.

**Requirements:**
- VGSOM needs 9 bits for 500 hosts.
- EE needs 9 bits for 500 hosts.

**Subnet bits available:** 11 − 9 = **2 bits** for subnetting. This is enough to create 2 subnets (using `10` and `01`, avoiding all-0s and all-1s).

**Step: Assign subnet IDs using bits 22-23.**
- VGSOM gets subnet ID `10`:
  - New mask = 21 + 2 = **/23**
  - Address: **203.110.12.0/23**

- EE gets subnet ID `01`:
  - New mask = **/23**
  - Address: **203.110.10.0/23**

**Each department now has:** 9 host bits → 2^9 − 2 = **510 usable host addresses** (enough for 500 hosts each).

### 🎯 Exam Important Points
- The hierarchical division summary:
  - Level 0: `203.110.0.0/19` (entire IIT KGP network)
  - Level 1: CSE = `203.110.16.0/21`, VGSOM+EE = `203.110.8.0/21`
  - Level 2: VGSOM = `203.110.12.0/23`, EE = `203.110.10.0/23`
- This is a **two-level hierarchical subnetting** approach.
- The supernetting router sits between the main router and the VGSOM/EE subnets.

### ⚠️ Common Confusions
- Students forget that the second level of subnetting works on the VGSOM+EE pool, NOT the original pool. The starting point is /21, not /19.
- Each level of subnetting increases the subnet mask length.

---

## Concept 11: Fixed Length Subnet Mask

### 📌 Concept Name
Fixed Length Subnet Mask (FLSM)

### 🧠 Simple Explanation
In the IIT Kharagpur example above, notice something:

**At Level 1:** Both CSE and VGSOM+EE got the same mask → **/21**.  
**At Level 2:** Both VGSOM and EE got the same mask → **/23**.

This is called **Fixed Length Subnet Mask (FLSM)** — at each level, every subnet uses the **same subnet mask length**.

This is simple to implement but can be **wasteful**. For example, CSE needs 2000 hosts but gets 2^11 − 2 = 2046 addresses, while VGSOM+EE combined needs only 1000 hosts but also gets the same 2046 addresses. Many addresses go unused.

### 🎯 Exam Important Points
- FLSM = all subnets at the same level share the same subnet mask.
- Simple but potentially wastes IP addresses.
- This was the method used in the IIT KGP example.

---

## Concept 12: Variable Length Subnet Mask (VLSM)

### 📌 Concept Name
Variable Length Subnet Mask (VLSM)

### 🧠 Simple Explanation
CIDR allows a more flexible approach: **Variable Length Subnet Mask (VLSM)**.

In VLSM, **different subnets under the same network can have different subnet mask lengths**. One subnet might use /22, another might use /23. This lets you allocate IP addresses more efficiently based on actual need.

**Example from the transcript:**

**Given:** Network = `202.110.0.0/20` (12 host bits available, supports 2^12 − 2 = 4094 hosts).

**Requirement:**
- Subnet 1 (S1): 1000 hosts → needs 10 bits (2^10 = 1024)
- Subnet 2 (S2): 500 hosts → needs 9 bits (2^9 = 512)
- Subnet 3 (S3): 500 hosts → needs 9 bits (2^9 = 512)

**The problem with FLSM:** If you use fixed-length masks, you'd give all subnets 10 host bits (to accommodate S1's 1000 hosts). That leaves only 2 bits for subnet IDs → only 2 usable subnets → not enough for 3.

**VLSM solution:**
- For S1: Use **/22** as the mask (taking 2 bits for subnet). S1 gets 10 host bits, which supports 1000 hosts. Subnet ID = `10`.
- For S2: Use **/23** as the mask (taking 3 bits for subnet). S2 gets 9 host bits, which supports 500 hosts. Subnet ID = `101`.
- For S3: Use **/23** as the mask. S3 gets 9 host bits. Subnet ID = `110`.

So S1 has mask /22 while S2 and S3 have mask /23. The subnet masks are **different** — hence "variable length."

### 🎯 Exam Important Points
- VLSM = different subnets can have different subnet mask lengths.
- This is one of the **key advantages of CIDR** over classful addressing.
- VLSM allows more efficient use of IP address space.
- Solves problems that FLSM cannot (like the 3-subnet problem with limited bits).

### ⚠️ Common Confusions
- The subnet IDs must be chosen carefully so that one subnet's address doesn't become a valid host address of another subnet. For example, if S1 uses `10` as its 2-bit subnet ID, then S2 should NOT use `010` as its 3-bit ID, because `010` could be interpreted as a host in S1's space.
- This is the **prefix matching** concern: no subnet prefix should be a prefix of another subnet's address.

---

## Concept 13: Prefix Matching Caution in VLSM

### 📌 Concept Name
Prefix Matching Rule in Variable Length Subnetting

### 🧠 Simple Explanation
When using VLSM, you must be very careful about which subnet IDs you choose. The rule is:

**No subnet's prefix should be a prefix of another subnet's address space.**

In the example:
- S1 uses 2 bits: `10` with mask /22.
- S2 uses 3 bits: `101` with mask /23.

The professor specifically says: if instead of `101`, you chose `010` for S2, then `010` could be mistaken as a host address inside S1's range (because S1's prefix is just `10`, and `010` starts with `0` which is a different pattern — but the concern is about how the routing table interprets these).

The safe choices mentioned in the transcript:
- S1: `10` (2 bits) → /22
- S2: `101` (3 bits) → /23
- S3: `110` (3 bits) → /23

Here, `10` is NOT a prefix of `101` or `110` in the way that would cause confusion, because the different mask lengths make the boundaries clear.

### 🎯 Exam Important Points
- When using VLSM, subnet IDs must be chosen so they do not create ambiguity.
- This relates to how routers do **longest prefix matching** when forwarding packets.
- This is an advantage of CIDR — it supports this flexible allocation.

### ⚠️ Common Confusions
- Students confuse prefix matching in VLSM with the prefix matching used in routing tables. While related, in this context it specifically means: don't let one subnet's full address look like it belongs to another subnet.

---

## Concept 14: Address Hierarchy — The Core Principle

### 📌 Concept Name
IP Address Hierarchy in IPv4

### 🧠 Simple Explanation
The entire lecture demonstrates one fundamental principle: **IP addresses work in a hierarchy.**

- **Level 1:** The **network address** (network prefix) uniquely identifies a network.
- **Level 2:** The **host address** uniquely identifies a specific machine inside that network.

When you do subnetting, you add more levels:
- The network prefix → identifies the organization's network.
- The subnet bits → identify which subnet within the organization.
- The host bits → identify which machine within that subnet.

This hierarchy allows organizations to receive a single pool of IP addresses and then internally organize them into departments, floors, buildings, etc. — all without the outside world needing to know the internal structure.

### 🎯 Exam Important Points
- The address hierarchy is: **Network → Subnet → Host**.
- This is the fundamental concept behind all of CIDR subnetting and supernetting.
- External routers only see the network prefix; internal routers handle subnet routing.

---

## Concept 15: IP Allocation Authority — PNIC (India)

### 📌 Concept Name
Central IP Allocation Authority

### 🧠 Simple Explanation
The transcript mentions that in the IIT Kharagpur example, the IP address pool (`203.110.0.0/19`) is received from **PNIC** — which is the Indian IP allocation authority.

In the real world, IP addresses are not randomly assigned. There is a hierarchy:
- Global organizations allocate IP blocks to regional authorities.
- Regional authorities (like PNIC in India) allocate to organizations.
- Organizations then internally divide using subnetting.

The border router of the organization connects to the outside world and holds the IP pool information.

### 🎯 Exam Important Points
- IP addresses are allocated hierarchically by authorities.
- In India, PNIC is the central allocation body (as mentioned in the transcript).
- The organization's border router is the entry point where the IP pool is assigned.

---

## Concept 16: Preview of Next Lecture Topics

### 📌 Concept Name
What Comes Next (Lecture Preview)

### 🧠 Simple Explanation
The professor mentions that in the next class, they will cover:

1. **CIDR Routing Mechanism** — how routers use CIDR-based addresses to forward packets.
2. **Network Address Translation (NAT)** — a technique to effectively utilize IPv4 addresses by allowing multiple devices to share a single public IP.
3. **IPv6 Overview** — a brief look at IP version 6 and how it differs from IPv4.
4. **IPv6's "Biggest Failure"** — the professor teases that IPv6 adoption has been one of the biggest failures in computer networking, and will discuss the reasons.

### 🎯 Exam Important Points
- NAT, IPv6, and CIDR routing are upcoming exam topics.
- The mention of IPv6 as a "failure" is a notable discussion point the professor plans to cover.

---

## Summary Table: IIT Kharagpur Subnetting Solution

| Level | Entity | Network Address | Mask | Host Bits | Usable Hosts |
|-------|--------|----------------|------|-----------|-------------|
| 0 | IIT KGP (entire) | 203.110.0.0 | /19 | 13 | 8190 |
| 1 | CSE | 203.110.16.0 | /21 | 11 | 2046 |
| 1 | VGSOM + EE | 203.110.8.0 | /21 | 11 | 2046 |
| 2 | VGSOM | 203.110.12.0 | /23 | 9 | 510 |
| 2 | EE | 203.110.10.0 | /23 | 9 | 510 |

---

## Summary Table: FLSM vs. VLSM

| Feature | FLSM | VLSM |
|---------|------|------|
| Subnet mask | Same for all subnets at each level | Different subnets can have different masks |
| Efficiency | May waste addresses | More efficient address usage |
| Complexity | Simpler to configure | More complex but more flexible |
| Supported by | CIDR | CIDR |
| Example masks | All subnets use /21 | One subnet /22, others /23 |

---

## 10 MCQs — Strictly from Lecture 28 Transcript

### Q1.
**An organization has the IP pool `203.110.0.0/16` and wants to create 3 subnets. How many bits must be borrowed from the host part?**

A) 1 bit  
B) 2 bits  
C) 3 bits  
D) 4 bits  

**Answer: C) 3 bits**  
**Explanation:** With 2 bits, you get 4 combinations but must avoid all-0s and all-1s, leaving only 2 usable subnet IDs — not enough for 3 subnets. With 3 bits, 8 − 2 = 6 usable subnet IDs, which is sufficient.

---

### Q2.
**What is the problem with using all-zeros as a subnet ID?**

A) The broadcast address of the subnet clashes with the original network's broadcast  
B) The network address of the subnet becomes the same as the original network address  
C) It causes routing loops  
D) It wastes too many IP addresses  

**Answer: B)**  
**Explanation:** As stated in the transcript, when all subnet bits are 0, the subnet's network address becomes identical to the original network's network address, causing ambiguity.

---

### Q3.
**What is the problem with using all-ones as a subnet ID?**

A) The network address of the subnet clashes with the original network address  
B) The broadcast address of the subnet becomes the same as the original network's broadcast address  
C) Hosts cannot be assigned any addresses  
D) The subnet mask becomes too long  

**Answer: B)**  
**Explanation:** The transcript explains that when all subnet bits are 1, the broadcast address of the subnet equals the broadcast address of the original network.

---

### Q4.
**In the IIT Kharagpur example, the network `203.110.0.0/19` is given. How many host bits are available?**

A) 19  
B) 16  
C) 13  
D) 11  

**Answer: C) 13**  
**Explanation:** Host bits = 32 − 19 = 13 bits.

---

### Q5.
**CSE department needs 2000 hosts. How many bits are required for host addressing?**

A) 9 bits  
B) 10 bits  
C) 11 bits  
D) 12 bits  

**Answer: C) 11 bits**  
**Explanation:** 2^10 = 1024 (not enough for 2000). 2^11 = 2048 (enough for 2000). So 11 bits are needed.

---

### Q6.
**In the IIT KGP example, why was supernetting applied to VGSOM and EE?**

A) Because VGSOM and EE are in the same building  
B) Because with only 2 bits for subnet IDs, you cannot create 3 subnets  
C) Because they both need exactly 500 hosts  
D) Because the original mask was too long  

**Answer: B)**  
**Explanation:** CSE requires 11 host bits, leaving only 2 bits for subnetting from the 13 available. With 2 bits (after avoiding all-0s and all-1s), only 2 subnets are possible. Combining VGSOM and EE reduces the problem to 2 subnets.

---

### Q7.
**After the first level of subnetting in the IIT KGP example, what is the subnet mask for CSE?**

A) /19  
B) /20  
C) /21  
D) /23  

**Answer: C) /21**  
**Explanation:** The original mask is /19. Two bits are borrowed for subnetting at the first level: 19 + 2 = /21.

---

### Q8.
**What is the network address assigned to VGSOM after the second level of subnetting?**

A) 203.110.8.0/21  
B) 203.110.12.0/23  
C) 203.110.10.0/23  
D) 203.110.16.0/21  

**Answer: B) 203.110.12.0/23**  
**Explanation:** The transcript states that at the second level, VGSOM gets network address 203.110.12.0/23 with subnet ID `10`.

---

### Q9.
**What is Variable Length Subnet Mask (VLSM)?**

A) All subnets must use the same mask length  
B) Different subnets under the same network can have different subnet mask lengths  
C) The subnet mask can change dynamically during routing  
D) Only classful addressing supports variable masks  

**Answer: B)**  
**Explanation:** The transcript defines VLSM as the ability to use different mask lengths for different subnets. For example, one subnet uses /22 while another uses /23, which is supported by CIDR.

---

### Q10.
**In the VLSM example with network `202.110.0.0/20`, subnet S1 needs 1000 hosts and uses mask /22. What mask do subnets S2 and S3 (each needing 500 hosts) use?**

A) /22  
B) /23  
C) /24  
D) /20  

**Answer: B) /23**  
**Explanation:** S2 and S3 need 9 host bits each (2^9 = 512 ≥ 500). With /23, host bits = 32 − 23 = 9 bits. The transcript confirms that S1 uses /22 while S2 and S3 use /23, demonstrating variable length masks.

---

*End of Lecture 28 Study Guide*
