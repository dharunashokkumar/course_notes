# 📚 Lecture 7 — Application Layer – III (Client Server, FTP)

### Course: Computer Networks and Internet Protocol
### Prof. Soumya Kanti Ghosh | IIT Kharagpur

---

## 📖 Overview of This Lecture

This lecture covers **two main areas**:

1. **Client-Server Model** — The fundamental way applications communicate over a network.
2. **FTP (File Transfer Protocol)** — A widely used application layer protocol for transferring files.
3. **TFTP (Trivial File Transfer Protocol)** — A lightweight version of FTP.
4. **Sockets** — The mechanism that enables communication between processes.
5. **FTP Commands** — Access, file management, data formatting, and file transfer commands.

Let us go concept by concept.

---

---

## 🔷 CONCEPT 1: Client-Server Paradigm (The Big Picture)

---

### 📌 Concept Name

**Client-Server Model**

### 🧠 Simple Explanation

The client-server model is the **most common way** applications talk to each other over the internet.

The idea is very simple:

- There is a **server program** — it offers a service (like giving you a web page or a file).
- There is a **client program** — it requests that service (like your browser asking for a web page).

For every application, there is a pair: **a server and a client**.

**Examples from the transcript:**

- If you do **FTP** → there is an FTP server and an FTP client.
- If you do **Telnet** → there is a Telnet server and a Telnet client.
- If you browse the web → there is an HTTP server and your browser acts as the HTTP client.

The server and client **can be on the same machine** or on **different machines**. If they are on different machines, the client must know where the server is (its address) and must establish a connection before communication begins.

Underneath all of this, the network uses **TCP/IP** or similar models to carry the data.

### 🛠 Real-world Example (from transcript)

Think of an HTTP server. When you type `http://www.iitkgp.ac.in` in your browser:

- Your **browser** is the HTTP **client**.
- The IIT Kharagpur **web server** (somewhere on the internet) is the HTTP **server**.
- The server was **already waiting** for your request.
- Once it receives the request, it sends back the web page.

The HTTP server is often called **`httpd`** (HTTP daemon) in Linux.

### 🎯 Exam Important Points

- Server = process that **offers** a service.
- Client = process that **requests** a service.
- Server **always waits** for client requests (server is always active).
- Client and server can be on the **same or different machines**.
- The handling mechanism remains the **same** whether on the same or different machine.
- The predominant application layer communication model is the **client-server model**.

### ⚠️ Common Confusions

| Confusion | Clarification |
|---|---|
| "Server is a machine" | No. Server is a **process** (a program running). It can run on any machine. |
| "Client must be on a different machine" | No. Client and server **can run on the same machine**. |
| "Server sends data first" | No. Server **waits** for the client to send a request first. |

---

---

## 🔷 CONCEPT 2: Server Lifecycle — How a Server Works

---

### 📌 Concept Name

**Typical Client-Server Scenario (Server Lifecycle)**

### 🧠 Simple Explanation

Here is the step-by-step flow of how a server operates:

1. **Server process starts** on some computer.
2. It **initializes itself** (sets up everything it needs).
3. It **goes to sleep**, waiting for a client request.
4. A **client process starts** (on the same system or a different system).
5. The client **sends a request** to the server.
6. The server **wakes up**, processes the request, and provides the service.
7. After finishing, the server **goes back to sleep**, waiting for the next client.
8. The **process repeats**.

This is the basic "vanilla" operation of any client-server system.

### 🎯 Exam Important Points

- Server **initializes first**, then **sleeps** waiting for requests.
- Client initiates the communication by **sending a request**.
- After serving, server **goes back to sleep**.
- The roles are **asymmetric** — client and server do different things.

### ⚠️ Common Confusions

| Confusion | Clarification |
|---|---|
| "Server and client have symmetric roles" | No. Their roles are **asymmetric**. The server waits; the client initiates. |

---

---

## 🔷 CONCEPT 3: Iterative Server vs. Concurrent Server

---

### 📌 Concept Name

**Two Types of Servers — Iterative and Concurrent**

### 🧠 Simple Explanation

When there are **multiple clients** wanting service, the server has two ways to handle them:

#### ✅ Iterative Server

- Handles **one client at a time**, then moves to the next.
- Only a **single copy** of the server runs.
- The server knows **in advance** roughly how long each request will take.
- Other clients **must wait** if the server is busy.
- Used when resources must be allocated **one by one** (e.g., reserving a shared resource).

Think of it like: **One cashier at a shop**. One customer is served, then the next.

#### ✅ Concurrent Server

- Handles **multiple clients at the same time**.
- When a new request comes, the server **creates a copy of itself** (forks a child process) to handle that client.
- The original server goes back to **listening** for new requests.
- As many copies of the server as there are client requests (limited by resources).
- Used when the amount of work to handle a request is **unknown**.

Think of it like: **Multiple cashiers opening at a shop** — each customer gets their own cashier.

**Example from transcript:** HTTP servers are typically **concurrent** — they serve many users at the same time.

### 🎯 Exam Important Points

| Feature | Iterative Server | Concurrent Server |
|---|---|---|
| Clients served | One at a time | Multiple at the same time |
| Server copies | Single copy runs | Multiple copies (forked child processes) |
| Client waiting | Yes, must wait | No, each gets a dedicated copy |
| When used | Work time is known in advance | Work time is unknown |
| Example | Resource reservation | HTTP servers |

### ⚠️ Common Confusions

| Confusion | Clarification |
|---|---|
| "Concurrent means faster" | Not necessarily. It means **parallel handling**. Iterative may be needed when resources can't be shared. |
| "Iterative server can serve only one client ever" | No. It serves many clients, but **one after another**. |

---

---

## 🔷 CONCEPT 4: Five Components of a Connection (The 5-Tuple)

---

### 📌 Concept Name

**Five Components Needed to Establish a Connection**

### 🧠 Simple Explanation

Before any communication can happen between a client and a server, a **connection must be established**. This connection is defined by **5 things** (called the **5-tuple**):

1. **Protocol used** — e.g., TCP or UDP (usually IP at the network layer)
2. **Source IP address** — IP of the client machine
3. **Source port number** — port of the client process
4. **Destination IP address** — IP of the server machine
5. **Destination port number** — port of the server process

**Why do we need all 5?**

- **IP address** identifies a **machine** on the network.
- **Port number** identifies a **specific process** on that machine.
- **Protocol** identifies **how** the data will be transported.

Together, IP + Port uniquely identifies a process.

**What if client and server are on the same machine?**

- The IP will be the same for both.
- But the **port numbers will be different**.
- The protocol will also be the same.
- So, the **port number** is what distinguishes the two connections.

**Example from transcript:** If you open multiple browser tabs — one for IIT Kharagpur website, one for IIT Delhi, one for IIT Madras — each tab uses a **different port number** on your machine. The 5-tuple uniquely identifies each connection.

### 🎯 Exam Important Points

- A connection is defined by a **5-tuple**: Protocol, Source IP, Source Port, Destination IP, Destination Port.
- IP identifies a **machine**; Port identifies a **process**.
- Even on the same machine, different **port numbers** distinguish different connections.
- This 5-tuple **defines and distinguishes every connection**.

### ⚠️ Common Confusions

| Confusion | Clarification |
|---|---|
| "IP alone is enough to identify a connection" | No. You need **IP + Port** to identify a process. |
| "Same machine means same connection" | No. Different ports mean **different connections** even on the same machine. |

---

---

## 🔷 CONCEPT 5: Developing a Network Application (Protocol Stack)

---

### 📌 Concept Name

**Layers Used to Develop a Network Application**

### 🧠 Simple Explanation

To build a network application, you use standard protocols at each layer:

| Layer | Standard Protocol |
|---|---|
| Data Link Layer | Ethernet |
| Network Layer | IP (Internet Protocol) |
| Transport Layer | TCP (or UDP) |
| Application Layer | Standard API like **Berkeley Socket Interface** |

The key idea: you build your application on top of these well-accepted, standard protocols. You don't need to reinvent the wheel at each layer.

### 🎯 Exam Important Points

- The recommended approach is to use **standard, well-accepted protocols** at each layer.
- **Berkeley Socket Interface** is the standard API at the application layer for network programming.

---

---

## 🔷 CONCEPT 6: What is a Socket?

---

### 📌 Concept Name

**Socket — The Mechanism for Inter-Process Communication (IPC)**

### 🧠 Simple Explanation

A **socket** is a method (mechanism) that allows **two processes to talk to each other**. These processes can be on the **same machine** or on **different machines**.

**Simple Analogy (from transcript):** A socket is like a **telephone**. Just as a telephone allows one person to speak to another, a socket allows one process to communicate with another.

**How it works:**

1. The **server** opens a **half-socket** — it specifies its own IP, its own port, and the protocol. Then it **waits** on that port for a client.
2. The **client** opens another **half-socket** — it specifies its own IP, its own port, and the protocol. The client **knows the server's IP** (either directly or via DNS).
3. The client sends a **connection request** to the server.
4. If everything matches (protocol, format, etc.), the **5-tuple is established** and a full socket connection is created.
5. Using this **socket ID**, all further communication (data transfer, etc.) happens.

**Comparison with file handling in C:** Just like in C programming, when you open a file you get a **file ID** to read/write — here when you establish a socket, you get a **socket ID** to send/receive data.

### 🎯 Exam Important Points

- Socket = mechanism for **Inter-Process Communication (IPC)**.
- Allows one process to speak to another on **same or different machines**.
- Analogy: like a **telephone** connecting two people.
- Server creates a **half-socket** (its IP, port, protocol) and waits.
- Client creates another **half-socket** and sends a connection request.
- Once connected, a **socket ID** is used for all further data transfer.

### ⚠️ Common Confusions

| Confusion | Clarification |
|---|---|
| "Socket is a physical device" | No. Socket is a **software mechanism** (like a programming construct). |
| "Socket only works on different machines" | No. It works on **same or different machines**. |

---

---

## 🔷 CONCEPT 7: Association and Half-Association

---

### 📌 Concept Name

**Association (5-tuple) and Socket / Half-Association (3-tuple)**

### 🧠 Simple Explanation

When two processes on two machines communicate, we define two things:

**Association** = the full connection. It is a **5-tuple**:

1. Protocol
2. Local IP address
3. Local port number
4. Remote IP address
5. Remote port number

**Socket (Half-Association)** = one side of the connection. It is a **3-tuple**:

- Protocol, Local IP, Local Port → this is the client's half-socket
- Protocol, Remote IP, Remote Port → this is the server's half-socket

When both halves come together, the full association (connection) is formed.

### 🎯 Exam Important Points

- **Association** = 5-tuple (full connection).
- **Socket / Half-association** = 3-tuple (one side of the connection).
- Two half-associations combine to form one full association.

---

---

## 🔷 CONCEPT 8: FTP — File Transfer Protocol

---

### 📌 Concept Name

**FTP (File Transfer Protocol)**

### 🧠 Simple Explanation

FTP is one of the most **widely used application layer protocols**. Its primary job is to **transfer files over a network**.

Key facts about FTP:

- It is a **client-server model** protocol.
- It uses **TCP** at the transport layer (so it provides **reliable, connection-oriented** service).
- It also works with the **Telnet protocol**.
- FTP is formally defined in **RFC 959**.

### 🎯 Exam Important Points

- FTP = **File Transfer Protocol**.
- Uses **TCP** (reliable, connection-oriented).
- Defined in **RFC 959**.
- Client-server based.

---

---

## 🔷 CONCEPT 9: FTP — Two Types of Connections (Control & Data)

---

### 📌 Concept Name

**FTP Uses Two Connections — Control Connection and Data Connection**

### 🧠 Simple Explanation

This is one of the **most important** things about FTP. Unlike many protocols that use just one connection, FTP uses **TWO separate connections**:

### Connection 1: Control Connection (Port 21)

- The **FTP client** initiates this connection to the server on **well-known port 21**.
- The client's port is typically an **ephemeral port** (a random high-numbered port).
- This connection is used for:
  - **Sending commands** (login, navigate directories, list files, terminate session)
  - **Receiving responses** from the server
- The FTP server **listens on port 21** for incoming connections.

### Connection 2: Data Connection (Port 20)

- This is a **separate connection** used only for **actual file transfer** (upload/download).
- Typically established on **server port 20**.
- The data connection is **only opened when needed** — when the client issues a command that requires data transfer (like retrieving a file or listing files).
- It is possible for both client and server to use **ephemeral ports** for the data connection.
- The data connection is **unilateral** — data flows only in **one direction** at a time (either client → server OR server → client, but NOT both simultaneously).
- **After data transfer completes**, the data connection is **closed**.
- **Only one data transfer** can happen per data connection. For multiple transfers, separate data connections are opened.

### Active vs. Passive Data Connection

- **Active mode**: Data connection is initiated by the **server** → called "active".
- **Passive mode**: Data connection is initiated by the **client** → called "passive".

### 🎯 Exam Important Points

| Feature | Control Connection | Data Connection |
|---|---|---|
| Port (default) | **21** | **20** |
| Purpose | Commands and responses | File transfer (upload/download) |
| When opened | At the start of FTP session | Only when data transfer is needed |
| When closed | At end of session | After each data transfer completes |
| Direction | Both ways (commands & responses) | **Unilateral** (one direction at a time) |

- FTP can have a control connection **without** ever opening a data connection (if no files are transferred).
- Active mode = server initiates data connection.
- Passive mode = client initiates data connection.
- Only **one data transfer per data connection**.

### ⚠️ Common Confusions

| Confusion | Clarification |
|---|---|
| "FTP uses only one connection" | No. FTP uses **two** — control (port 21) and data (port 20). |
| "Port 20 is always used for data" | Not always. Ephemeral ports may be used, especially in passive mode. |
| "Data can flow both ways simultaneously" | No. Data connection is **unilateral** (one direction at a time). |
| "Control connection closes after each command" | No. Control connection stays open throughout the session. |

---

---

## 🔷 CONCEPT 10: FTP Basic Working — Processes (DTP and PI)

---

### 📌 Concept Name

**FTP Processes — Data Transfer Process (DTP) and Protocol Interpreter (PI)**

### 🧠 Simple Explanation

FTP has **two main processes** running at both client and server ends:

### 1. Protocol Interpreter (PI)

- Manages the **control connection**.
- **Translates** user commands into RFC-standard FTP commands.
- Sends these commands to the server.
- The **server's PI** receives commands and initiates appropriate actions.

### 2. Data Transfer Process (DTP)

- Manages the **data connection**.
- Handles the actual **file transfer** (upload/download).
- Comes into play only when the PI determines that data transfer is needed.

### How they work together:

1. The **client's user interface** communicates with the **PI**.
2. PI translates commands and sends them via the **control connection**.
3. The **server's PI** receives commands and, if data transfer is needed, activates the **DTP**.
4. **DTPs on both sides** handle the data transfer.
5. After transfer is complete, the data connection is **closed**.
6. Control returns to the **PIs** on both sides.

### Client vs. Server FTP structure:

- **Client FTP** has: User Interface + PI + DTP
- **Server FTP** has: PI + DTP (no user interface needed)

Both sides also have a **file system** — the client's local file system and the server's remote file system.

### 🎯 Exam Important Points

- **PI (Protocol Interpreter)** → manages control connection, translates commands.
- **DTP (Data Transfer Process)** → manages data connection, handles file transfer.
- Client FTP = **User Interface + PI + DTP**.
- Server FTP = **PI + DTP** (no user interface).
- PI works first (control), then DTP comes into play (data), then control returns to PI.

---

---

## 🔷 CONCEPT 11: Active Mode vs. Passive Mode (Port Details)

---

### 📌 Concept Name

**FTP Active Mode and Passive Mode (with Port Numbers)**

### 🧠 Simple Explanation

### Active Mode

| Connection | Client Port | Server Port |
|---|---|---|
| Control | N (where N > 1023) | **21** |
| Data | N + 1 | **20** |

- Client uses a large port (above 1023) for control.
- For data, client uses the **next port** (N+1) and server uses **port 20**.
- The **server initiates** the data connection.

### Passive Mode

| Connection | Client Port | Server Port |
|---|---|---|
| Control | N (where N > 1023) | **21** |
| Data | N + 1 | P (where P > 1023) |

- Control connection is the same.
- For data, the **server uses a random large port** (P > 1023) instead of port 20.
- The **client initiates** the data connection.

### 🎯 Exam Important Points

- Active mode: server port for data = **20**, server initiates data connection.
- Passive mode: server port for data = **any large port > 1023**, client initiates data connection.
- Control port is always **21** on server side.
- Client always uses ports **> 1023** (not reserved/restricted ports).

---

---

## 🔷 CONCEPT 12: File Transfer Modes in FTP

---

### 📌 Concept Name

**FTP File Transfer Modes — ASCII and Binary**

### 🧠 Simple Explanation

When transferring files via FTP, you need to specify the **type of file**:

**ASCII Mode:**

- Used for **text files**.
- Examples: `.txt`, `.html`, `.asp`, `.js`

**Binary Mode:**

- Used for **non-text files**.
- Examples: `.doc`, `.pdf`, `.mp3` (media files)

You can define which mode to use before transferring.

### 🎯 Exam Important Points

- Two file transfer modes: **ASCII** (text) and **Binary** (non-text).
- ASCII → txt, html | Binary → doc, pdf, media files.

---

---

## 🔷 CONCEPT 13: FTP — User's Perspective (Operations)

---

### 📌 Concept Name

**FTP Operations from a User's Point of View**

### 🧠 Simple Explanation

When a user uses FTP, they typically do the following (in order):

1. **Connect** to the remote host (the FTP server).
2. **Navigate** the directory structure on the remote server.
3. **List** the files available for transfer.
4. **Define** the transfer mode (ASCII/Binary), transfer type, and data structure.
5. **Transfer** data — either upload (put) or download (get).
6. **Disconnect** from the remote host when done.

### A Typical FTP Scenario (from transcript):

1. Log on to the FTP server.
2. Navigate to the correct remote directory.
3. Specify the file type (binary/ASCII).
4. **Send a file** → use the `PUT` command (upload from client to server).
5. **Retrieve a file** → use the `GET` command (download from server to client).
6. **Terminate** the session by quitting.

### 🎯 Exam Important Points

- FTP operations: Connect → Navigate → List → Define mode → Transfer → Disconnect.
- **PUT** = upload (client → server).
- **GET** = download (server → client).

---

---

## 🔷 CONCEPT 14: TFTP — Trivial File Transfer Protocol

---

### 📌 Concept Name

**TFTP (Trivial File Transfer Protocol)**

### 🧠 Simple Explanation

TFTP is a **simplified, lightweight version** of FTP. It is designed for situations where full FTP is too heavy or unnecessary.

**Key characteristics:**

- TFTP is a **disk-to-disk data transfer** protocol.
- It has a **simple architecture** — deliberately kept simple for easy implementation.
- It runs on top of **UDP** (User Datagram Protocol) — NOT TCP. So it is **not reliable**.
- The TFTP client initially sends a request through **well-known port 69** (not port 21 like FTP).
- After the initial request on port 69, the server and client **agree on other ports** for the rest of the session.

**Where is TFTP used?**

- **Diskless devices** downloading firmware at boot time.
- **Automated processes** where assigning user ID/password is not feasible.
- **Small, resource-constrained devices** where a full FTP implementation is too heavy (like routers, network devices).
- Uploading **configuration files** to routers and network devices.

**Limitations of TFTP:**

- Very limited — can only **read a file from a server** or **write a file to a server**.
- **No user authentication** — it is an **insecure protocol**.
- Security is handled by other mechanisms (like physical security or restricted network access).

### 🎯 Exam Important Points

| Feature | FTP | TFTP |
|---|---|---|
| Transport Layer | **TCP** | **UDP** |
| Port | **21** (control), **20** (data) | **69** |
| Reliability | Reliable | Not reliable |
| Authentication | Yes (USER, PASS) | **No authentication** |
| Complexity | Full-featured | Very simple |
| Use case | General file transfer | Firmware updates, config uploads in constrained devices |
| Defined in | RFC 959 | — |

### ⚠️ Common Confusions

| Confusion | Clarification |
|---|---|
| "TFTP uses TCP like FTP" | No. TFTP uses **UDP**. FTP uses TCP. |
| "TFTP uses port 21" | No. TFTP uses **port 69**. |
| "TFTP is secure" | No. TFTP has **no user authentication** — it is insecure. |
| "TFTP can do everything FTP can" | No. TFTP can only **read or write files** — no directory browsing, etc. |

---

---

## 🔷 CONCEPT 15: FTP Commands

---

### 📌 Concept Name

**FTP Commands — Access, File Management, Data Formatting, and File Transfer**

### 🧠 Simple Explanation

FTP defines several commands that the client can use. They are grouped into categories:

### A. Access Commands

| Command | Argument | Description |
|---|---|---|
| USER | Userid | User information (login name) |
| PASS | User password | Password for authentication |
| ACCT | Account to be charged | Account information |
| REIN | — | Reinitialize the session |
| QUIT | — | Log out of the system |
| ABOR | — | Abort the previous command |

### B. File Management Commands

| Command | Argument | Description |
|---|---|---|
| CWD | Directory name | Change to another directory |
| DELE | File name | Delete a file |
| LIST | Directory name | List files in a directory |
| NLIST | Directory name | List names of files in a directory |
| MKD | Directory name | Make (create) a new directory |
| PWD | — | Print working directory |
| RMD | Directory name | Remove (delete) a directory |
| RNFR | File name (old) | Rename file — specify old name |
| RNTO | File name (new) | Rename file — specify new name |

### C. Data Formatting Commands

| Command | Arguments | Description |
|---|---|---|
| TYPE | A (ASCII), E (EBCDIC), I (Image), N (Nonprint), T (Telnet) | Define the file type and print format |
| STRU | F (File), R (Record), P (Page) | Define the organization of data |
| MODE | S (Stream), B (Block), C (Compressed) | Define the transmission mode |

### D. File Transfer Commands

| Command | Argument | Description |
|---|---|---|
| RETR | File name | Retrieve (download) a file |
| STOR | File name | Store (upload) a file |
| STOU | File name | Same as STOR but file must not be overwritten |
| ALLO | File name | Allocate storage space for files |
| REST | File name | Restart file transfer from a specified point |
| STAT | — | Return the status |

### 🎯 Exam Important Points

- **USER, PASS** → for login/authentication.
- **QUIT** → log out.
- **ABOR** → abort previous command.
- **CWD** → change directory.
- **RETR** → retrieve/download file.
- **STOR** → store/upload file.
- **TYPE** → define file type (ASCII, Binary, etc.).
- **MODE** → define transmission mode (Stream, Block, Compressed).
- **STRU** → define data structure (File, Record, Page).

---

---

## 🔷 CONCEPT 16: DNS Role in FTP / Client-Server Communication

---

### 📌 Concept Name

**DNS Resolves Names to IP Addresses for Communication**

### 🧠 Simple Explanation

When you want to connect to a server (say, for FTP), you typically know its **name** (like `www.iitkgp.ac.in`). But the network layer only understands **IP addresses**, not names.

So, the **DNS (Domain Name System)** resolves the name into an IP address. Once the client has the server's IP, it can establish the connection.

This was briefly mentioned in the transcript in the context of how a client knows where the server is.

### 🎯 Exam Important Points

- Network layer understands only **IP addresses**.
- **DNS** converts a hostname into an IP address.
- Client must know the server's **IP address** (either directly or via DNS) to connect.

---

---

## ✅ Summary Table — Key Facts from Lecture 7

| Topic | Key Point |
|---|---|
| Client-Server Model | Server offers service; Client requests service; Roles are asymmetric |
| Iterative Server | Serves one client at a time; single copy runs |
| Concurrent Server | Serves multiple clients; forks child processes |
| 5-Tuple | Protocol, Source IP, Source Port, Dest IP, Dest Port |
| Socket | Mechanism for IPC; like a telephone |
| Half-Association | 3-tuple: Protocol, IP, Port |
| FTP | File transfer; TCP-based; RFC 959 |
| FTP Control Connection | Port 21; sends commands & responses |
| FTP Data Connection | Port 20; sends actual files; unilateral |
| FTP Active Mode | Server initiates data connection; server uses port 20 |
| FTP Passive Mode | Client initiates data connection; server uses random port > 1023 |
| DTP | Data Transfer Process — handles file transfer |
| PI | Protocol Interpreter — handles control commands |
| ASCII Mode | For text files (txt, html) |
| Binary Mode | For non-text files (doc, pdf, mp3) |
| TFTP | Trivial FTP; uses UDP; port 69; no authentication |
| PUT / STOR | Upload file (client → server) |
| GET / RETR | Download file (server → client) |
| DNS | Resolves name to IP for connecting to the server |

---

---

## 📝 10 MCQs — Strictly from Lecture 7

---

### Q1. What is the default port number for FTP control connection?

A) 20
B) 25
C) 21
D) 69

**Answer: C) 21**

**Explanation:** The FTP control connection, which handles commands and responses, uses **port 21** by default. Port 20 is for the data connection.

---

### Q2. What transport layer protocol does TFTP use?

A) TCP
B) UDP
C) IP
D) ICMP

**Answer: B) UDP**

**Explanation:** Unlike FTP which uses TCP, TFTP is implemented on top of **UDP** (User Datagram Protocol). This makes it simpler but not reliable.

---

### Q3. A connection in TCP/IP is uniquely identified by a:

A) 3-tuple
B) 4-tuple
C) 5-tuple
D) 2-tuple

**Answer: C) 5-tuple**

**Explanation:** A connection is defined by 5 components: Protocol, Source IP, Source Port, Destination IP, and Destination Port.

---

### Q4. In FTP, the data connection is:

A) Bidirectional — data flows both ways simultaneously
B) Unilateral — data flows in one direction at a time
C) Always initiated by the client
D) Always on port 21

**Answer: B) Unilateral — data flows in one direction at a time**

**Explanation:** The FTP data connection is unilateral. File can transfer either from client to server or server to client, but **not both simultaneously**.

---

### Q5. A socket is also called a:

A) Full-association (5-tuple)
B) Half-association (3-tuple)
C) Quarter-association (2-tuple)
D) None of the above

**Answer: B) Half-association (3-tuple)**

**Explanation:** A socket is a 3-tuple (Protocol, Local IP, Local Port) and is also called a **half-association**. Two half-associations form a full association (5-tuple).

---

### Q6. What is the well-known port number used by TFTP?

A) 21
B) 20
C) 80
D) 69

**Answer: D) 69**

**Explanation:** The TFTP client initially sends a request through **well-known port 69**. This is different from FTP which uses port 21.

---

### Q7. In FTP active mode, the data connection is initiated by:

A) The client
B) The server
C) Both simultaneously
D) Neither — it uses UDP

**Answer: B) The server**

**Explanation:** In active mode, the **server** initiates the data connection (and uses port 20). In passive mode, the client initiates it.

---

### Q8. Which of the following is TRUE about an iterative server?

A) It forks a child process for each client
B) It serves all clients simultaneously
C) A single copy of the server runs at all times
D) It does not require clients to wait

**Answer: C) A single copy of the server runs at all times**

**Explanation:** In an iterative server, only a **single copy** runs. It handles one request at a time, and other clients **must wait** if the server is busy.

---

### Q9. Which FTP process manages the control connection?

A) DTP (Data Transfer Process)
B) PI (Protocol Interpreter)
C) User Interface
D) DNS

**Answer: B) PI (Protocol Interpreter)**

**Explanation:** The **Protocol Interpreter (PI)** manages the control connection, translates user commands into FTP commands, and communicates with the server. The DTP handles the data connection.

---

### Q10. Which of the following is NOT a characteristic of TFTP?

A) It uses UDP
B) It has no user authentication
C) It uses port 69
D) It supports full directory browsing and manipulation

**Answer: D) It supports full directory browsing and manipulation**

**Explanation:** TFTP is very limited. It can only **read a file from a server** or **write a file to a server**. It does NOT support directory browsing, navigation, or manipulation. That is a feature of full FTP.

---

---

## 🏁 End of Lecture 7 — Complete

All concepts from the transcript have been covered:

1. Client-Server Model (definition, philosophy)
2. Server Lifecycle (initialize → sleep → serve → sleep)
3. Iterative vs. Concurrent Servers
4. 5-Tuple (connection identification)
5. Developing a Network Application (layer-wise protocols)
6. Sockets (IPC mechanism)
7. Association and Half-Association
8. FTP Overview (TCP, RFC 959)
9. FTP Two Connections (Control port 21, Data port 20)
10. FTP Processes (PI and DTP)
11. Active Mode vs. Passive Mode
12. File Transfer Modes (ASCII, Binary)
13. FTP User Operations (connect, navigate, list, transfer, disconnect)
14. TFTP (UDP, port 69, no authentication)
15. FTP Commands (Access, File Management, Data Formatting, File Transfer)
16. DNS role in resolving server names

---
