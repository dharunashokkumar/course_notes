# Lecture 35: Internet QoS – V (Integrated and Differentiated Service Architecture)

**Course:** Computer Networks and Internet Protocol  
**Instructor:** Prof. Sandip Chakraborty, IIT Kharagpur  
**Topic:** IntServ Architecture, DiffServ Architecture, RSVP Protocol

---

## Overview of This Lecture

This is the **last lecture on Internet Quality of Service (QoS)**. It covers two major QoS architectures used on the internet:

1. **Integrated Service Architecture (IntServ)** — provides **guaranteed** QoS
2. **Differentiated Service Architecture (DiffServ)** — provides **best-effort** QoS

The lecture also covers the **Resource Reservation Protocol (RSVP)** which is the key protocol used inside IntServ, and the **Bandwidth Broker** used inside DiffServ.

---

## Concept 1: Two Modes of QoS Service

📌 **Concept Name:** Guaranteed Service vs Best-Effort Service

🧠 **Simple Explanation:**

There are two ways to provide QoS on the internet:

**Mode 1 — Guaranteed Service (IntServ):**  
The network **promises** that it will meet your Service Level Agreement (SLA) 100% of the time. For example, if your SLA says "all my packets should have less than 10 milliseconds of delay," then the network **ensures** that every single packet meets this target. This is called **Integrated Service Architecture**.

**Mode 2 — Best-Effort Service (DiffServ):**  
The network **tries its best** to meet your QoS requirements, but there is **no guarantee**. If the network is too congested, your packets may suffer. But if the load is moderate, the network will try to give you good service. This is called **Differentiated Service Architecture**.

🛠 **Real-world Example (from transcript):**  
Think of an **airport security check**. If the airport is extremely crowded with millions of passengers at peak time, even security officials cannot do anything — everybody waits. But when the load is high but not extremely high, the security officials try to help — if one queue is growing longer but another queue has fewer people, they shift some passengers to the shorter queue. That is best-effort service — they try their best but cannot guarantee.

🎯 **Exam Important Points:**
- IntServ = Guaranteed QoS
- DiffServ = Best-effort QoS (no 100% guarantee)
- IntServ requires coordination among **all routers** in the path
- DiffServ is more suitable for **internet-scale** implementation because coordination among all routers is not needed
- DiffServ only requires coordination among **DiffServ domains** (not individual routers)

⚠️ **Common Confusions:**
- "Best-effort" in DiffServ does NOT mean "no service." It means the network will try to provide QoS but cannot guarantee it 100% of the time.
- IntServ is NOT practical for large-scale internet because of the coordination overhead among millions of routers.

---

## Concept 2: Integrated Service Architecture (IntServ) — Core Idea

📌 **Concept Name:** IntServ Architecture

🧠 **Simple Explanation:**

In IntServ, to provide guaranteed QoS, **every single router** in the end-to-end path must:
- Know about your SLA (Service Level Agreement)
- **Reserve resources** (like bandwidth, buffer space) for your specific flow
- **Coordinate** with all other routers in the path

Because a packet travels through **many routers** (controlled by different ISPs — tier 0, tier 1, tier 2, local ISPs), all these different service providers must coordinate with each other. This makes IntServ **very complex and difficult to implement at internet scale**.

🎯 **Exam Important Points:**
- IntServ requires **per-flow state** at every intermediate router
- Each router must **reserve resources** for individual flows
- Routers must **coordinate** with each other along the entire path
- The scalability problem is the main reason IntServ is impractical for the full internet

⚠️ **Common Confusions:**
- IntServ is not just about having good routers — the challenge is the **coordination** among millions of routers across different ISPs.

---

## Concept 3: Internet Service Architecture (ISA) — Steps Inside a Router

📌 **Concept Name:** ISA (Internet Service Architecture)

🧠 **Simple Explanation:**

ISA is the framework that describes how IntServ QoS works inside a router. It has these key steps:

**Step 1 — Admission Control:**  
Before allowing a new flow into the network, the router checks: "Do I have enough resources to support this new flow's QoS requirements?" If yes, the flow is admitted. If no, the flow is **dropped** (denied entry). This is done using the **Resource Reservation Protocol (RSVP)**.

**Step 2 — Routing Control:**  
The routing decision is made based on QoS parameters. If a particular router is overloaded, the packet is routed to an **alternate router** that is less loaded. So routing depends on QoS, not just shortest path.

**Step 3 — Queuing Strategies:**  
Different queuing mechanisms (priority queuing, weighted fair queuing, etc.) are used to handle different flow requirements.

**Step 4 — Congestion Avoidance:**  
Algorithms like **Random Early Detection (RED)** are used to prevent congestion before it happens.

🛠 **Real-world Example (from transcript):**  
When you make a phone call and hear a voice saying "all lines are busy, please dial after some time," this is the **admission control** at work — the network is telling you it does not have enough resources to admit your call.

🎯 **Exam Important Points:**
- ISA integrates routing and QoS together inside the router
- Four key components: Admission Control, Routing Control, Queuing Strategies, Congestion Avoidance
- RSVP feeds into admission control
- If resources cannot be reserved, admission control **denies** the flow

---

## Concept 4: ISA Architecture Inside a Router — Forwarding Plane

📌 **Concept Name:** ISA Router Internal Architecture

🧠 **Simple Explanation:**

Inside a router running ISA, there are two planes:

**Control Plane (Upper Part):**
- **Routing Protocols** → find out the routing paths
- **Routing Database** → stores routing information
- **Reservation Protocol (RSVP)** → reserves resources at each router
- **Admission Control** → decides whether to allow new flows
- **Management Agent** → manages traffic shaping, traffic policing
- **Traffic Control Database** → tells how packets need to be treated

**Forwarding Plane (Lower Part):**
When a packet arrives:
1. **Classifier + Route Selection** → classifies the packet into a traffic class AND selects the outgoing route by looking at the routing database
2. **Packet Scheduler** → gets input from the route information AND from the traffic control database, then puts the packet into the correct queue
3. **Queues** → there are multiple queues — one for best-effort traffic and others for QoS traffic
4. **Scheduler** → runs on the queues and forwards packets based on the chosen queuing policy

🎯 **Exam Important Points:**
- Classifier determines the QoS class of the packet
- Packet scheduler uses BOTH route info and traffic control info
- Multiple queues exist — best effort queue + QoS-specific queues
- The entire ISA integrates routing and QoS into one unified treatment

---

## Concept 5: Resource Reservation Protocol (RSVP)

📌 **Concept Name:** RSVP Protocol

🧠 **Simple Explanation:**

RSVP stands for **Resource Reservation Protocol**. It is a **network control protocol** that allows a data receiver to request a **special end-to-end QoS** for its data flow.

Key facts about RSVP:
- It is a **network control protocol**, NOT a routing protocol
- It works **in association with IP** (not as a replacement)
- It is designed to operate with current and future **unicast and multicast** routing protocols
- It reserves resources at **every router** in the end-to-end path

🎯 **Exam Important Points:**
- RSVP = Network Control Protocol (NOT a routing protocol) — this is a very important exam point
- RSVP works alongside routing protocols, not instead of them
- RSVP handles resource reservation; routing protocols handle path selection
- Supports both unicast and multicast

⚠️ **Common Confusions:**
- Students often think RSVP is a routing protocol. It is NOT. It is a QoS protocol that works **alongside** routing.

---

## Concept 6: ISA and RSVP Architecture Together

📌 **Concept Name:** How IntServ and RSVP Work Together (Host + Router)

🧠 **Simple Explanation:**

The IntServ+RSVP architecture runs on **two types of machines**: the **Host machine** (your computer) and the **Router machine**.

**Inside the Host Machine:**
- **Application** → generates traffic
- **Classifier** → classifies packets into QoS classes
- **RSVP Daemon** → communicates with RSVP daemons on routers to reserve resources
- **Packet Scheduler** → schedules packets into queues based on what resources were reserved

**Inside the Router Machine:**
- **Routing Protocol Daemon** → handles routing decisions
- **RSVP Daemon** → communicates with other RSVP daemons (on hosts and other routers)
- **Classifier** → classifies packets
- **Packet Scheduler** → decides the **next hop** (from routing) and the **class queue** (from RSVP/classifier), then forwards the packet

**Critical Point:** RSVP daemons run on the **host AND all intermediate routers**. They all talk to each other. The RSVP daemon at the host sends resource reservation requests, and every router in the path either accepts (reserves resources) or rejects (admission control denies the flow).

🎯 **Exam Important Points:**
- RSVP daemon runs at BOTH the host and ALL intermediate routers
- RSVP daemons communicate with each other across the entire path
- If any router cannot reserve resources → admission control denies the flow
- Packet scheduler uses BOTH next hop info (routing) and class queue info (RSVP)
- This coordination requirement across ALL routers is what makes IntServ difficult to scale

---

## Concept 7: RSVP Terminologies — Traffic Control, Classifier, Scheduler

📌 **Concept Name:** RSVP Key Terms

🧠 **Simple Explanation:**

In RSVP, QoS is implemented through **traffic control** which has two main components:

1. **Packet Classifier** → determines the QoS level of each packet (which class does it belong to?)
2. **Packet Scheduler** → a link-layer-dependent mechanism that determines which packets get forwarded and when

**Important detail about outgoing interfaces:**  
A router can have **multiple outgoing interfaces** (like eth0, eth1, eth2, eth3). For **each outgoing interface**, the router must maintain **separate queues**. The routing algorithm tells which outgoing interface the packet goes to, and then the queuing mechanism at that specific interface serves the packets.

🎯 **Exam Important Points:**
- Traffic control = Packet Classifier + Packet Scheduler
- Queues are **per outgoing interface** (not per router as a whole)
- Routing algorithm → decides which interface; Queuing mechanism → decides order of service at that interface
- Scheduler achieves the desired QoS for each outgoing interface

---

## Concept 8: RSVP Reservation Procedure — Admission Control + Policy Control

📌 **Concept Name:** RSVP Reservation Setup Process

🧠 **Simple Explanation:**

When you want to reserve resources using RSVP, the following happens:

**Step 1:** An RSVP QoS request is sent to the router.

**Step 2:** The request is checked by **two decision modules:**

- **Admission Control Module:** Checks — "Does this router have **enough available resources** to support this request?" If the router has 10 Mbps free bandwidth and you need 5 Mbps, it can admit you. If only 2 Mbps is free but you need 5 Mbps, it rejects you.

- **Policy Control Module:** Checks — "Does this user have **administrative permission** to make this reservation?" This comes from the SLA. If you haven't purchased a premium SLA, even if you try to send VoIP packets as high-priority, they will be treated as best-effort packets. You need a proper SLA to get QoS treatment.

**Step 3 — If BOTH checks pass:**  
The parameters are set in the packet classifier and the link-layer interface to provide the desired QoS.

**Step 3 — If EITHER check fails:**  
RSVP returns an **error notification** to the application, saying "you are not allowed to send this packet with this QoS."

🎯 **Exam Important Points:**
- Two checks: Admission Control (resource availability) + Policy Control (administrative permission/SLA)
- BOTH must pass for reservation to succeed
- If either fails → error notification is sent back
- Policy control is based on Service Level Agreement (SLA)
- Without a proper SLA, your packets are treated as best-effort even if you mark them as high priority

⚠️ **Common Confusions:**
- Admission control and policy control are SEPARATE checks. Admission control is about resources; policy control is about permission/SLA.

---

## Concept 9: RSVP Reservation Model — Flowspec and Filterspec

📌 **Concept Name:** Flow Descriptor = Flowspec + Filterspec

🧠 **Simple Explanation:**

An RSVP reservation request consists of two parts:

**1. Flowspec (Flow Specification):**
- Specifies the **desired level of QoS** (what quality do you expect?)
- Used to set parameters in the **Packet Scheduler**
- Contains two sub-parts:
  - **Rspec (Reservation Specification):** Defines the desired QoS parameters — like token rate, token bucket size (these are scheduler parameters)
  - **Tspec (Traffic Specification):** Defines the data flow characteristics — like peak bandwidth, latency, delay variation, maximum SDU size, minimum policed size (these are flow-specific parameters)

**2. Filterspec (Filter Specification):**
- Together with the session specification, defines the **set of data packets** to apply QoS to
- Used to set parameters in the **Packet Classifier**
- Tells the classifier what types of queuing to apply (priority queuing, custom queuing, weighted fair queuing, etc.)
- Essentially **filters out** the packets and puts them into appropriate queues

**Together, Flowspec + Filterspec = Flow Descriptor**

🎯 **Exam Important Points:**
- Flowspec → configures the **Packet Scheduler**
- Filterspec → configures the **Packet Classifier**
- Flowspec = Rspec (QoS parameters) + Tspec (traffic flow parameters)
- Rspec: token rate, token bucket size
- Tspec: peak bandwidth, latency, delay variation, max SDU size, min policed size
- Flow Descriptor = Flowspec + Filterspec (the pair)

⚠️ **Common Confusions:**
- Students mix up flowspec and filterspec. Remember: flowspec = scheduler (how to serve), filterspec = classifier (how to sort/filter packets).

---

## Concept 10: Problems with RSVP and IntServ

📌 **Concept Name:** Scalability Issues of RSVP/IntServ

🧠 **Simple Explanation:**

RSVP has **two major problems:**

1. The RSVP daemon needs to maintain **per-flow states** at every intermediate router. This means every router must remember information about every individual flow passing through it. This is a **heavy process**.

2. Because of this per-flow state and per-flow processing, RSVP has **scalability concerns** over large networks. The internet has millions of routers — asking every router to track every individual flow is not practical.

**Because of these problems, the networking community moved from IntServ towards DiffServ (Differentiated Service Architecture).**

🎯 **Exam Important Points:**
- RSVP requires **per-flow state** at every intermediate router — this is the root of the scalability problem
- Per-flow processing = heavy overhead
- This is the KEY reason why DiffServ was developed as an alternative
- IntServ works well in small, controlled networks but NOT at internet scale

---

## Concept 11: Differentiated Service Architecture (DiffServ) — Core Idea

📌 **Concept Name:** DiffServ Architecture

🧠 **Simple Explanation:**

DiffServ is a **coarse-grained, class-based** mechanism for traffic management. Instead of tracking every individual flow (like IntServ), DiffServ uses a small number of **fixed, pre-defined traffic classes**.

**How does it classify packets?**

DiffServ uses a **6-bit DSCP field** (Differentiated Services Code Point). This DSCP field is inside the **8-bit DS (Differentiated Services) field** in the **IP header** itself. The DSCP value tells the router which traffic class the packet belongs to.

**Key difference from IntServ:**

| Feature | IntServ | DiffServ |
|---------|---------|----------|
| Traffic classes | User-specific, flexible | Fixed, network-wide, pre-defined |
| Classification | Uses Filterspec (per user) | Uses DSCP field (same for all users) |
| Classifier behavior | User-specific | Fixed behavior |
| Need for Filterspec | Yes | No |
| Scalability | Poor (per-flow state) | Good (per-class state) |

**DiffServ-aware routers** implement something called **Per-Hop Behaviour (PHB)**, which defines how packets of each class are forwarded.

**DiffServ Domain:** A group of routers that implement a common, administratively defined DiffServ policy.

🎯 **Exam Important Points:**
- DiffServ = coarse-grained, class-based mechanism
- Uses 6-bit DSCP field inside the 8-bit DS field in the IP header
- Traffic classes are FIXED and NETWORK-WIDE (not user-specific)
- No need for Filterspec because classes are pre-determined
- DiffServ implements **Per-Hop Behaviour (PHB)**
- DiffServ Domain = group of routers with common DiffServ policies

⚠️ **Common Confusions:**
- DSCP is 6 bits, but it sits inside an 8-bit DS field in the IP header. Don't confuse the two sizes.
- DiffServ does NOT mean no QoS — it means QoS through fixed classes rather than per-flow reservation.

---

## Concept 12: DiffServ Architecture — Multiple Domains and Edge Routers

📌 **Concept Name:** DiffServ Domains and Traffic Conditioning

🧠 **Simple Explanation:**

In the DiffServ architecture, the internet is divided into multiple **DiffServ domains** (DS domains). Each domain can be a different ISP. For example:
- DS 1 = Local ISP
- DS 2 = Tier 1 ISP
- DS 3 = Another Local ISP

When a packet travels from source to destination, it passes through multiple DS domains. At the **boundary (edge) of each domain**, the router makes an **estimation:**
- "What is the end-to-end QoS requirement (from the SLA)?"
- "How much QoS has this packet already received?"

**Example from transcript:**  
If the SLA says end-to-end delay must be 30 milliseconds, and the packet has already experienced 20 ms of delay by the time it reaches DS2, then DS2 knows it must deliver the packet within the remaining 10 ms to meet the SLA.

Based on this estimation, the DiffServ domain decides how to treat the packet — whether to increase its priority or decrease it.

🎯 **Exam Important Points:**
- Packets pass through multiple DS domains
- Edge routers at domain boundaries do the traffic conditioning
- Each domain estimates: remaining QoS budget = total SLA requirement − QoS already received
- DiffServ requires coordination only between DS **domains** (not all individual routers)
- This is much more scalable than IntServ's router-to-router coordination

---

## Concept 13: Bandwidth Broker

📌 **Concept Name:** Bandwidth Broker in DiffServ

🧠 **Simple Explanation:**

A **Bandwidth Broker** is an agent that:
- Has knowledge of an organization's **priorities and policies**
- **Allocates QoS resources** according to those policies
- Defined in **RFC 2638**

In DiffServ, Bandwidth Brokers manage each DS domain. To achieve end-to-end resource allocation **across separate domains**, the Bandwidth Broker of one domain must **communicate with its adjacent peers** (the Bandwidth Brokers of neighboring domains).

For example: Bandwidth Broker at DS2 communicates with Bandwidth Broker at DS1 and Bandwidth Broker at DS3 to determine what QoS can be given to a flow.

This communication is based on **bilateral agreements** between adjacent domains. If a domain does NOT have a peering relationship (agreement) with a neighbor, it cannot ensure end-to-end QoS. This is why DiffServ is a **best-effort** service — it depends on whether the ISPs along the path have agreements with each other.

🎯 **Exam Important Points:**
- Bandwidth Broker = agent that allocates QoS resources based on organization policies
- Defined in RFC 2638
- Communicates with **adjacent peer** Bandwidth Brokers
- Uses **bilateral agreements** between neighboring domains
- If no agreement exists between two domains → end-to-end QoS cannot be ensured
- This is the reason DiffServ is best-effort, not guaranteed

⚠️ **Common Confusions:**
- Bandwidth Broker is NOT in IntServ — it is specific to DiffServ architecture.
- "Bilateral" means only between two adjacent domains, not all domains at once.

---

## Concept 14: Service Level Agreement (SLA) and Traffic Conditioning Agreement (TCA)

📌 **Concept Name:** SLA and TCA in DiffServ

🧠 **Simple Explanation:**

DiffServ has **two types of agreements:**

**1. Service Level Agreement (SLA):**
- A set of parameters and values that define the service offered to a traffic stream by a DS domain
- It specifies the **quality of service** a user will get
- Example: "Within Class 1, give extra priority to my VoIP traffic"

**2. Traffic Conditioning Agreement (TCA):**
- A set of parameters and values that specify **classifier rules and traffic profile**
- It defines which **fixed class** your traffic belongs to
- Think of it as choosing a service tier:
  - Class 1 → Pay more money, get better service
  - Class 2 → Pay less money, get decent service
  - Class 3 → Pay least, get basic service

**How they relate:**  
TCA tells which class you purchased. SLA tells what specific quality you get within that class. It is like buying a plane ticket: TCA = choosing Business Class vs Economy (the class), SLA = the specific services promised within that class.

🎯 **Exam Important Points:**
- SLA = defines the SERVICE offered to a traffic stream
- TCA = defines CLASSIFIER RULES and TRAFFIC PROFILE
- TCA determines which class → SLA determines quality within that class
- Both are required in DiffServ architecture
- Different from IntServ where the flowspec and filterspec serve similar purposes

---

## Concept 15: Boundary Nodes (Edge Nodes) in DiffServ

📌 **Concept Name:** Boundary/Edge Nodes

🧠 **Simple Explanation:**

In a DS domain, **boundary nodes** (also called **edge nodes** or **border nodes**) are the routers that connect one DS domain to another DS domain (or to a non-DiffServ domain).

These boundary nodes are responsible for:
1. **Classification** — mapping packets to a forwarding class
2. **Conditioning** — ensuring that the traffic from a customer **conforms to their SLA**

So the boundary node is the "gatekeeper" of each DiffServ domain. It checks incoming traffic and decides how to treat it.

🎯 **Exam Important Points:**
- Boundary/edge nodes interconnect DS domains
- They perform classification AND conditioning
- They ensure SLA compliance
- They are the most important nodes in DiffServ (the interior/core routers just follow DSCP)

---

## Concept 16: Traffic Conditioning — Meter, Marker, Shaper, Dropper

📌 **Concept Name:** Four Components of Traffic Conditioning

🧠 **Simple Explanation:**

Traffic conditioning is a set of control functions applied to classified packets to enforce the TCA. It has **four components:**

**1. Meter:**
- Measures the classified traffic stream against the traffic profile
- Estimates: "How much QoS has this packet already received?" and "How much QoS still needs to be given?"
- Example: If end-to-end delay = 30 ms, and packet already has 10 ms delay, the meter says "20 ms remaining"

**2. Marker:**
- Based on the meter's measurement, the marker **dynamically assigns priority** to the packet
- If the packet needs to be delivered within 20 ms but other packets in the queue only need 30 ms → increase this packet's priority
- If the packet needs 30 ms but other packets need only 5 ms → decrease this packet's priority
- The marker updates the DSCP or priority information

**3. Shaper:**
- Shapes the traffic flow — controls the rate at which packets are sent
- Ensures traffic conforms to the agreed-upon profile

**4. Dropper:**
- Drops packets that violate the traffic profile
- If traffic exceeds the agreed-upon rate, excess packets are dropped

**The flow:** Network Traffic → MF Classifier → Marker ← Meter (with TCA info) → Shaper/Dropper → To Network Interior

🎯 **Exam Important Points:**
- Four components: Meter, Marker, Shaper, Dropper
- Meter MEASURES traffic against the profile
- Marker DYNAMICALLY ASSIGNS priority based on meter's input
- Shaper controls the RATE of traffic
- Dropper REMOVES excess packets
- Priority is assigned dynamically by comparing the packet's remaining QoS budget with other packets in the queue

⚠️ **Common Confusions:**
- The marker does NOT just mark once — it dynamically adjusts priority based on current conditions. This is a key difference from static priority assignment.

---

## Concept 17: Per-Hop Behaviours (PHB) in DiffServ

📌 **Concept Name:** Four Types of Per-Hop Behaviour

🧠 **Simple Explanation:**

Per-Hop Behaviour (PHB) defines how packets are forwarded at each DiffServ-aware router. There are **four types:**

**1. Default PHB:**
- Provides **best-effort** service
- No special treatment — regular internet service

**2. Expedited Forwarding (EF) PHB:**
- Gives priority to **low-loss, low-latency** traffic (like VoIP)
- Can be implemented using **Priority Queuing**
- The most premium service

**3. Assured Forwarding (AF) PHB:**
- **Assures delivery** under prescribed conditions
- Example: You require a fixed amount of bandwidth for your application
- Can be implemented using **Custom Queuing**

**4. Class-Based Selector PHB:**
- Maintains **backward compatibility** with the IP Precedence field (the old way of marking priority)
- Can use **Weighted Fair Queuing** to ensure fairness

🎯 **Exam Important Points:**
- Four PHBs: Default, EF, AF, Class-Based Selector
- Default = best effort
- EF = low loss, low latency → uses **Priority Queuing**
- AF = assured delivery under conditions → uses **Custom Queuing**
- Class-Based Selector = backward compatible with IP Precedence → uses **Weighted Fair Queuing**
- Remember which queuing mechanism goes with which PHB — very exam relevant!

---

## Concept 18: Working Steps of a DS Domain

📌 **Concept Name:** Step-by-Step Working of DiffServ

🧠 **Simple Explanation:**

Here is the complete step-by-step flow of how DiffServ works:

**Step 1:** The source/user makes a **contract** with the ISP for a specific SLA.

**Step 2:** The source sends a **request message** to the **first-hop router**.

**Step 3:** The first-hop router sends the request to the **Bandwidth Broker**.

**Step 4:** The Bandwidth Broker checks whether the SLA can be met and sends back either **Accept** or **Reject**.

**Step 5:** If accepted, either the source OR the first-hop router **marks the DSCP field** in the IP header and starts sending packets.

**Step 6:** **Edge routers** at every DS domain boundary **check compliance with the SLA** and perform **policing**. Excess packets (those that exceed the agreed rate) are either **discarded** or **marked as low priority** to comply with the SLA.

**Step 7:** **Core routers** (interior routers) simply look at the **DSCP field** and apply the corresponding **Per-Hop Behaviour** — they do not do complex classification.

**Important:** Because excess packets are marked as low priority (not guaranteed delivery), DiffServ is NOT a guaranteed QoS — it is an **expected/best-effort QoS**.

🎯 **Exam Important Points:**
- Source contacts ISP → ISP involves Bandwidth Broker → Accept/Reject
- DSCP field is marked by source or first-hop router
- Edge routers check SLA compliance and do policing
- Excess packets → discarded or marked LOW PRIORITY (this is why DiffServ ≠ guaranteed QoS)
- Core routers ONLY look at DSCP and apply PHB — they do NOT perform complex QoS processing
- The heavy QoS work is done at EDGE routers, not core routers

---

## Concept 19: IntServ vs DiffServ — Complete Comparison

📌 **Concept Name:** Key Differences Summary

🧠 **Simple Explanation:**

| Feature | IntServ | DiffServ |
|---------|---------|----------|
| QoS Type | Guaranteed | Best-effort |
| Coordination | Among ALL routers | Among DS domains only |
| State Maintenance | Per-flow state at every router | Per-class state (fixed classes) |
| Classification | User-specific (Filterspec) | Fixed DSCP field |
| Protocol | RSVP | Bandwidth Broker |
| Scalability | Poor (not suitable for internet) | Good (suitable for internet) |
| Resource Reservation | Done at each router | Done at domain level |
| Queuing | Configured per user | Fixed PHBs |
| Classes | Flexible, per-user | Fixed, network-wide |
| Implementation Complexity | Very High | Moderate |

🎯 **Exam Important Points:**
- This comparison table is extremely exam-important
- The main advantage of DiffServ = scalability
- The main advantage of IntServ = guaranteed QoS
- For internet-scale: DiffServ is preferred
- For small controlled networks: IntServ can work

---

## 10 MCQs — Strictly from Lecture 35

### Q1.
**What type of QoS does the Integrated Service Architecture (IntServ) provide?**

A) Best-effort QoS  
B) Guaranteed QoS  
C) No QoS  
D) Random QoS  

**Answer: B) Guaranteed QoS**  
**Explanation:** The transcript clearly states that IntServ provides guaranteed service — the network ensures that the SLA requirements are met 100% of the time. Every router reserves resources to guarantee the promised QoS.

---

### Q2.
**RSVP is a ______ protocol.**

A) Routing protocol  
B) Transport layer protocol  
C) Network control protocol  
D) Application layer protocol  

**Answer: C) Network control protocol**  
**Explanation:** The transcript specifically says "RSVP is a network control protocol" and emphasizes that "it is not a routing protocol." It works in association with IP but handles resource reservation, not routing.

---

### Q3.
**What are the two decision modules involved in the RSVP reservation procedure?**

A) Flowspec and Filterspec  
B) Admission Control and Policy Control  
C) Marker and Meter  
D) Shaper and Dropper  

**Answer: B) Admission Control and Policy Control**  
**Explanation:** The transcript states that the RSVP QoS request is passed to two local decision modules — admission control (checks resource availability) and policy control (checks if user has administrative permission/SLA).

---

### Q4.
**In RSVP, the Flowspec is used to set parameters in the:**

A) Packet Classifier  
B) Routing Table  
C) Packet Scheduler  
D) Bandwidth Broker  

**Answer: C) Packet Scheduler**  
**Explanation:** The transcript states: "Flowspec is used to set the parameters in the packet scheduler." Filterspec is used for the packet classifier. Don't confuse the two.

---

### Q5.
**What is the main scalability problem with IntServ/RSVP?**

A) DSCP field is too small  
B) RSVP daemon needs to maintain per-flow states at intermediate routers  
C) Bandwidth broker communication is slow  
D) Queues are too long  

**Answer: B) RSVP daemon needs to maintain per-flow states at intermediate routers**  
**Explanation:** The transcript says: "RSVP daemon needs to maintain per flow states at intermediate routers" and this "per flow state and per flow processing raises the scalability concerns over a large network."

---

### Q6.
**In DiffServ architecture, the DSCP field is how many bits?**

A) 4 bits  
B) 8 bits  
C) 6 bits  
D) 2 bits  

**Answer: C) 6 bits**  
**Explanation:** The transcript states that DiffServ uses a "six bit differentiated service code point field or the DSCP field" which is included inside the "eight bit differentiated service field (DS field)" in the IP header.

---

### Q7.
**Which component in DiffServ traffic conditioning dynamically assigns priority to packets?**

A) Meter  
B) Marker  
C) Shaper  
D) Dropper  

**Answer: B) Marker**  
**Explanation:** The transcript clearly explains that the marker dynamically assigns priority by comparing the packet's remaining QoS budget with other packets in the queue. The meter only measures; the marker acts on that measurement.

---

### Q8.
**In DiffServ, Expedited Forwarding (EF) PHB can be implemented using:**

A) Custom Queuing  
B) Weighted Fair Queuing  
C) Priority Queuing  
D) FIFO Queuing  

**Answer: C) Priority Queuing**  
**Explanation:** The transcript says: "The expedited forwarding can be implemented inside the priority queuing" while "assured forwarding can be implemented with the help of custom queuing."

---

### Q9.
**In DiffServ architecture, what is a Bandwidth Broker?**

A) A router that forwards packets  
B) An agent that allocates QoS resources based on an organization's policies  
C) A protocol for reserving bandwidth  
D) A type of queue  

**Answer: B) An agent that allocates QoS resources based on an organization's policies**  
**Explanation:** The transcript defines Bandwidth Broker as "an agent that has some knowledge of an organization's priorities and policies, and allocates quality of service resources with respect to those policies" as per RFC 2638.

---

### Q10.
**In a DiffServ domain, what do core routers (interior routers) do when they receive a packet?**

A) Perform admission control and policy control  
B) Run the Bandwidth Broker  
C) Simply look at the DSCP field and decide the per-hop behaviour  
D) Maintain per-flow state for each packet  

**Answer: C) Simply look at the DSCP field and decide the per-hop behaviour**  
**Explanation:** The transcript states: "The core router it will just look at to the DSCP and decides the corresponding per hop behaviour." The heavy QoS processing (classification, policing, SLA compliance) is done by edge routers, not core routers.

---

## Quick Revision Summary

- **IntServ** = Guaranteed QoS, uses RSVP, per-flow state, coordination among ALL routers, NOT scalable
- **DiffServ** = Best-effort QoS, uses Bandwidth Broker, per-class state (DSCP), coordination among DS domains only, SCALABLE
- **RSVP** = Network Control Protocol (NOT routing), reserves resources end-to-end
- **RSVP Reservation** = Admission Control + Policy Control must both pass
- **Flow Descriptor** = Flowspec (scheduler) + Filterspec (classifier)
- **Flowspec** = Rspec (QoS params) + Tspec (traffic params)
- **DiffServ DSCP** = 6 bits inside 8-bit DS field in IP header
- **Traffic Conditioning** = Meter + Marker + Shaper + Dropper
- **PHBs** = Default (best-effort), EF (priority queuing), AF (custom queuing), Class-Based Selector (WFQ)
- **DiffServ Working** = Source → First-hop → Bandwidth Broker → Accept/Reject → Mark DSCP → Edge routers police → Core routers just read DSCP
- **SLA** = service parameters for a traffic stream; **TCA** = classifier rules and traffic profile
