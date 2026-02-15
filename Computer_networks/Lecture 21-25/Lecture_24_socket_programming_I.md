# Lecture 24: SOCKET PROGRAMMING – I

## 📚 Course: Computer Networks and Internet Protocol
### Prof. Sandip Chakraborty | IIT Kharagpur | NPTEL

---

## 📌 Concept 1: Connecting Network with the Operating System

### 🧠 Simple Explanation

Before we learn socket programming, we need to understand **where the network protocol stack lives** inside a computer.

The TCP/IP protocol stack has **5 layers**. These layers are NOT all in the same place inside your computer:

- **Physical Layer** and **part of the Data Link Layer** → These are implemented in **hardware** (like your network card).
- **Upper part of the Data Link Layer** (MAC and Logical Link Control), **Network Layer**, and **Transport Layer** → These are implemented inside the **kernel space** of the operating system.
- **Application Layer** → This is where YOU write your programs. This lives in the **user space**.

So the picture is:
- **User Space**: Your applications (browser, FTP client, VoIP app, etc.)
- **Kernel Space**: Transport Layer, Network Layer, upper Data Link Layer
- **Hardware**: Physical Layer, lower Data Link Layer

Now the key question is: **How does your application (in user space) talk to the protocol stack (in kernel space)?**

The answer is: through **system calls**. In a UNIX/Linux based operating system, when your application needs to use the network, it makes system calls. These system calls transfer your request from user space to the kernel, where the actual protocol stack does the work.

If you want to explore the protocol stack implementation yourself, you can download the UNIX kernel source and check the **net module** under `/usr/src/linux/net/`.

### 🎯 Exam Important Points
- The TCP/IP protocol stack is implemented inside the **OS kernel**.
- Physical layer and part of data link layer are in **hardware**.
- Transport, Network, and upper Data Link layers are in **kernel space**.
- Applications run in **user space**.
- The interaction between user space and kernel space happens through **system calls**.
- In UNIX, the network protocol stack code is in the **net module** of the kernel.

### ⚠️ Common Confusions
- Students often think the entire protocol stack is in hardware. **No** — only the bottom layers are in hardware. Most of it is in the OS kernel.
- Don't confuse "kernel space" with "hardware." Kernel space is still **software**, but it runs with special privileges.

### 📝 Possible NPTEL-style Questions
- "Where is the transport layer implemented?" → **Inside the OS kernel space**
- "How does a user application interact with the TCP/IP stack?" → **Through system calls**

---

## 📌 Concept 2: Application Layer Multiplexing (IP Address + Port Number)

### 🧠 Simple Explanation

On a single computer, you can run **multiple applications** at the same time — a browser, a file transfer program, a VoIP application, etc. Each of these may use a different transport protocol:
- **HTTP** (browsing) → uses **TCP**
- **FTP** (file transfer) → uses **TCP**
- **VoIP** (voice over internet) → uses **UDP**

All of these applications share the same network (IP) layer below. So how do we tell them apart? This is called **application layer multiplexing**, and it is done by the transport layer.

Two key identifiers are used:
1. **IP Address** → Identifies **which device** (machine) in the network. It works at the IP/Network layer.
2. **Port Number** → Identifies **which application** (process) on that device. It works at the transport layer.

**Example from the transcript:**
- Device A has IP address `202.141.81.2`. A browser application runs on port `8081` using TCP.
- Device B has IP address `203.12.23.43`. An HTTP server runs on port `8080`.
- When Device A's browser communicates with Device B's HTTP server, the communication is identified by the combination of IP addresses and port numbers.

In a UNIX system, each application is represented as a **process**. So when we say "application-to-application communication," we really mean **process-to-process communication**. This process-to-process communication is achieved using the transport layer, which uses the IP address (to find the machine) and the port number (to find the specific process on that machine).

### 🎯 Exam Important Points
- **IP address** → uniquely identifies a **machine** in the network.
- **Port number** → uniquely identifies a **process/application** on a machine.
- Transport layer does **application layer multiplexing** using port numbers.
- Different applications can use different protocols (TCP or UDP).
- In UNIX, applications are represented as **processes**.

### ⚠️ Common Confusions
- IP address does NOT identify an application — it identifies a machine. Port number identifies the application on that machine.
- Multiple applications can run on the same machine, each with a different port number.

### 📝 Possible NPTEL-style Questions
- "What is used to differentiate two devices at the IP layer?" → **IP address**
- "What enables application layer multiplexing at the transport layer?" → **Port numbers**

---

## 📌 Concept 3: What is a Socket?

### 🧠 Simple Explanation

A **socket** is basically a **logical pipe** (a logical connection) that connects one process to another process across the network.

Think of it this way:
- Process A is running on Machine 1 (at IP `202.141.81.2`, port `8081`).
- Process B is running on Machine 2 (at IP `203.12.23.43`, port `8080`).
- A socket creates a **logical pipe** between these two processes.

You can have **multiple such logical pipes** at the transport layer — each one is a separate socket.

**Sending data over the internet means sending data through these logical pipes (sockets).**

Now, the behavior of the socket depends on the transport protocol:
- In case of **TCP** → the socket creates an **end-to-end connection** (like a dedicated phone line).
- In case of **UDP** → the socket creates **end-to-end data transmission semantics** (like sending a letter — no dedicated connection, just send and hope it arrives).

### 🎯 Exam Important Points
- A socket is a **logical pipe** between two processes.
- Sockets enable **process-to-process communication** across the network.
- A socket is uniquely identified by the combination of **IP address + port number** on both ends.
- TCP sockets are **connection-oriented**; UDP sockets are **connectionless**.

### ⚠️ Common Confusions
- A socket is NOT a physical wire. It is a **logical** concept — a software abstraction.
- A socket connects **processes**, not just machines. One machine can have many sockets open at the same time.

### 📝 Possible NPTEL-style Questions
- "What is a socket in networking?" → **A logical pipe/connection between two processes**
- "What creates the end-to-end connection in case of TCP?" → **The socket**

---

## 📌 Concept 4: Socket Programming Framework (Client-Server Model)

### 🧠 Simple Explanation

Socket programming is how you actually write code to use sockets. It follows a **client-server model**. The communication happens through a series of **system calls** that you execute from a C program. These system calls help you get services from the TCP/IP protocol stack inside the OS kernel.

Here is the overall flow:

**SERVER SIDE (step by step):**
1. **`socket()`** → Creates the server-side end of the logical pipe.
2. **`bind()`** → Binds the socket to a specific port number. This is like announcing: "I am listening on port 8080."
3. **`listen()`** → Puts the server in **listening state**, waiting for incoming connections.
4. **`accept()`** → Accepts an incoming connection from a client. In TCP, the three-way handshake happens between `connect()` and `accept()`.
5. **`recv()` / `send()`** → Receive data from or send data to the client.
6. **`close()`** → Closes the connection.

**CLIENT SIDE (step by step):**
1. **`socket()`** → Creates the client-side end of the logical pipe.
2. **`connect()`** → Initiates a connection to the server's announced port.
3. **`send()` / `recv()`** → Send data to or receive data from the server.
4. **`close()`** → Closes the connection.

**Why does the client NOT need `bind()` and `listen()`?**
Because the **server** is the one making the announcement ("I am listening at port 8080"). The **client** is the one who initiates the connection — it already knows the server's IP and port. So the client just connects directly. The client does NOT need to announce anything.

**TCP Three-Way Handshake (within connect and accept):**
- In case of TCP, when the client calls `connect()` and the server calls `accept()`, the TCP three-way handshake happens automatically:
  1. Client sends **SYN** packet to server.
  2. Server replies with **SYN + ACK** packet.
  3. Client sends **ACK** packet back.
- After this, the connection is established and data transfer can begin.

### 🎯 Exam Important Points
- Server calls: `socket()` → `bind()` → `listen()` → `accept()` → `recv()/send()` → `close()`
- Client calls: `socket()` → `connect()` → `send()/recv()` → `close()`
- **`bind()` and `listen()` are ONLY on the server side**, NOT on the client side.
- TCP three-way handshake occurs between `connect()` (client) and `accept()` (server).
- The server announces its port; the client initiates the connection.

### ⚠️ Common Confusions
- Many students think `bind()` is needed on both sides. **No — only the server binds to a fixed port.**
- The client does NOT call `listen()` or `bind()` — it only calls `socket()`, `connect()`, then sends/receives data.
- `accept()` is only on the server side.

### 📝 Possible NPTEL-style Questions
- "Which system calls are used only at the server side?" → **`bind()`, `listen()`, `accept()`**
- "What happens between connect() and accept() in TCP?" → **Three-way handshake (SYN, SYN+ACK, ACK)**

---

## 📌 Concept 5: Types of Sockets — Stream, Datagram, and Raw

### 🧠 Simple Explanation

Since the internet is a tradeoff between **performance** and **reliability**, we have two transport protocols (TCP and UDP). Accordingly, we have **two main types of sockets** (plus a third special one):

**1. Stream Socket (SOCK_STREAM):**
- This is a **TCP-based** socket.
- It is **reliable** and **connection-oriented**.
- Data arrives in order, without duplicates, and delivery is guaranteed.
- Used when you need reliability (e.g., file transfer, web browsing).

**2. Datagram Socket (SOCK_DGRAM):**
- This is a **UDP-based** socket.
- It is **unreliable** and **connectionless**.
- Data may arrive out of order, may be duplicated, or may be lost.
- Used when you need performance (e.g., VoIP, multimedia streaming).

**3. Raw Socket:**
- This is a special type of socket.
- It **bypasses the transport layer** and directly interacts with the **IP layer**.
- The transcript mentions this but says it will not be discussed in detail.

### 🎯 Exam Important Points
- **SOCK_STREAM** = TCP = Reliable + Connection-Oriented
- **SOCK_DGRAM** = UDP = Unreliable + Connectionless
- **Raw Socket** = Bypasses transport layer, directly accesses IP layer
- Stream socket is for reliability-focused applications.
- Datagram socket is for performance-focused applications.

### ⚠️ Common Confusions
- "Stream" does NOT mean video streaming. SOCK_STREAM means a continuous, reliable stream of bytes (TCP).
- Datagram socket is NOT inherently bad — it is chosen deliberately when speed matters more than reliability.

### 📝 Possible NPTEL-style Questions
- "Which type of socket provides reliable, connection-oriented service?" → **SOCK_STREAM**
- "What does a raw socket allow?" → **Bypasses transport layer to directly interact with IP layer**

---

## 📌 Concept 6: The `socket()` System Call

### 🧠 Simple Explanation

To create a socket, you use the `socket()` system call. Here is the syntax:

```c
int s;
s = socket(domain, type, protocol);
```

This call takes **three parameters**:

1. **`domain`** — The communication domain (address family).
   - We normally use **`AF_INET`** which stands for IPv4 protocol.
   - This tells the system that we are using IPv4 internet addresses.

2. **`type`** — The type of socket.
   - **`SOCK_STREAM`** → for a TCP socket
   - **`SOCK_DGRAM`** → for a UDP socket

3. **`protocol`** — The protocol family to use.
   - Usually set to **`0`** (the system will choose the appropriate protocol automatically based on the type).

The `socket()` call returns a **socket ID** (an integer). This socket ID is like a handle — you use it in all subsequent calls (bind, listen, connect, etc.) to refer to this particular socket.

### 🎯 Exam Important Points
- `socket()` takes 3 parameters: **domain, type, protocol**.
- `AF_INET` is used for **IPv4** addresses.
- `SOCK_STREAM` = TCP socket; `SOCK_DGRAM` = UDP socket.
- The protocol parameter is usually set to **0**.
- The return value is the **socket ID** (integer) used in later system calls.

### ⚠️ Common Confusions
- `AF_INET` is NOT a type of socket — it is the **address family** (domain). The type is SOCK_STREAM or SOCK_DGRAM.
- Setting protocol to 0 does NOT mean "no protocol." It means "let the system choose the default protocol for the given type."

### 📝 Possible NPTEL-style Questions
- "What does AF_INET represent in the socket() call?" → **IPv4 address family**
- "What is the return value of the socket() system call?" → **A socket ID (integer)**

---

## 📌 Concept 7: The `bind()` System Call

### 🧠 Simple Explanation

After creating a socket on the **server side**, you need to **bind** that socket to a specific port number. This is done using the `bind()` system call.

```c
int status;
status = bind(s, &address, address_size);
```

It takes **three parameters**:

1. **`s`** — The socket ID returned by the `socket()` call.
2. **`&address`** — A pointer to a structure (`struct sockaddr_in`) that contains the **IP address** and **port number** of the server.
3. **`address_size`** — The size of the address structure.

The `bind()` call returns a **status** — whether the bind was successful or not.

The `address` structure is of type **`struct sockaddr_in`** and has these important fields:
- **`sin_family`** → Set to `AF_INET` (for IPv4).
- **`sin_addr.s_addr`** → The source IP address. Set to **`INADDR_ANY`** to choose the local address of the machine automatically.
- **`sin_port`** → The port number. But this must be converted from **host byte order** to **network byte order** using the function **`htons()`**.

### 🎯 Exam Important Points
- `bind()` is used on the **server side** to bind a socket to a port.
- The `sockaddr_in` structure contains: `sin_family`, `sin_addr.s_addr`, and `sin_port`.
- `INADDR_ANY` automatically uses the machine's local IP address.
- Port number must be converted using **`htons()`** (host to network short).
- `bind()` returns a status (success or failure).

### ⚠️ Common Confusions
- `bind()` is only called on the **server side**, NOT on the client side.
- Don't forget `htons()` for the port number — without it, the port number may be interpreted incorrectly on machines with different byte orders.

### 📝 Possible NPTEL-style Questions
- "What function is used to bind a socket to a port?" → **`bind()`**
- "What structure holds the address and port for binding?" → **`struct sockaddr_in`**

---

## 📌 Concept 8: Byte Order — Little Endian vs Big Endian

### 🧠 Simple Explanation

This is a very important concept for understanding why we need `htons()`.

Different computer systems store data in memory in different ways:

**Little Endian:**
- Stores the **least significant byte first** (from right to left).
- Example: If the data in a register is `0A 0B 0C 0D`, in memory it is stored as: `0D 0C 0B 0A`.

**Big Endian:**
- Stores the **most significant byte first** (from left to right).
- Example: If the data in a register is `0A 0B 0C 0D`, in memory it is stored as: `0A 0B 0C 0D`.

**The Problem:**
Imagine a little endian machine sends data to a big endian machine. The little endian machine sends bytes in the order `0D 0C 0B 0A`. But the big endian machine receives `0D` first and puts it in the first position, resulting in the bytes being interpreted in the **wrong order**.

**The Solution — Network Byte Order:**
To solve this inconsistency, we define a **standard byte order for the network** (which is big endian). Before sending data over the network:
1. Convert from **host byte order** → **network byte order** (using `htons()` for short values like port numbers).
2. Send data over the network.
3. At the receiving end, convert from **network byte order** → **host byte order**.

This way, no matter if the sender is little endian or big endian, the data will be correctly interpreted at the receiving end.

**The function `htons()`** stands for "**Host TO Network Short**" — it converts a short integer (like a port number) from host byte order to network byte order.

### 🎯 Exam Important Points
- **Little Endian** = least significant byte stored first.
- **Big Endian** = most significant byte stored first.
- The **network byte order** is a standard fixed byte order for transmitting data.
- **`htons()`** converts host byte order to network byte order (for short integers like port numbers).
- This conversion prevents misinterpretation of data between different machine architectures.

### ⚠️ Common Confusions
- `htons()` is NOT optional — you MUST use it for port numbers to ensure correct communication across different architectures.
- Network byte order is a **fixed** standard (big endian), regardless of the machine type.

### 📝 Possible NPTEL-style Questions
- "Why is htons() needed when specifying a port number?" → **To convert host byte order to network byte order for consistency across different architectures**
- "What is the difference between little endian and big endian?" → **Little endian stores least significant byte first; big endian stores most significant byte first**

---

## 📌 Concept 9: Setting Up the Address Variable (Example)

### 🧠 Simple Explanation

Here is a concrete example from the transcript of how you set up the address structure for a server:

```c
int port = 3028;
server_address.sin_family = AF_INET;             // IPv4 address family
server_address.sin_addr.s_addr = INADDR_ANY;     // Use local machine's IP
server_address.sin_port = htons(port);            // Convert port to network byte order
```

What this does:
- Sets the address family to IPv4 (`AF_INET`).
- Uses `INADDR_ANY` to automatically pick the IP address of the machine where the code is running. You could also manually put a specific IP address here, but it must match the IP address of your network interface.
- Sets the port to 3028, converted to network byte order using `htons()`.

### 🎯 Exam Important Points
- `AF_INET` → IPv4
- `INADDR_ANY` → automatically selects the local machine's IP address.
- Port number MUST be passed through `htons()`.
- You can also manually set a specific IP address, but it must match your network interface's IP.

---

## 📌 Concept 10: The `listen()` System Call and the `accept()` System Call

### 🧠 Simple Explanation

**`listen()`:**
After `bind()`, the server calls `listen()` to start **waiting for incoming connections**.

```c
listen(sock_fd, 5);
```

The second parameter (here `5`) is the **backlog** — it specifies **how many maximum connections can be waiting in the queue** (backlogged) while the server is busy handling another connection. If more clients try to connect beyond this limit, they will be refused.

**`accept()`:**
Once a client tries to connect, the server calls `accept()` to **accept the incoming connection**.

```c
new_sock_fd = accept(sock_fd, &client_address, &addr_length);
```

Key points about `accept()`:
- It takes the **socket ID** on which the server is listening.
- It fills in the **client's address** information (so the server knows who connected).
- It returns a **NEW socket ID** (`new_sock_fd`). This new socket ID represents the specific connection to that particular client.

**Why a new socket ID?** Because the server may have **multiple clients** connecting simultaneously. Each client gets its own logical pipe (its own socket). The original `sock_fd` continues to listen for new connections, while each `new_sock_fd` handles communication with a specific client.

### 🎯 Exam Important Points
- `listen()` puts the server in a **listening/waiting state**.
- The backlog parameter of `listen()` specifies max pending connections in the queue.
- `accept()` returns a **new socket ID** for each client connection.
- The original socket continues listening; new sockets handle individual clients.
- After `accept()`, the client's address is stored in the `client_address` variable.

### ⚠️ Common Confusions
- `accept()` does NOT use the same socket ID as `listen()`. It creates a **new** socket ID for the accepted connection.
- The backlog parameter is NOT the total number of clients the server can ever handle — it is the number of clients that can **wait in the queue** at the same time.

### 📝 Possible NPTEL-style Questions
- "Why does accept() return a new socket ID?" → **Because each client connection needs its own separate logical pipe/socket**
- "What does the backlog parameter in listen() represent?" → **Maximum number of pending connections in the queue**

---

## 📌 Concept 11: Active Open vs Passive Open

### 🧠 Simple Explanation

There are two types of connection opening:

**Passive Open (Server Side):**
- The server announces its address, binds to a port, and **waits** for incoming connections.
- It remains in an **open state**, ready for any client to connect.
- The server does NOT initiate the connection — it passively waits.

**Active Open (Client Side):**
- The client **actively initiates** a connection to the server.
- The client opens a connection only when there is a **need for data transfer**.
- The connection is initialized by the client.

So in summary: **Server = Passive Open** (waits) | **Client = Active Open** (initiates).

### 🎯 Exam Important Points
- **Passive open** = Server side; server waits for connections.
- **Active open** = Client side; client initiates the connection.
- The connection is always initialized by the **client**.
- The server must already be in a listening (passive) state before the client can connect.

### ⚠️ Common Confusions
- "Passive" does NOT mean the server is doing nothing. It means the server is actively waiting and ready, but it does not start the connection itself.

### 📝 Possible NPTEL-style Questions
- "Which side of the connection performs passive open?" → **Server**
- "Which side initiates the connection?" → **Client (active open)**

---

## 📌 Concept 12: Data Transfer — Stream Socket vs Datagram Socket Functions

### 🧠 Simple Explanation

Once a connection is established (or in case of UDP, once sockets are created), data can be transferred. The functions used are **different** for stream sockets and datagram sockets:

**For Stream Socket (TCP):**
- Use **`read()`** and **`write()`** functions.
- `read(new_sock_fd, buffer, length)` → Reads data from the socket into a buffer.
- `write(new_sock_fd, message, length)` → Writes data from a buffer to the socket.
- You use the **new socket ID** returned by `accept()` (not the original listening socket).

**For Datagram Socket (UDP):**
- Use **`recvfrom()`** and **`sendto()`** functions.
- `recvfrom()` → Receives data from a socket (also gets the sender's address).
- `sendto()` → Sends data to a socket at a specified destination address.

The key difference: In TCP, since a connection already exists, you just read/write. In UDP, since there is NO connection, each `sendto()` and `recvfrom()` must specify **who** to send to or **who** sent the data.

### 🎯 Exam Important Points
- **TCP (Stream)** uses `read()` and `write()`.
- **UDP (Datagram)** uses `recvfrom()` and `sendto()`.
- For TCP data transfer, use the **new socket ID** from `accept()`, NOT the listening socket.
- UDP functions include address information because there is no pre-established connection.

### ⚠️ Common Confusions
- Do NOT use `read()/write()` for UDP or `recvfrom()/sendto()` for TCP — each socket type has its own data transfer functions.
- In TCP, the new_sock_fd from `accept()` is used for data transfer, not the original sock_fd.

### 📝 Possible NPTEL-style Questions
- "Which functions are used for data transfer in a datagram socket?" → **`recvfrom()` and `sendto()`**
- "Which functions are used for data transfer in a stream socket?" → **`read()` and `write()`**

---

## 📌 Concept 13: UDP Server – How It Works (Demo Code Walkthrough)

### 🧠 Simple Explanation

The transcript walks through a **UDP server** code. Here is the step-by-step flow:

**Step 1:** Include standard headers.

**Step 2:** Inside `main()`:
- Declare the address structure (`struct sockaddr_in`) for the server.
- Declare a socket identifier.
- Define a port number.

**Step 3:** Create the socket:
```c
socket(AF_INET, SOCK_DGRAM, 0)
```
- `AF_INET` = IPv4
- `SOCK_DGRAM` = UDP socket (datagram)
- `0` = default protocol

If there is an error during socket creation, print an error message.

**Step 4:** Set up the server address:
- `sin_family = AF_INET`
- `sin_addr.s_addr = INADDR_ANY` (use local machine's address)
- `sin_port = htons(port)` (port from command line argument)

**Step 5:** Call `bind()` to bind the socket to the port.

**Step 6:** Set socket options using `setsockopt()` with `SO_REUSEADDR`:
- This option allows the same port to be reused for multiple connections.
- The transcript notes this is **not a safe idea** but is sometimes useful.

**Step 7:** Declare buffers for receiving data, and a client address variable.

**Step 8:** Call **`recvfrom()`** to receive data.
- **Important for UDP:** There is NO `connect()` or `accept()` call. Because UDP is connectionless, you do NOT need to establish a connection. You can directly receive data after binding.
- The `recvfrom()` function also captures the **client's address** details.

**Step 9:** After receiving data, call **`sendto()`** to send a response back to the client.

### 🎯 Exam Important Points
- UDP server does **NOT** use `connect()` or `accept()` — these are TCP-specific.
- UDP server uses `recvfrom()` and `sendto()` directly after `bind()`.
- `setsockopt()` with `SO_REUSEADDR` allows port reuse.
- No connection establishment happens in UDP.

### ⚠️ Common Confusions
- The biggest confusion: students try to use `connect()` and `accept()` for UDP. **These are NOT needed for UDP.**
- `SO_REUSEADDR` is a convenience option, NOT a required step.

### 📝 Possible NPTEL-style Questions
- "In a UDP server, which calls are NOT needed?" → **`connect()` and `accept()`**
- "What does SO_REUSEADDR do?" → **Allows the same port to be reused for multiple connections**

---

## 📌 Concept 14: UDP Client – How It Works (Demo Code Walkthrough)

### 🧠 Simple Explanation

The transcript also walks through the **UDP client** code:

**Step 1:** Declare the server address structure.

**Step 2:** From the command line, take:
- The **hostname** (name/IP of the server)
- The **port** address where the server is listening.

**Step 3:** Create the socket:
```c
socket(AF_INET, SOCK_DGRAM, 0)
```
Same as server — `AF_INET`, `SOCK_DGRAM` (UDP), protocol `0`.

**Step 4:** Get the server IP address.

**Step 5:** Set up the server address:
- `sin_family`, host address, and server port (values provided through the command line).

**Step 6:** **Directly call `sendto()`** to send data to the server.
- No connection is established (no `connect()` call).
- Just specify the server address in the `sendto()` function and send.

**Step 7:** Call **`recvfrom()`** to receive the response from the server.

**Step 8:** Print the received data.

**Key observation from the demo:**
- The client sends a message "hello dear" to the server.
- The server receives it and sends it back.
- The client receives and prints the echoed message.

### 🎯 Exam Important Points
- UDP client also does NOT use `connect()` or `accept()`.
- UDP client directly uses `sendto()` and `recvfrom()`.
- Client takes server hostname and port as command line arguments.
- No connection establishment in UDP — data is sent directly.

---

## 📌 Concept 15: Running the UDP Server and Client (Demo Observations)

### 🧠 Simple Explanation

The transcript shows a live demo of running the UDP server and client on the **same machine**:

**Running the server:**
```
./server 2333
```
- The server binds to port **2333** and starts waiting.

**Running the client:**
```
./client localhost 2333
```
- The hostname is **localhost** (because both are on the same machine).
- The port is **2333** (the port where the server is bound).

**What happens:**
- The client sends a message to the server.
- The server receives the message and sends it back.
- The client receives and prints the message.

**Key observation about client port behavior:**
- The server binds to a **fixed, well-known port** (2333 in this example).
- But the **client does NOT bind** to any specific port. During runtime, the client **randomly chooses** a port address.
- Each time you run the client, it picks a **different port number**.
- This is because the client does not need a well-known port — the server needs to be found, not the client.

### 🎯 Exam Important Points
- Server binds to a **fixed port** (well-known port).
- Client does **NOT bind** to a fixed port — it gets a **random port** assigned at runtime.
- Every time the client runs, it may get a **different port number**.
- `localhost` can be used as the hostname when server and client are on the same machine.

### ⚠️ Common Confusions
- The client's port is NOT the same as the server's port. The client gets a random ephemeral port.
- Running the client multiple times produces different client port numbers each time.

### 📝 Possible NPTEL-style Questions
- "Why does the client port change with every run?" → **Because the client does not bind to a fixed port; it randomly selects a port at runtime**
- "What hostname is used when server and client are on the same machine?" → **localhost**

---

## 📌 Concept 16: Key Difference — TCP Server Flow vs UDP Server Flow

### 🧠 Simple Explanation

Let's clearly compare the system call flow for TCP and UDP:

| Step | TCP Server | UDP Server |
|------|-----------|------------|
| 1 | `socket()` | `socket()` |
| 2 | `bind()` | `bind()` |
| 3 | `listen()` | ❌ Not needed |
| 4 | `accept()` | ❌ Not needed |
| 5 | `read()` / `write()` | `recvfrom()` / `sendto()` |
| 6 | `close()` | `close()` |

| Step | TCP Client | UDP Client |
|------|-----------|------------|
| 1 | `socket()` | `socket()` |
| 2 | `connect()` | ❌ Not needed |
| 3 | `send()` / `recv()` | `sendto()` / `recvfrom()` |
| 4 | `close()` | `close()` |

**Key takeaway:** In UDP, there is no connection — no `listen()`, no `accept()`, no `connect()`. You just create the socket, bind (on the server), and directly send/receive.

The transcript mentions that the **TCP server and TCP client demo will be shown in the next class** (Lecture 25).

### 🎯 Exam Important Points
- TCP needs `listen()`, `accept()`, `connect()` — UDP does NOT.
- TCP uses `read()/write()` — UDP uses `recvfrom()/sendto()`.
- This comparison is a very common exam question pattern.

---

## 📝 Summary Table — All Key Concepts of Lecture 24

| Concept | Key Point |
|---------|-----------|
| Protocol Stack Location | Transport, Network, upper Data Link → Kernel Space; Physical, lower Data Link → Hardware; Application → User Space |
| System Calls | Bridge between user space and kernel space in UNIX |
| IP Address | Identifies a machine in the network |
| Port Number | Identifies an application/process on a machine |
| Socket | Logical pipe between two processes |
| SOCK_STREAM | TCP based, reliable, connection-oriented |
| SOCK_DGRAM | UDP based, unreliable, connectionless |
| Raw Socket | Bypasses transport layer, accesses IP directly |
| `socket()` params | domain (AF_INET), type (STREAM/DGRAM), protocol (0) |
| `bind()` | Binds socket to a specific port (server only) |
| `htons()` | Converts host byte order to network byte order |
| Little Endian | Least significant byte stored first |
| Big Endian | Most significant byte stored first |
| `listen()` | Server enters listening/waiting state |
| `accept()` | Server accepts connection, returns new socket ID |
| Passive Open | Server waits for connections |
| Active Open | Client initiates connection |
| TCP data transfer | `read()` and `write()` |
| UDP data transfer | `recvfrom()` and `sendto()` |
| UDP difference | No `connect()`, `accept()`, or `listen()` needed |
| Client port | Randomly assigned at runtime; changes each run |

---

## 📝 10 MCQs — Strictly from Lecture 24 Transcript

---

### Q1. Where is the transport layer of the TCP/IP protocol stack implemented?

(A) In the hardware  
(B) In the user space  
(C) Inside the kernel space of the operating system  
(D) In the application layer  

**✅ Answer: (C)**  
**Explanation:** As per the transcript, the transport layer, network layer, and upper part of the data link layer are implemented inside the kernel space of the operating system. The physical layer and part of the data link layer are in hardware. Applications run in user space.

---

### Q2. What is a socket in networking?

(A) A physical wire connecting two machines  
(B) A logical pipe connecting one process to another process  
(C) A hardware component on the network card  
(D) A type of IP address  

**✅ Answer: (B)**  
**Explanation:** The transcript defines a socket as a "logical connection from one process to another process." It creates a logical pipe between two processes running on different machines. It is NOT a physical entity.

---

### Q3. Which system calls are specific to the server side in TCP socket programming?

(A) socket(), connect(), close()  
(B) bind(), listen(), accept()  
(C) send(), recv(), close()  
(D) socket(), sendto(), recvfrom()  

**✅ Answer: (B)**  
**Explanation:** The transcript clearly states that `bind()` and `listen()` are not needed on the client side because the client initiates the connection. `accept()` is also server-side only. The client only needs `socket()`, `connect()`, then data transfer calls.

---

### Q4. What does AF_INET represent in the socket() system call?

(A) A type of TCP connection  
(B) The IPv4 address family  
(C) A UDP datagram type  
(D) A raw socket identifier  

**✅ Answer: (B)**  
**Explanation:** The transcript explains that `AF_INET` is the address family for IPv4 protocol. It is set as the domain parameter in the `socket()` call to indicate that we are using IPv4 addresses.

---

### Q5. Why is the function htons() used when setting the port number?

(A) To encrypt the port number  
(B) To convert the port number from host byte order to network byte order  
(C) To assign a random port number  
(D) To validate the port number  

**✅ Answer: (B)**  
**Explanation:** The transcript explains that different machines may be little endian or big endian, causing inconsistency in data interpretation. `htons()` (Host To Network Short) converts the port number to a standard network byte order to avoid this problem.

---

### Q6. In a little endian system, if the register holds data 0A 0B 0C 0D, how is it stored in memory?

(A) 0A 0B 0C 0D  
(B) 0D 0C 0B 0A  
(C) 0B 0A 0D 0C  
(D) 0C 0D 0A 0B  

**✅ Answer: (B)**  
**Explanation:** The transcript explicitly states that in a little endian system, data is stored from right to left. So the least significant byte (0D) is stored first, followed by 0C, 0B, and finally 0A.

---

### Q7. Which type of socket is connection-oriented and reliable?

(A) SOCK_DGRAM  
(B) SOCK_STREAM  
(C) Raw Socket  
(D) SOCK_RAW  

**✅ Answer: (B)**  
**Explanation:** The transcript states that SOCK_STREAM creates a socket which is "reliable and connection oriented" — it is a TCP kind of socket. SOCK_DGRAM is unreliable and connectionless (UDP).

---

### Q8. In a UDP server, which of the following system calls is NOT required?

(A) socket()  
(B) bind()  
(C) accept()  
(D) recvfrom()  

**✅ Answer: (C)**  
**Explanation:** The transcript explicitly states: "for the UDP case, we do not create any connection. So, we do not require this connect and accept calls." The `accept()` call is specific to TCP because it requires connection establishment. UDP just binds and directly sends/receives.

---

### Q9. Why does the accept() system call return a new socket ID?

(A) Because the old socket has expired  
(B) Because each client connection needs a separate logical pipe  
(C) Because the server port changes after each connection  
(D) Because UDP requires a new socket for each message  

**✅ Answer: (B)**  
**Explanation:** The transcript explains that when multiple clients connect simultaneously, you need to create separate logical pipes to separate clients. The `accept()` call returns a new socket ID for each client so that data can be sent to the correct client through the correct pipe.

---

### Q10. When running a UDP client multiple times, what happens to the client's port number?

(A) It stays the same every time  
(B) It is always 0  
(C) It changes randomly with each run  
(D) It is always the same as the server's port  

**✅ Answer: (C)**  
**Explanation:** The transcript demonstrates in the live demo that "at the client side as the client do not bind itself to a well known port during the runtime, the client randomly chooses one port address." Running the client multiple times shows different port numbers each time.

---

*📌 End of Lecture 24 — SOCKET PROGRAMMING – I*  
*Next lecture covers: TCP Server and TCP Client demo with variants.*
