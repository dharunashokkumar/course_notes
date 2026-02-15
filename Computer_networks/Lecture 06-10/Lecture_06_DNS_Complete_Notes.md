# Lecture 6: Application Layer - II (DNS)

**Course:** Computer Networks and Internet Protocol  
**Instructor:** Prof. Soumya Kanti Ghosh, IIT Kharagpur  

---

## Topics Covered in This Lecture

1. What is DNS and Why Do We Need It?
2. Top Level Domains (TLDs)
3. Domain Name Space
4. Domain Names and Labels
5. Domain Name Structure (Hierarchical Tree)
6. Fully Qualified Domain Names (FQDNs)
7. Generic TLDs (gTLDs)
8. Country Domains
9. Zones and Domains
10. Zone Transfer (Primary and Secondary DNS Servers)
11. DNS in the Internet (Generic, Country, and Inverse Domains)
12. Inverse Domain
13. Name Resolution Process (BIND, nslookup)
14. Recursive Resolution
15. Iterative Name Resolution
16. DNS Full Resolver vs. Stub Resolver
17. DNS Resource Records (RR)
18. DNS RR Message Format
19. DNS Messages (Query and Response)
20. Header Format and Flag Fields
21. Question Record Format and Query Name Format
22. Resource Record Format
23. Examples of Query and Response Messages (Forward and Inverse)

---

## Concept 1: What is DNS and Why Do We Need It?

📌 **Concept Name:** Domain Name System (DNS)

🧠 **Simple Explanation:**

DNS stands for **Domain Name System**. It is a **global database system** that is used for internet addressing, email, and other information.

Now, why do we need DNS? Think about it this way — whenever you want to send a data packet from one computer to another on the internet, you need the **IP address** of the destination. For example, if you want to open "www.iitkgp.ac.in", the actual communication requires its IP address like 203.16.x.x. But remembering IP addresses for every website is very tedious and difficult for humans.

So, DNS was created to give us **human-readable names** (like www.google.com) that can be **converted into IP addresses** behind the scenes. Names are much easier to use and memorize.

Here is the key point: **routers and network devices only understand IP addresses**. Names can only be understood at the **application level**. So someone needs to convert this name to IP — and that is exactly the **primary job of DNS**.

DNS also works with the concept of **domains and sub-domains**. For example, "iitkgp.ac.in" is a domain. Under it, "cse.iitkgp.ac.in" is a sub-domain. The management of these domains is **distributed** — no single server handles everything.

🛠 **Real-world Example (from transcript):**  
When you type "www.iitkgp.ac.in" in your browser, the browser (which is a client) asks the DNS to resolve this name. The DNS server finds the IP address and sends it back. Once the browser has the IP address, it communicates with that destination. All intermediate routers only understand this IP address.

🎯 **Exam Important Points:**
- DNS is a **global database** for internet addressing and mail.
- DNS **translates domain names to IP addresses** (name-to-IP conversion).
- Names are understood only at the **application layer**; routers work with IP.
- Domain management is **distributed**, not centralized.
- DNS servers perform the translation.

⚠️ **Common Confusions:**
- DNS does NOT carry data between computers. It only helps in **finding the IP address** of the destination.
- DNS works at the **Application Layer** of TCP/IP, not at the Network Layer.

---

## Concept 2: Application Layer Protocols (Quick Recap)

📌 **Concept Name:** Application Layer Examples

🧠 **Simple Explanation:**

The lecture begins by quickly recalling the TCP/IP Application Layer. At the application layer, there are several important protocols:

- **File Transfer:** TFTP, FTP, NFS
- **Email:** SMTP
- **Remote Login:** Telnet, rlogin
- **Network Management:** SNMP
- **Name Management:** DNS (used by routers also)

Today's lecture focuses on **DNS** — the name management protocol.

🎯 **Exam Important Points:**
- DNS is an **application layer** protocol.
- DNS falls under **Name Management** category in the TCP/IP application layer.
- SNMP is for network management (don't confuse with DNS).

---

## Concept 3: Top Level Domains (TLDs)

📌 **Concept Name:** Top Level Domains (TLDs)

🧠 **Simple Explanation:**

At the very top of the DNS hierarchy, we have **Top Level Domains** or **TLDs**. These are the rightmost part of any domain name. For example, in "www.google.com", the TLD is **.com**.

TLDs are of two main types as per this lecture:

**1. Generic TLDs (gTLDs):** These are typically **three characters or more** in length. Examples include:
- **com** — Commercial organizations
- **org** — Non-profit organizations
- **net** — Network service providers
- **gov** — U.S. government
- **mil** — U.S. military
- **edu** — Educational organizations

**2. Country Domains (ccTLDs):** These are **two-character** codes based on ISO 3166 standard. Examples:
- **in** — India
- **us** — United States
- **au** — Australia
- **uk** — British or Irish
- **fr** — France
- **de** — Germany
- **jp** — Japanese
- **ca** — Canadian

These TLDs are registered with and maintained by **ICANN** (Internet Corporation for Assigned Names and Numbers).

🎯 **Exam Important Points:**
- Generic TLDs = **3 or more characters** (com, edu, gov, org, net, mil, etc.)
- Country TLDs = **2 characters** based on **ISO 3166** country codes
- ICANN is the authority that registers and maintains TLDs
- Remember specific examples: com = Commercial, edu = Education, gov = US Govt, mil = Military, org = Non-profit

⚠️ **Common Confusions:**
- "in" is India (not "internet"), "us" is United States — these are country domains, not generic.
- Generic TLDs can be more than 3 characters too (e.g., "museum", "aero").

---

## Concept 4: Domain Name Space

📌 **Concept Name:** Domain Name Space

🧠 **Simple Explanation:**

The Domain Name Space is the overall **tree-like structure** that organizes all domain names on the internet.

At the very top is the **Root** (represented by a dot "."). Below the root, we have the TLDs like arpa, com, edu, org, ad, zw, etc. Below each TLD, there are further sub-levels.

For example, under "in" (India), there is "ac" (academic). Under "ac", there is "iitkgp" (IIT Kharagpur). Under "iitkgp", there can be "cse" (CSE department).

So the full path becomes: **cse.iitkgp.ac.in**

This entire tree of names is what we call the **Domain Name Space**. Each level adds more specificity.

🎯 **Exam Important Points:**
- Domain Name Space is a **hierarchical tree structure**.
- Root is at the top (represented by a dot).
- Below root → TLDs → second-level domains → sub-domains → and so on.
- Each node in the tree has a **label**.

---

## Concept 5: Domain Names and Labels

📌 **Concept Name:** Domain Names and Labels

🧠 **Simple Explanation:**

Every node in the domain name space tree has a **label**. When you read labels from bottom to top (from the specific host to the root), and separate them with dots, you get the **domain name**.

For example, consider the path: Root → edu → fhda → atc → challenger

Reading from bottom to top gives us: **challenger.atc.fhda.edu.**

Here:
- "challenger" is the label at the lowest level
- "atc" is the next label
- "fhda" is the next
- "edu" is the TLD
- The final dot represents the **root domain**

Each label corresponds to a level in the tree, and the full string of labels separated by dots gives us the complete domain name.

🎯 **Exam Important Points:**
- Each node has a **label**.
- Domain name = labels read from bottom to top, separated by dots.
- The trailing dot represents the **root**.

---

## Concept 6: Domain Name Structure (Hierarchical Tree)

📌 **Concept Name:** Domain Name Structure

🧠 **Simple Explanation:**

Domain names are arranged in a **hierarchical tree-like structure**. This is very important to understand.

Take the example of India's academic network:
- At the top is "in" (India)
- Below "in" we have "ac" (academic) and "nic" (National Informatics Centre)
- Below "ac" we have "iitkgp" (IIT Kharagpur), "iitb" (IIT Bombay), etc.
- Below "iitkgp" we have departments like "cse", "ece", "mech"
- Below "iitb" we have "cse", "math", etc.

So, the full domain name for CSE at IIT Kharagpur would be: **cse.iitkgp.ac.in**

Now here is an important point: there can be "cse" under iitkgp AND also "cse" under iitb. If you just look at "cse" alone, it may **clash** — it is **partially qualified**. But "cse.iitkgp.ac.in" and "cse.iitb.ac.in" are both **unique** and will never clash because they are **fully qualified**.

🎯 **Exam Important Points:**
- Domain names form a **hierarchical tree**.
- Same label (like "cse") can exist at multiple places in the tree — they don't clash because their **full path** is different.
- The hierarchy allows **easy management** — adding, deleting, updating domains independently.

---

## Concept 7: Fully Qualified Domain Names (FQDNs)

📌 **Concept Name:** Fully Qualified Domain Name (FQDN)

🧠 **Simple Explanation:**

If a domain name **ends in a dot**, it is considered **complete**. This is called a **Fully Qualified Domain Name (FQDN)** or an **absolute domain name**.

Example: **cse.iitkgp.ac.in.** (notice the dot at the end) — this is fully qualified.

If a domain name **does not end in a dot**, it is considered **incomplete** or **partially qualified**. For example, just "cse.iitkgp" is partially qualified.

When a domain name is incomplete, the **DNS resolver** may complete it by **appending a suffix** to make it fully qualified. The rules for how this suffix is added are **implementation-dependent and locally configurable**.

🛠 **Real-world Example (from transcript):**  
If you type just "cse.iitkgp" in your browser, the DNS resolver on your machine might automatically append ".ac.in." to make it "cse.iitkgp.ac.in." — making it fully qualified before sending the DNS query.

🎯 **Exam Important Points:**
- **FQDN = domain name ending with a dot** (the dot represents the root)
- FQDN is also called an **absolute domain name**
- If name does NOT end with a dot → it is **incomplete/partially qualified**
- DNS resolver can **append a suffix** to complete it
- Suffix rules are **implementation-dependent and locally configurable**

⚠️ **Common Confusions:**
- Students often forget that the **trailing dot** is what makes a name fully qualified.
- "cse.iitkgp.ac.in" (without dot) is technically partially qualified; "cse.iitkgp.ac.in." (with dot) is FQDN.

---

## Concept 8: Generic TLDs (gTLDs) — Details

📌 **Concept Name:** Generic TLDs (gTLDs) with Examples

🧠 **Simple Explanation:**

Generic TLDs are those top-level domain names that are **three or more characters** long. They are registered and maintained by **ICANN** (Internet Corporation for Assigned Names and Numbers).

Some examples from the lecture:

| Domain Name | Meaning |
|---|---|
| aero | Air transport industry |
| biz | Business use |
| cat | The Catalan culture |
| com | Commercial organizations |
| coop | Cooperatives |
| edu | Educational organizations |
| gov | U.S. governmental agencies |
| info | Informational sites |
| int | International organizations |
| jobs | Employment-related sites |
| mil | The U.S. military |
| mobi | Mobile devices sites |
| museum | Museums |

🎯 **Exam Important Points:**
- gTLDs have **3 or more characters**.
- Maintained by **ICANN**.
- Know the meaning of common ones: com, edu, gov, mil, org, net.

---

## Concept 9: Country Domains

📌 **Concept Name:** Country Domains (ccTLDs)

🧠 **Simple Explanation:**

Country domains are TLDs named for each of the **ISO 3166 international 2-character country codes**. They range from "ae" (United Arab Emirates) to "zw" (Zimbabwe). These are also called **geographical domains**.

Many countries have their own **second-level domains** underneath their country TLD, which work similar to the generic TLDs. For example, under India's ".in":
- "ac.in" — for academic institutions (like edu)
- "co.in" — for commercial organizations (like com)

🎯 **Exam Important Points:**
- Country domains = **2-character** codes from **ISO 3166**.
- Also called **geographical domains**.
- Countries can have second-level domains that **parallel generic TLDs** (e.g., ac.in parallels edu, co.in parallels com).

---

## Concept 10: Domains — The Concept

📌 **Concept Name:** Domains

🧠 **Simple Explanation:**

A **domain** is a subtree of the domain name space. Each domain has a **domain server (DNS server)** that keeps records about the things inside that domain. This is called a **resource record**.

The domain server has:
- **Authority** over its own domain
- Information about its **sub-domains** — which it can **delegate** to other servers

For example, the ".com" domain server knows about everything directly under .com. If there is something deeper (like mcgraw.com), it may delegate that to another DNS server.

🎯 **Exam Important Points:**
- A domain is a **subtree** of the domain name space.
- Every domain server has **resource records**.
- Domain servers can **delegate** sub-domains to other servers.

---

## Concept 11: Zones and Domains — The Difference

📌 **Concept Name:** Zones vs. Domains

🧠 **Simple Explanation:**

This is a very important distinction:

**Domain** = The entire set of names and machines that are contained under an organizational domain name. For example, the domain "iitkgp.ac.in" includes ALL names and machines under IIT Kharagpur.

**Zone** = A domain **minus** any sub-domains that have been **delegated** to other DNS servers.

Think of it this way: If the ac.in domain server has delegated "iitkgp.ac.in" to a separate DNS server, then the **zone** of ac.in is everything under ac.in **except** what belongs to iitkgp.ac.in (because iitkgp has its own server now).

A zone is what a **single DNS server is actually responsible for**.

**Authoritative vs Non-authoritative answers:**
- When a DNS server answers from its own zone data → **authoritative answer**
- When it answers using data obtained from another domain → **non-authoritative answer**

🎯 **Exam Important Points:**
- **Domain** = entire set of names under an organizational name
- **Zone** = domain **minus** sub-domains delegated to other DNS servers
- Zone = what a single DNS server is directly responsible for
- Authoritative answer = from own zone data
- Non-authoritative answer = obtained from another server

⚠️ **Common Confusions:**
- Domain ≠ Zone. Domain is bigger; Zone is the portion a single server handles.
- This is a very common exam question — know the difference clearly!

---

## Concept 12: Primary and Secondary DNS Servers (Zone Transfer)

📌 **Concept Name:** Primary Server, Secondary Server, and Zone Transfer

🧠 **Simple Explanation:**

Each domain name is typically served by **2 or more DNS servers** for **redundancy**. If one server fails, the other can still resolve names. These servers are called:

- **Primary Server:** Only **one** primary server per zone. It loads all information from the **disk file** (the master copy of zone data is stored here).
- **Secondary Server:** There can be **one or more** secondary servers. They load all information **from the primary server** (not from their own disk).

**Zone Transfer:** When the secondary server downloads/copies data from the primary server, this process is called **zone transfer**. This is how secondary servers stay synchronized with the primary.

🎯 **Exam Important Points:**
- Each domain served by **2 or more DNS servers** (for redundancy).
- **Only one primary** server per zone.
- **Multiple secondary** servers possible.
- Primary has the **master copy** of zone data (loaded from disk).
- Secondary gets data from primary through **zone transfer**.
- If primary fails, secondary can still resolve queries.

⚠️ **Common Confusions:**
- Zone transfer is from primary → secondary (not the other way around).
- The primary loads from disk; secondary loads from primary.

---

## Concept 13: DNS in the Internet — Three Categories

📌 **Concept Name:** DNS in the Internet (Generic, Country, and Inverse Domains)

🧠 **Simple Explanation:**

In the internet, the DNS tree at the top level (just below the root) has **three categories** of domains:

1. **Generic Domains** — TLDs with 3 or more characters (com, edu, gov, etc.). For example, the path "chal.atc.fhda.edu" is in the generic domain.

2. **Country Domains** — TLDs with 2 characters (in, us, uk, etc.). For example, the path "www.iitkgp.ac.in" is in the country domain.

3. **Inverse Domain** — This is a special domain used for **reverse DNS resolution** (IP address → domain name, the opposite of normal DNS).

🎯 **Exam Important Points:**
- Three categories below root: **Generic**, **Country**, and **Inverse** domains.
- Generic = name to IP (forward).
- Inverse = IP to name (reverse).

---

## Concept 14: Inverse Domain (Reverse DNS)

📌 **Concept Name:** Inverse Domain

🧠 **Simple Explanation:**

Normal DNS does **forward resolution** — you give a domain name and get back an IP address.

**Inverse Domain** does the **opposite** — you give an IP address and get back the domain name. This is called **reverse resolution** or **inverse resolution**.

How does it work? The inverse domain is organized under **arpa** → **in-addr** in the DNS tree. The IP address is written in **reverse order** and appended with ".in-addr.arpa."

For example, if the IP address is **132.34.45.121**, in the inverse domain it is represented as: **121.45.34.132.in-addr.arpa.**

(Notice: the IP octets are reversed!)

**Why is inverse DNS needed?** One use case from the transcript: An FTP server receives a connection from an IP address (say 153.2.7.9). The FTP server wants to **verify** whether this client is authorized. So it does a reverse DNS lookup to find out **which domain** this IP belongs to, and then checks if that domain is authorized.

🛠 **Real-world Example (from transcript):**  
An FTP server receives a packet from IP 153.2.79.9. It wants to verify the client. It does an inverse DNS query: 9.79.2.153.in-addr.arpa → and gets back the domain name. This helps in authentication.

🎯 **Exam Important Points:**
- Inverse domain = **IP to domain name** (reverse resolution).
- IP address is written in **reverse** under "in-addr.arpa".
- Example: IP 132.34.45.121 → 121.45.34.132.in-addr.arpa
- Used for **authentication/verification** purposes.
- Forward resolution (normal) = domain name → IP
- Inverse resolution = IP → domain name

⚠️ **Common Confusions:**
- The IP octets are **reversed** in the inverse domain representation — this is a very common trick question!
- "arpa" and "in-addr" are part of the inverse domain structure.

---

## Concept 15: Name Resolution Process (BIND, nslookup)

📌 **Concept Name:** Name Resolution Process

🧠 **Simple Explanation:**

The most commonly used DNS server software is **BIND** — which stands for **Berkeley Internet Name Domain**. It runs on UNIX/Linux systems as a process (daemon) called **named**.

When an application (like a web browser) needs to find the IP address for a domain name, it invokes the **DNS name resolver**. The DNS resolver then translates the fully qualified domain name into the corresponding IP address.

You can use the command **nslookup** to manually perform DNS resolution. For example:
- Type "nslookup www.iitkgp.ac.in" → it returns the IP address.
- Type "nslookup www.google.com" → it returns Google's IP address.

If the local name server does not have the information, it **asks its primary server**, and this continues up the hierarchy. For redundancy, each host may also have **one or more secondary name servers** which can be queried when the primary fails.

🎯 **Exam Important Points:**
- **BIND** = Berkeley Internet Name Domain (most commonly used DNS server).
- BIND runs as a process called **named** on UNIX/Linux.
- **nslookup** command = used to manually query DNS.
- If local server doesn't have info → asks its primary server → and so on.
- Secondary name servers provide **redundancy**.

---

## Concept 16: Recursive Resolution

📌 **Concept Name:** Recursive Resolution

🧠 **Simple Explanation:**

In **recursive resolution**, the client asks its local DNS server to resolve a name. If the local server doesn't know the answer, **it takes responsibility** to find it. It asks the next server in the hierarchy, which in turn asks the next, and so on — until the answer is found. The final answer is then passed back through the chain to the client.

The key point is: **the client only communicates with its local server** and only receives **one final response**.

For example (from the transcript diagram):
1. Client asks fhda.edu server: "What is the IP of mcgraw.com?"
2. fhda.edu doesn't know → asks edu server
3. edu server doesn't know → asks root server
4. Root server → directs to com server
5. com server → finds mcgraw.com → sends answer back
6. Answer flows back: com → root → edu → fhda.edu → client

Each server in the chain recursively asks the next one and waits for the answer.

🎯 **Exam Important Points:**
- In recursive resolution, **the server takes responsibility** to find the answer.
- Client sends **one query** and receives **one final answer**.
- The query travels up the hierarchy: local → edu → root → com → target, and the answer flows back.
- Only **one response** comes back to the client.

---

## Concept 17: Iterative Name Resolution

📌 **Concept Name:** Iterative Name Resolution

🧠 **Simple Explanation:**

In **iterative resolution**, the client itself does the work step by step.

The client sends a query to a DNS server. If that server doesn't have the answer, instead of finding the answer itself, it simply tells the client: **"I don't know, but try asking this other server."** It returns the address of the next DNS server to query.

The client then sends a new query to the next server. If that server also doesn't know, it gives the client yet another server address. This continues until the client finally gets the answer.

**Key difference from recursive:**
- **Recursive** = the server does all the work; client waits for one final answer.
- **Iterative** = the client does the work; each server only gives a referral if it doesn't know.

🎯 **Exam Important Points:**
- In iterative resolution, the **client** sends queries **sequentially** to different servers.
- If a server doesn't have the answer → it returns the **address of the next server** to query.
- Unlike recursive, where only **one response** comes back, in iterative the **client contacts multiple servers**.
- Client gets **referrals** (not answers) from each intermediate server.

⚠️ **Common Confusions:**
- In recursive, the SERVER does the chasing. In iterative, the CLIENT does the chasing. This is a very common exam question.

---

## Concept 18: DNS Full Resolver

📌 **Concept Name:** DNS Full Resolver

🧠 **Simple Explanation:**

A **full resolver** acts as a middle-man between the user program and the name server.

The process works like this:
1. The user program sends a **query** to the full resolver.
2. The full resolver sends the query to the **name server**.
3. The name server looks in its **database**. If it has the answer, it sends it back.
4. If the name server doesn't have the answer, it contacts a **foreign name server** (another server that may know).
5. The answer is cached and sent back to the user.

The full resolver maintains a **cache** — so if the same query comes again, it can answer much faster without going to the name server again. The name server itself also has its own **database and cache**.

🎯 **Exam Important Points:**
- Full resolver sits **between user program and name server**.
- Full resolver maintains its own **cache** for faster repeated lookups.
- Name server has its own **database** and **cache**.
- If name server doesn't know → it asks **foreign name server**.

---

## Concept 19: Domain Name Stub Resolver

📌 **Concept Name:** Stub Resolver

🧠 **Simple Explanation:**

A **stub resolver** is a simpler and more lightweight version of a resolver. It is a **routine linked with the user program** itself. It simply **forwards** the queries directly to a name server for processing.

Unlike the full resolver (which has its own cache and intelligence), the stub resolver is **embedded in the user program** and just passes queries directly.

On most platforms, the stub resolver is implemented by two library routines:
- **gethostbyname()** — given a name, find the IP
- **gethostbyaddr()** — given an IP, find the name

The stub resolver is **much faster**, more popular, and **mostly used** in practice because it is lightweight.

🎯 **Exam Important Points:**
- Stub resolver is a **routine linked with the user program**.
- It **forwards queries** directly to a name server.
- Implemented by: **gethostbyname()** and **gethostbyaddr()**.
- Stub resolver is **faster and more popular** than full resolver.
- No own cache (unlike full resolver) — directly hits the name server.

⚠️ **Common Confusions:**
- Full resolver has its own cache; stub resolver does NOT.
- Stub resolver is embedded in the program; full resolver is a separate entity.

---

## Concept 20: DNS Resource Records (RR)

📌 **Concept Name:** DNS Resource Records

🧠 **Simple Explanation:**

The DNS database is a **distributed database** made up of **Resource Records (RRs)**. These resource records are divided into classes for different kinds of networks.

Resource records provide a **mapping between domain names and network objects**. The most common network objects are IP addresses of internet hosts, but the domain name system can accommodate many other types of objects too (like HTTP servers, FTP servers, mail servers, etc.).

**Structure of a zone's resource records:**
- A zone consists of a **group of resource records**.
- It **begins with** a **Start of Authority (SOA)** record — this identifies the domain name of the zone.
- There is a **Name Server (NS) record** for the **primary name server** of that zone.
- There may also be NS records for **secondary name servers**.
- NS records tell us **which name servers are authoritative** for that zone.

**Fields of a Resource Record:**
1. **Name** — the domain name
2. **Type** — type of record
3. **Class** — the network class
4. **TTL** — Time to Live (how long the record is valid)
5. **RDlength** — length of the resource data
6. **RData** — the actual resource data (e.g., the IP address)

🎯 **Exam Important Points:**
- DNS database = composed of **Resource Records (RRs)**.
- RRs provide **mapping between domain names and network objects**.
- Zone starts with **SOA (Start of Authority)** record.
- **NS records** identify authoritative name servers.
- RR fields: **Name, Type, Class, TTL, RDlength, RData**.
- NS records determine if an answer is **authoritative or non-authoritative**.

---

## Concept 21: DNS RR Message Format

📌 **Concept Name:** DNS RR Message Format

🧠 **Simple Explanation:**

When DNS clients and servers communicate, they exchange messages in a standard format. The DNS message format has these sections:

**Header section contains:**
- **Identification** — a unique ID to match queries with responses
- **Parameters/Flags** — various flag bits
- **QDcount** — number of question records
- **ANcount** — number of answer records
- **NScount** — number of authority records
- **ARcount** — number of additional records

**Followed by four sections:**
1. **Question Section** — what is being asked
2. **Answer Section** — the answer to the question
3. **Authority Section** — information about authoritative servers
4. **Additional Information Section** — extra helpful information

🎯 **Exam Important Points:**
- DNS message format has: Identification, Parameters, QDcount, ANcount, NScount, ARcount.
- Four sections: **Question, Answer, Authority, Additional Information**.
- All DNS clients and servers follow this same format (it is the standard).

---

## Concept 22: DNS Messages — Query and Response

📌 **Concept Name:** Query and Response Messages

🧠 **Simple Explanation:**

DNS messages are of **two types:**
1. **Query** — a question sent by the client
2. **Response** — the answer sent back by the server

**Query message** has: Header + Question Section (only).

**Response message** has: Header + Question Section + Answer Section + Authoritative Section + Additional Section.

In the query message, the answer records, authoritative records, and additional records counts are all **0s** (since we're only asking a question, not providing answers).

🎯 **Exam Important Points:**
- DNS has two message types: **Query** and **Response**.
- Query = Header + Question Section.
- Response = Header + Question + Answer + Authoritative + Additional sections.
- In query: ANcount, NScount, ARcount = **all 0s**.

---

## Concept 23: Header Format and Flag Fields

📌 **Concept Name:** Header Format and Flag Fields

🧠 **Simple Explanation:**

The header format includes:
- **Identification** — to match query with response
- **Flags** — detailed control information
- **Number of question records** — how many questions
- **Number of answer records** — all 0s in query
- **Number of authoritative records** — all 0s in query
- **Number of additional records** — all 0s in query

The **Flag field** contains several sub-fields:

| Flag | Meaning |
|---|---|
| **QR** | Query (0) or Response (1) |
| **OpCode** | 0 = Standard, 1 = Inverse, 2 = Server Status |
| **AA** | Authoritative Answer (is the server authoritative for this domain?) |
| **TC** | Truncated (was the record too large and truncated?) |
| **RD** | Recursion Desired (does the client want recursive resolution?) |
| **RA** | Recursion Available (does the server support recursion?) |
| **rCode** | Status of the error (error code in the response) |

🎯 **Exam Important Points:**
- **QR** flag: 0 = Query, 1 = Response
- **OpCode**: 0 = Standard (name→IP), 1 = Inverse (IP→name), 2 = Server status
- **AA** = Authoritative Answer flag
- **TC** = Truncated flag
- **RD** = Recursion Desired
- **RA** = Recursion Available
- **rCode** = Error status code

⚠️ **Common Confusions:**
- AA is only meaningful in **response** messages.
- RD is set by the **client** in the query; RA is set by the **server** in the response.

---

## Concept 24: Question Record Format and Query Name Format

📌 **Concept Name:** Question Record Format and Query Name Format

🧠 **Simple Explanation:**

The **Question Record** has three fields:
1. **Query Name** — the domain name being asked about
2. **Query Type** — type of query (A record, NS record, etc.)
3. **Query Class** — network class

**Query Name Format:**
The domain name is encoded in a special way. Each label is preceded by a **count** of characters in that label, and the entire name ends with **0**.

Example: For the name **admin.atc.fhda.edu.** the encoding would be:
- 5 → a d m i n (5 characters)
- 3 → a t c (3 characters)
- 4 → f h d a (4 characters)
- 3 → e d u (3 characters)
- 0 → end of name

🎯 **Exam Important Points:**
- Question record: **Query Name + Query Type + Query Class**
- Query name is encoded with a **count** before each label
- Name ends with **0** (indicating end)
- Example: admin.atc.fhda.edu → [5]admin[3]atc[4]fhda[3]edu[0]

---

## Concept 25: Resource Record Format

📌 **Concept Name:** Resource Record Format

🧠 **Simple Explanation:**

The resource record format (used in answer, authority, and additional sections) has these fields:
1. **Domain name** — the domain this record is about
2. **Domain type** — type of the record
3. **Domain class** — the network class
4. **Time to Live (TTL)** — how long this record is valid (in seconds)
5. **Resource data length** — length of the data
6. **Resource data** — the actual data (e.g., the IP address)

🎯 **Exam Important Points:**
- Resource Record fields: **Domain name, Domain type, Domain class, TTL, Resource data length, Resource data**.
- TTL tells how long the record can be cached.

---

## Concept 26: Examples — Forward and Inverse DNS Messages

📌 **Concept Name:** DNS Query/Response Examples

🧠 **Simple Explanation:**

**Example 1 (Forward Resolution):**
A resolver wants to find the IP address of **"chal.fhda.edu"**. It sends a query message to the local DNS server.
- The query message contains the identification (0x1333), flags (0x0100 meaning standard query with recursion desired), question count = 1, and the encoded domain name.
- The response message comes back with identification (0x1333 — same as query), flags (0x8180), answer count = 1, and the actual IP address in the answer section.
- In the example, the resolved IP was **153.18.8.105**.

**Example 2 (Inverse Resolution):**
An FTP server receives a packet from IP **153.2.79.9** and wants to verify the client. It needs to find the domain name for this IP.
- The IP address is written in reverse in the inverse domain: **9.79.2.153.in-addr.arpa.**
- The query is sent with OpCode = 1 (inverse query), flags = 0x0900.
- The response comes back with the domain name corresponding to that IP.

🎯 **Exam Important Points:**
- In forward query: Identification matches between query and response.
- In inverse query: **IP is reversed** and appended with ".in-addr.arpa."
- Forward query uses **OpCode = 0** (standard).
- Inverse query uses **OpCode = 1** (inverse).
- Response contains the actual IP (forward) or domain name (inverse) in the answer section.

---

## Summary of Lecture 6

The key takeaways from this entire lecture:

1. DNS converts human-readable domain names to IP addresses (and vice versa for inverse DNS).
2. Domain Name Space is a hierarchical tree with Root → TLDs → sub-domains.
3. TLDs are of two types: Generic (3+ chars) and Country (2 chars, ISO 3166).
4. FQDN ends with a dot; partial names can be completed by the resolver.
5. Zone ≠ Domain. Zone = Domain minus delegated sub-domains.
6. Primary server holds master copy; secondary servers sync via zone transfer.
7. DNS has three categories: Generic domains, Country domains, and Inverse domains.
8. Two types of resolution: Recursive (server does the work) and Iterative (client does the work).
9. Full resolver has cache; Stub resolver is lightweight and directly queries name server.
10. Resource Records (RRs) map domain names to network objects; SOA and NS are key records.
11. DNS messages have Query and Response types with specific header/flag formats.
12. BIND is the most common DNS software; nslookup is the command to test DNS.

---

## 10 MCQs — Strictly From Lecture 6

### Q1. What is the primary function of DNS?

(A) To send emails across the internet  
(B) To translate domain names to IP addresses  
(C) To encrypt data during transmission  
(D) To manage network bandwidth  

**Answer: (B)**  
**Explanation:** As explained in the lecture, the primary job of DNS is to resolve/convert domain names (like www.iitkgp.ac.in) to IP addresses. This is called name-to-IP conversion.

---

### Q2. What is a Fully Qualified Domain Name (FQDN)?

(A) A domain name that has at least three labels  
(B) A domain name that ends with a dot, representing the root  
(C) A domain name that only uses generic TLDs  
(D) A domain name that includes the IP address  

**Answer: (B)**  
**Explanation:** The lecture states that if a domain name ends in a dot, it is assumed to be complete and is called a Fully Qualified Domain Name (FQDN) or absolute domain name. The trailing dot represents the root.

---

### Q3. What is the difference between a zone and a domain?

(A) They are the same thing  
(B) A zone is a domain minus any sub-domains delegated to other DNS servers  
(C) A domain is smaller than a zone  
(D) A zone includes all sub-domains including delegated ones  

**Answer: (B)**  
**Explanation:** As per the lecture, a domain represents the entire set of names under an organizational name, while a zone is a domain minus any sub-domains that have been delegated to other DNS servers. A zone is what a single DNS server is responsible for.

---

### Q4. Country domain TLDs are based on which standard?

(A) IEEE 802  
(B) ISO 3166 two-character country codes  
(C) RFC 2131  
(D) ITU-T X.25  

**Answer: (B)**  
**Explanation:** The lecture clearly states that country domains are top-level domains named for each of the ISO 3166 international 2-character country codes (like "in" for India, "us" for US).

---

### Q5. In inverse DNS, how is the IP address 132.34.45.121 represented?

(A) 132.34.45.121.arpa.in-addr  
(B) 121.45.34.132.in-addr.arpa  
(C) 132.34.45.121.in-addr.arpa  
(D) arpa.in-addr.121.45.34.132  

**Answer: (B)**  
**Explanation:** The lecture explains that in inverse domain, the IP address is written in reverse order and appended with ".in-addr.arpa". So 132.34.45.121 becomes 121.45.34.132.in-addr.arpa.

---

### Q6. What is zone transfer?

(A) Moving a domain from one country to another  
(B) The process where the secondary server gets data from the primary server  
(C) Transferring DNS queries from client to server  
(D) Converting domain names to IP addresses  

**Answer: (B)**  
**Explanation:** The lecture defines zone transfer as the process where secondary servers get copies of zone data from the primary server. The primary server has the master copy, and secondary servers synchronize through zone transfer.

---

### Q7. What does the BIND DNS software stand for?

(A) Binary Internet Name Database  
(B) Berkeley Internet Network Domain  
(C) Berkeley Internet Name Domain  
(D) Basic Internet Name Directory  

**Answer: (C)**  
**Explanation:** The lecture states that BIND stands for Berkeley Internet Name Domain. It runs under UNIX/Linux as a process called "named".

---

### Q8. In recursive resolution, who does the work of contacting multiple servers to find the answer?

(A) The client itself  
(B) The local DNS server (and servers in the hierarchy)  
(C) The browser  
(D) The root server only  

**Answer: (B)**  
**Explanation:** In recursive resolution, the local DNS server takes responsibility. It contacts the next server, which contacts the next, and so on. The client only sends one query and receives one final answer. In contrast, in iterative resolution, the client does the work.

---

### Q9. Which flag in the DNS header indicates whether the response is from an authoritative server?

(A) QR  
(B) TC  
(C) AA  
(D) RD  

**Answer: (C)**  
**Explanation:** The lecture explains that the AA (Authoritative Answer) flag indicates whether the responding server is authoritative for the domain in question. QR indicates query/response, TC means truncated, and RD means recursion desired.

---

### Q10. What are the two library routines used by a stub resolver?

(A) getIP() and getName()  
(B) resolve() and lookup()  
(C) gethostbyname() and gethostbyaddr()  
(D) dnsquery() and dnsreply()  

**Answer: (C)**  
**Explanation:** The lecture states that the stub resolver is implemented by two library routines: gethostbyname() (to get IP from name) and gethostbyaddr() (to get name from IP address). The stub resolver is linked directly with the user program.

---

*End of Lecture 6 Notes*
