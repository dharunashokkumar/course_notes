# Lecture 25: Socket Programming – II (TCP Socket Programming)

**Course:** Computer Networks and Internet Protocol  
**Instructor:** Prof. Sandip Chakraborty, IIT Kharagpur  
**Topic:** TCP Server & Client Implementation, Iterative Server, Concurrent Server (fork), Select System Call

---

## Recap from Previous Lecture

In Lecture 24, we studied **UDP socket programming** — how to create a UDP server and a UDP client using **datagram sockets (SOCK_DGRAM)**. We saw how data is sent and received using the UDP protocol.

In **this lecture (Lecture 25)**, we move to **TCP socket programming**. We will learn how to do the same thing using the **TCP protocol** and also explore **different variants of TCP servers**: Iterative Server, Concurrent Server using fork(), and Server using the select() system call.

---

## Concept 1: TCP Server Code — Creating the Socket

### 📌 Concept Name
**TCP Server Socket Creation**

### 🧠 Simple Explanation
Just like UDP, the first step for a TCP server is to create a socket. But there is one key difference: instead of using `SOCK_DGRAM` (which was for UDP), we now use **`SOCK_STREAM`** (which is for TCP).

The socket() system call looks like this:

```
socket(AF_INET, SOCK_STREAM, 0)
```

Here:
- **AF_INET** → Protocol family for IPv4
- **SOCK_STREAM** → Socket type for TCP (connection-oriented, reliable stream)
- **0** → Protocol field (usually set to 0, meaning the system picks the default protocol for this socket type)

The server also declares a variable of type `struct sockaddr_in` to hold the server address, and a file descriptor variable to store the socket ID that gets created.

The port number is taken from the **command line argument** when running the server program.

### 🎯 Exam Important Points
- TCP uses **SOCK_STREAM**, UDP uses **SOCK_DGRAM** — this is a very common exam question
- `AF_INET` is used for IPv4 addressing
- The protocol field is typically set to **0**
- The socket() call returns a **file descriptor** (a number) that identifies this socket

### ⚠️ Common Confusions
- Students confuse SOCK_STREAM (TCP) with SOCK_DGRAM (UDP). Remember: **Stream = TCP, Datagram = UDP**
- The socket is not yet connected to anything at this point — it is just created

---

## Concept 2: Binding the Socket — bind() System Call

### 📌 Concept Name
**Server Address Initialization and bind() Call**

### 🧠 Simple Explanation
After creating the socket, we need to tell the server **which address and port** it should listen on. We do this by:

1. **Initializing the server address structure:**
   - `sin_family = AF_INET` → We are using IPv4
   - `sin_addr = INADDR_ANY` → This means the server will accept connections on **any network interface** of the local machine (localhost)
   - `sin_port = port number` → The port number taken from the command line

2. **Calling bind():**
   - The bind() system call **attaches** the socket to the server address and port
   - Format: `bind(socket_fd, server_address, size_of_address)`

If the bind fails (for example, if the port is already being used by another application), we get an error message.

### 🎯 Exam Important Points
- **INADDR_ANY** allows the server to accept connections on any local network interface
- bind() can fail if the **port number is already in use** by another application
- bind() takes three parameters: socket file descriptor, address pointer, and address size

### ⚠️ Common Confusions
- INADDR_ANY does NOT mean any remote address — it means the server binds to **all available local addresses**
- bind() is done on the **server side**, not the client side

---

## Concept 3: listen() System Call — Preparing to Accept Connections

### 📌 Concept Name
**The listen() Call and Backlog Concept**

### 🧠 Simple Explanation
After binding, the server must **announce** that it is ready to accept incoming connections. This is done using the **listen()** call.

```
listen(socket_fd, 5)
```

Here:
- `socket_fd` → The socket file descriptor
- `5` → The **maximum number of connections that can be backlogged** (waiting in queue)

**What is backlog?** The server can handle only **one client at a time** (in the basic iterative model). While the server is busy with one client, other clients may also try to connect. These waiting connections are stored in a **backlog queue**. The number 5 means up to 5 connections can wait in this queue.

Think of it like a restaurant: one customer is being served, and 5 others can wait in line. Any more than that get turned away.

### 🎯 Exam Important Points
- listen() sets a **flag** that the socket is now in **listening state**
- The backlog parameter defines how many connections can **wait in the queue**
- listen() does NOT accept any connection — it only prepares the socket for incoming connections

### ⚠️ Common Confusions
- listen() does NOT establish a connection — it just makes the socket **ready to listen**
- The backlog number is NOT the total number of clients the server can ever handle — it is only the number that can **wait in line at one time**

---

## Concept 4: setsockopt() — Reusing the Port Address

### 📌 Concept Name
**SO_REUSEADDR Option**

### 🧠 Simple Explanation
After the listen() call, the server makes a call to `setsockopt()` function with the **SO_REUSEADDR** option. This allows the server to **reuse the same port number** even if it was recently used by another process.

Without this option, if you stop and quickly restart the server, you might get a "port already in use" error because the OS still holds that port for a short time. `SO_REUSEADDR` prevents this problem.

### 🎯 Exam Important Points
- `setsockopt()` with `SO_REUSEADDR` allows **reuse of the same port number**
- This is a common practice in server programming

---

## Concept 5: accept() System Call — Accepting Client Connections

### 📌 Concept Name
**The accept() Blocking Call**

### 🧠 Simple Explanation
Now the server enters a **while loop** and calls `accept()`. This is the most important step for TCP.

```
new_fd = accept(socket_fd, client_address, client_address_length)
```

Key facts about accept():
1. **accept() is a BLOCKING call** — the server will **wait here** until a client makes a connect() call
2. When a client connects, accept() returns a **NEW file descriptor** (new_fd). This new file descriptor represents the specific connection between this server and that particular client
3. The original socket_fd remains available for accepting more connections
4. accept() also fills in the **client's address and port** information

After accepting, the server prints a message showing which client IP and port the connection came from.

### 🛠 Real-world Example (from transcript)
When the demo was run, the server showed messages like:
- "Received new connection from 127.0.0.1 at port 47676"
- Then another connection at port 47678
- Then another at port 47680

This shows that **each time the client connects**, it uses a **different random port number**, while the server stays on its **fixed well-known port** (like 2444 in the demo).

### 🎯 Exam Important Points
- accept() is a **blocking call** — the server waits until a client connects
- accept() returns a **NEW socket file descriptor** that is specific to that client connection
- The original socket continues to listen for new connections
- TCP uses a **three-way handshake** (connect from client + accept from server initiates this)
- Client uses **random/ephemeral ports**, server uses a **well-known fixed port**

### ⚠️ Common Confusions
- accept() creates a NEW file descriptor — the original socket file descriptor is NOT used for data transfer
- accept() is only on the **server side**; the client uses **connect()** instead

---

## Concept 6: Sending and Receiving Data — recv() and send()

### 📌 Concept Name
**Data Transfer in TCP using recv() and send()**

### 🧠 Simple Explanation
After the connection is accepted, the server uses the **new file descriptor** (returned by accept) to send and receive data:

- **recv(new_fd, buffer, buffer_length, flags)** → Reads data from the client into the buffer. Returns the number of bytes received. If it returns **0**, it means the client has closed the connection.
- **send(new_fd, buffer, data_length, flags)** → Sends data back to the client.

In the demo, the server **echoes back** whatever data it receives from the client. So if the client sends "hello there", the server receives it and sends the same "hello there" back to the client.

When `recv()` returns 0 (no more data), the server **closes** that particular connection using `close()`.

### 🎯 Exam Important Points
- Data transfer uses the **new file descriptor** from accept(), NOT the original server socket
- recv() returning **0** means the client has disconnected
- In the demo, the server acts as an **echo server** — it sends back whatever it receives

### ⚠️ Common Confusions
- Do NOT use the original socket fd for send/recv — you must use the **new fd** returned by accept()

---

## Concept 7: TCP Client Implementation

### 📌 Concept Name
**TCP Client — socket(), connect(), send(), recv()**

### 🧠 Simple Explanation
The client side follows these steps:

**Step 1: Create a Socket**
Same as the server — `socket(AF_INET, SOCK_STREAM, 0)`

**Step 2: Set Up Server Address**
- Get the server **hostname** and **port** from the command line
- Use `gethostbyname()` function to convert hostname to an IP address
- Fill in the `serverAddr` structure with: AF_INET, the server's IP address, and the port number

**Step 3: Connect to Server**
```
connect(socket_fd, server_address, address_size)
```
The `connect()` call initiates the **TCP three-way handshake** with the server. Once the handshake is successful, the connection is established.

**Step 4: Send Data**
```
send(socket_fd, "hello there", message_length, flags)
```
The client sends the message "hello there" to the server.

**Step 5: Receive Data**
```
recv(socket_fd, buffer, buffer_length, flags)
```
The client receives the echoed message back from the server and prints it.

### 🛠 Real-world Example (from transcript)
In the demo:
- Server was started on port 2444
- Client was run with `./client localhost 2444`
- The client sent "hello there", server echoed it back, and client printed the received message

### 🎯 Exam Important Points
- Client uses **connect()** to initiate TCP three-way handshake; Server uses **accept()** to accept it
- `gethostbyname()` converts a hostname to an IP address
- The client does NOT need to call bind() — the OS assigns a **random ephemeral port** automatically
- The client does NOT call listen() or accept()

### ⚠️ Common Confusions
- Server flow: socket → bind → listen → accept → recv/send
- Client flow: socket → connect → send → recv
- The client does NOT bind to a specific port — the OS assigns one randomly

---

## Concept 8: Iterative Server — What It Is and Why It Has Problems

### 📌 Concept Name
**Iterative Server Implementation**

### 🧠 Simple Explanation
The server implementation we saw above is called an **Iterative Server**. Here is why:

In the server code, there is a **while loop**:
```
while(true) {
    new_fd = accept(...)     // Wait for a client
    while(data) {
        recv(new_fd, ...)    // Receive data from this client
        send(new_fd, ...)    // Send data back to this client
    }
    close(new_fd)            // Close this client's connection
    // Only NOW can the next client be accepted
}
```

The server handles clients **one by one** (iteratively). While it is busy sending/receiving data with Client 1, it **cannot** accept Client 2. Client 2 must **wait in the backlog queue**.

**How Iterative Server Works (step-by-step):**
1. The listen() call sets the socket to listening state and sets the maximum backlog
2. The accept() call **blocks** until a new connection arrives in the queue
3. Once accepted, a new socket file descriptor (connfd) is returned for data transfer
4. **All other connections that arrive during this time are backlogged** in the connection queue
5. Only after the current connection is fully handled, the server loops back to accept the next connection

### 🛠 Real-world Example (from transcript)
Think of a web server receiving **thousands of requests per second**. If it handles them one by one, it will be extremely slow. This is why iterative servers are **not useful** for high-traffic scenarios like web servers.

### 🎯 Exam Important Points
- Iterative server handles **one client at a time**
- Other clients must **wait in the backlog queue**
- It is **inefficient** for scenarios with many simultaneous clients
- The accept() call is a **blocking call**
- The server is blocked on send/recv until the current client is done

### ⚠️ Common Confusions
- The server is NOT blocked on accept() when serving a client — it is blocked on the **send/recv loop**. It cannot go back to accept() until the current client's communication is finished.

---

## Concept 9: Concurrent Server Using fork() — Parallel Processing

### 📌 Concept Name
**Concurrent (Parallel) Server Implementation Using fork() System Call**

### 🧠 Simple Explanation
To solve the iterative server's problem, we create a **concurrent server** that handles multiple clients **in parallel**. The idea is:

> **Do not wait for send/recv to finish before accepting the next connection. Instead, create a child process to handle each connection.**

**How it works:**

1. The server calls `accept()` as before, getting a new file descriptor for the client connection
2. Immediately after accept, the server calls **`fork()`**
3. `fork()` creates a **child process** (a copy of the parent process)
4. In the **child process**, `fork()` returns **0**
5. In the **parent process**, `fork()` returns the **child's process ID** (a non-zero number)

**After the fork:**
- **Child process (fork returns 0):** Closes the original server socket (it doesn't need it). Uses the new file descriptor to handle send/recv with the client. This runs **in parallel**.
- **Parent process (fork returns child ID):** Does NOT enter the send/recv loop. Immediately goes back to the `accept()` call to accept the **next client connection**.

This way, the parent process is **never blocked** by send/recv operations. Each client gets its own child process to handle the communication.

### 🛠 Real-world Example (from transcript)
In the demo:
- Server was started on port 2555 (named "forkserver")
- Multiple clients connected from different terminal tabs simultaneously
- Each connection was handled by a **separate child process**
- The server showed connections from different ports, all handled in parallel

### 🎯 Exam Important Points
- fork() creates a **child process** — a copy of the parent process
- In child process: fork() returns **0**
- In parent process: fork() returns the **child process ID** (non-zero)
- The child handles the data transfer (send/recv)
- The parent immediately goes back to accept() for the next connection
- The child **closes the original server socket** since it only needs the new connection socket
- The client code remains **unchanged** — only the server changes

### ⚠️ Common Confusions
- fork() does NOT create a new socket — it creates a new **process**
- The child process inherits the new file descriptor from the parent
- The parent does NOT handle data — only the child does
- **Zombie process problem:** If the parent process dies or stops, child processes can become **zombies** (processes that are finished but not cleaned up)

---

## Concept 10: Problem with fork() — Zombie Processes

### 📌 Concept Name
**Zombie Processes in Concurrent Server**

### 🧠 Simple Explanation
While the fork()-based concurrent server solves the parallelism problem, it introduces a new issue: **zombie processes**.

In operating systems, when a parent process creates child processes using fork(), if the parent gets killed or stops, those child processes can become **zombies** — they are dead but their entries remain in the system's process table. This wastes system resources.

This is one of the motivations for looking at an alternative approach — the **select() system call**.

### 🎯 Exam Important Points
- fork()-based servers can create **zombie processes**
- Zombies are child processes that have finished but are not cleaned up by the parent
- This is a resource management concern

---

## Concept 11: The Need for Select — Peer-to-Peer Chat Scenario

### 📌 Concept Name
**Handling Multiple File Descriptors — The Chat Application Problem**

### 🧠 Simple Explanation
Consider a **peer-to-peer chat application** where multiple people send and receive messages to each other. There is **no central server** — each user runs their own TCP server for incoming messages.

Now here is the problem: In UNIX, **everything is a file**. A socket connection is a file descriptor, and the **keyboard input (standard input / STDIN)** is also a file descriptor.

When you are chatting:
- You receive messages from the **socket** (file descriptor for network)
- You type messages from the **keyboard** (STDIN file descriptor)

These two events are **asynchronous** — a message can arrive on the socket while you are typing on the keyboard. The question is: how do you switch between multiple file descriptors when inputs are coming from multiple places at the same time?

### 🎯 Exam Important Points
- In UNIX, **everything is a file** — sockets, keyboard input (STDIN), all are file descriptors
- A chat application needs to handle **multiple file descriptors simultaneously**
- The input from these file descriptors is **asynchronous** — events can happen at any time

### ⚠️ Common Confusions
- This is NOT about having multiple clients — it is about a **single process** needing to monitor **multiple file descriptors** at the same time

---

## Concept 12: The select() System Call — I/O Multiplexing

### 📌 Concept Name
**select() — Synchronous I/O Multiplexing Over Asynchronous Inputs**

### 🧠 Simple Explanation
The `select()` system call is an operating system call that lets a single process **monitor multiple file descriptors** at the same time. It acts as a **multiplexer** — out of many file descriptors, it finds which one is currently active (has data ready).

**Advantages of select() over fork():**
1. You do NOT need to create multiple child processes
2. No risk of **zombie processes**
3. Resources are managed more efficiently
4. You can handle multiple connections from a **single process**

**The select() function signature:**
```
int select(int nfds, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *timeout);
```

**Parameters:**
- `nfds` → Highest file descriptor number in any of the three sets, **plus 1**
- `readfds` → Set of file descriptors to watch for **reading** (incoming data)
- `writefds` → Set of file descriptors to watch for **writing** (output space available)
- `exceptfds` → Set of file descriptors to watch for **exceptions**
- `timeout` → How long to wait before giving up (in seconds and microseconds)

**Key idea:** select() provides **synchronous I/O multiplexing** over **asynchronous inputs**. The inputs can arrive at any time (asynchronous), but select() lets you pick one at a time (synchronous multiplexing) — either the keyboard or the socket, whichever is ready first.

### 🎯 Exam Important Points
- select() is a **multiplexer** for file descriptors
- It monitors three types of file descriptors: **read, write, and exception**
- `nfds` = highest file descriptor number + 1
- File descriptors use a structure called **fd_set** (a bitmap)
- select() provides **synchronous I/O multiplexing over asynchronous inputs**
- No child processes are created — everything runs in a **single process**

### ⚠️ Common Confusions
- select() does NOT create parallel processes like fork() — it handles everything in **one process** by switching between file descriptors
- The three fd_sets (read, write, except) are **separate** — you can use any combination

---

## Concept 13: How select() Works Internally

### 📌 Concept Name
**Internal Working of select()**

### 🧠 Simple Explanation
Here is what happens when select() is called:

**Step 1: Polling**
select() loops over **all the file descriptors** in the sets. For each file descriptor, it calls the file descriptor's **poll method** to check if any event is waiting (is data available to read? is space available to write? is there an exception?).

**Step 2: Check for Matches**
If **any** file descriptor matches the condition the user was looking for (read, write, or exception), select() **returns immediately** after updating the appropriate fd_set.

**Step 3: Sleep if Nothing Ready**
If **no** file descriptor is ready, select() goes to **sleep** for the timeout duration. During this sleep:
- If an interesting event happens on any file descriptor, that file descriptor will **notify its wait queue**, which wakes up the thread sleeping inside select()
- select() then repeats the loop to see which file descriptors are now ready

**Step 4: Timeout**
If the timeout expires and still no event occurred, select() returns with a 0 value.

### 🎯 Exam Important Points
- select() uses **polling** to check each file descriptor
- If a matching event is found → returns **immediately**
- If no event → **sleeps** until timeout or an event occurs
- File descriptors **notify** the wait queue when an event happens

---

## Concept 14: select() Blocking Behavior and Timeout

### 📌 Concept Name
**Blocking Behavior and Timeout of select()**

### 🧠 Simple Explanation
The select() call **remains blocked** (waits) until one of these three things happens:

1. **A file descriptor becomes ready** — some data is available to read, space is available to write, or an exception occurred
2. **A signal handler interrupts the call** — some external signal wakes it up
3. **The timeout expires** — the specified time duration has passed with no events

The timeout is specified using a structure with **seconds** and **microseconds** fields.

If you do NOT provide a timeout (set it to NULL), select() will wait **indefinitely** until a file descriptor becomes ready.

### 🎯 Exam Important Points
- select() blocks until: a file descriptor is ready, a signal interrupts it, OR the timeout expires
- Timeout is in **seconds and microseconds**
- NULL timeout = **infinite waiting**

---

## Concept 15: fd_set — File Descriptor Set Operations

### 📌 Concept Name
**fd_set Bitmap and FD_ZERO, FD_SET, FD_ISSET Macros**

### 🧠 Simple Explanation
The file descriptors are passed to select() using a special structure called **fd_set**, which is essentially a **bitmap** of fixed size. Each bit represents one file descriptor.

There are important macros to work with fd_set:

1. **FD_ZERO(&fdset)** → Initializes the fd_set to zero (clears all bits). You must do this first.
2. **FD_SET(fd, &fdset)** → Adds a file descriptor to the set (sets the corresponding bit to 1). This means "I want to watch this file descriptor."
3. **FD_ISSET(fd, &fdset)** → After select() returns, this checks whether a specific file descriptor is "set" (ready). If it returns true, data is available on that file descriptor.

### 🎯 Exam Important Points
- fd_set is a **bitmap** — each bit represents one file descriptor
- Always call **FD_ZERO** first to initialize
- Use **FD_SET** to add file descriptors you want to monitor
- Use **FD_ISSET** after select() returns to test which file descriptors are ready
- These are essential macros for working with select()

---

## Concept 16: Return Values of select()

### 📌 Concept Name
**select() Return Values**

### 🧠 Simple Explanation
After select() finishes, it returns a value that tells you what happened:

| Return Value | Meaning |
|---|---|
| **-1** | An **error** occurred |
| **0** | **Timeout** happened — no file descriptor became ready in the given time |
| **> 0** | The **number of file descriptors** that have events pending (ready for read, write, or exception) |

### 🎯 Exam Important Points
- Return -1 → Error
- Return 0 → Timeout (nothing happened)
- Return > 0 → Number of sockets/file descriptors with pending events

---

## Concept 17: TCP Server Using select() — Complete Flow

### 📌 Concept Name
**TCP Server Implementation with select()**

### 🧠 Simple Explanation
Now let us see how to build a TCP server using select() instead of fork().

**The setup** is the same as before: create socket, bind, listen, setsockopt.

**What is different:**
1. We declare a set of file descriptors (up to 16 in the demo) and use fd_set
2. We set the maximum file descriptor = the current socket fd
3. Inside the while loop:

**Step 1:** Initialize the fd_set using FD_ZERO and add the server socket using FD_SET

**Step 2:** Loop over all active file descriptors and add them to the fd_set

**Step 3:** Call select() — in the demo, no timeout is given, so it waits **indefinitely** until an event occurs

**Step 4:** When select() returns, check which file descriptor is ready using FD_ISSET:
- **If it is the server's original socket that is ready** → A **new connection** has arrived. Call `accept()` to accept it and **add the new file descriptor** to the list of file descriptors we are monitoring
- **If it is one of the existing client file descriptors** → Data is available. Call `recv()` to read the data and `send()` to echo it back

**Step 5:** If recv() returns 0 for a file descriptor → the client has disconnected. Close that file descriptor and **remove it from the fd_set**.

### 🛠 Real-world Example (from transcript)
In the demo:
- Server was run on port 2666 using select()
- The client implementation remained the **same** (no changes to client code)
- Multiple clients connected and data was handled correctly
- When a task was done, the connection was closed automatically

### 🎯 Exam Important Points
- The **client code does NOT change** when switching between iterative, fork, or select server — only the server changes
- select() allows handling multiple connections in a **single process** without fork()
- New connections are **added** to the fd_set; closed connections are **removed** from the fd_set
- No timeout in the demo → select() waits **indefinitely** for events

### ⚠️ Common Confusions
- select() does NOT create new processes — it multiplexes in a **single process**
- The server's original socket is also in the fd_set — when it is "ready", it means a **new connection** is waiting
- Existing client sockets being "ready" means **data has arrived** from that client

---

## Concept 18: Connection Refused Error

### 📌 Concept Name
**Connection Refused When No Server is Running**

### 🧠 Simple Explanation
In the demo, the professor showed what happens when a client tries to connect to a port **where no server is running**.

For example, the server was running on port 2666, but the client tried to connect to port 2555. The client received a **"Connection Refused"** error message from the connect() call.

This happens because there is no server listening on that port, so the TCP handshake cannot be completed.

### 🎯 Exam Important Points
- If a client tries to connect to a port with **no server running**, the connect() call returns a **"Connection Refused"** error
- This is a common networking error to understand

---

## Concept 19: Comparison of Three Server Types

### 📌 Concept Name
**Iterative vs Concurrent (fork) vs Select Server**

### 🧠 Simple Explanation

| Feature | Iterative Server | Concurrent Server (fork) | Select-based Server |
|---|---|---|---|
| **How it handles clients** | One at a time | One child process per client | Single process, multiplexing |
| **Parallelism** | None — sequential | Yes — using child processes | Yes — using I/O multiplexing |
| **Blocking problem** | Blocked on send/recv for current client | Not blocked — child handles data | Not blocked — select() monitors all fds |
| **Child processes** | None | Yes — one per connection | None |
| **Zombie risk** | No | Yes — zombie processes possible | No |
| **Resource efficiency** | Low for many clients | Moderate — process overhead | High — no process overhead |
| **Client code changes?** | No | No | No |
| **Use case** | Simple, low-traffic servers | Medium-traffic servers | High-efficiency servers, chat apps |

### 🎯 Exam Important Points
- The **client implementation remains the same** for all three server types
- Iterative = one-by-one, fork = parallel via processes, select = parallel via multiplexing
- select() is the most resource-efficient (no child processes, no zombies)

---

## Concept 20: TCP vs UDP Socket Programming Summary (From Lecture 24 + 25)

### 📌 Concept Name
**Key Differences Between TCP and UDP Sockets**

### 🧠 Simple Explanation

| Feature | UDP Socket | TCP Socket |
|---|---|---|
| **Socket Type** | SOCK_DGRAM | SOCK_STREAM |
| **Connection** | Connectionless | Connection-oriented |
| **Server calls** | socket, bind, recvfrom, sendto | socket, bind, listen, accept, recv, send |
| **Client calls** | socket, sendto, recvfrom | socket, connect, send, recv |
| **Handshake** | None | TCP three-way handshake (via connect + accept) |
| **accept() needed?** | No | Yes |
| **listen() needed?** | No | Yes |
| **connect() needed?** | No (optional) | Yes (mandatory on client) |
| **Data transfer** | sendto/recvfrom | send/recv |

### 🎯 Exam Important Points
- TCP requires listen(), accept(), and connect() — UDP does NOT
- TCP uses send()/recv() — UDP uses sendto()/recvfrom()
- TCP server gets a **new file descriptor** from accept() — UDP server uses the **same socket** for all clients
- The server announces itself to a **well-known port**; the client uses a **random ephemeral port**

---

## Summary Table of All System Calls Covered

| System Call | Used By | Purpose |
|---|---|---|
| **socket()** | Server & Client | Creates a new socket |
| **bind()** | Server | Binds socket to address and port |
| **listen()** | Server | Marks socket as listening; sets backlog |
| **accept()** | Server | Accepts incoming connection; returns new fd (blocking call) |
| **connect()** | Client | Initiates TCP three-way handshake with server |
| **send()** | Server & Client | Sends data over TCP connection |
| **recv()** | Server & Client | Receives data over TCP connection |
| **close()** | Server & Client | Closes a file descriptor / connection |
| **fork()** | Server (concurrent) | Creates a child process |
| **select()** | Server (multiplexing) | Monitors multiple file descriptors simultaneously |
| **setsockopt()** | Server | Sets socket options like SO_REUSEADDR |
| **gethostbyname()** | Client | Converts hostname to IP address |
| **FD_ZERO()** | Server (select) | Clears the fd_set bitmap |
| **FD_SET()** | Server (select) | Adds a fd to the set |
| **FD_ISSET()** | Server (select) | Checks if a fd is ready after select() |

---

# 10 MCQs — Lecture 25: Socket Programming – II

---

### Q1. What socket type is used for TCP socket programming?

A) SOCK_DGRAM  
B) SOCK_STREAM  
C) SOCK_RAW  
D) SOCK_SEQ  

**Answer: B) SOCK_STREAM**

**Explanation:** TCP is a stream-oriented, connection-oriented protocol. It uses SOCK_STREAM. SOCK_DGRAM is used for UDP (datagram-based, connectionless).

---

### Q2. In a TCP server, the accept() call is:

A) A non-blocking call that returns immediately  
B) A blocking call that waits until a client connects  
C) A call that sends data to the client  
D) A call that creates the socket  

**Answer: B) A blocking call that waits until a client connects**

**Explanation:** As per the transcript, accept() is a blocking call. The server keeps waiting at accept() until a client makes a connect() call. Once a client connects, accept() returns a new file descriptor for that connection.

---

### Q3. In a concurrent TCP server using fork(), what does the fork() system call return in the child process?

A) The parent process ID  
B) -1  
C) 0  
D) The child process ID  

**Answer: C) 0**

**Explanation:** The transcript clearly states: "In the child process, the fork call will return 0 and in the parent process the fork call will return the ID of the child process." So the child process gets a return value of 0.

---

### Q4. What is the backlog parameter in the listen() call?

A) The total number of clients the server can serve in its lifetime  
B) The maximum number of connections that can wait in the queue while the server is busy  
C) The maximum data size the server can receive  
D) The timeout for the connection  

**Answer: B) The maximum number of connections that can wait in the queue while the server is busy**

**Explanation:** The backlog parameter specifies how many connections can be queued (waiting) while the server is busy handling another connection. In the demo, it was set to 5.

---

### Q5. What problem does the concurrent server using fork() introduce?

A) It cannot handle multiple clients  
B) It creates zombie processes when the parent process dies  
C) It uses SOCK_DGRAM instead of SOCK_STREAM  
D) It requires changes to the client code  

**Answer: B) It creates zombie processes when the parent process dies**

**Explanation:** The transcript mentions that child processes can become zombies if the parent process gets killed or stops. This is a known problem with fork()-based servers and is one reason to consider the select() approach.

---

### Q6. The select() system call provides:

A) Asynchronous I/O multiplexing over synchronous inputs  
B) Synchronous I/O multiplexing over asynchronous inputs  
C) Parallel processing using multiple child processes  
D) Connectionless data transfer  

**Answer: B) Synchronous I/O multiplexing over asynchronous inputs**

**Explanation:** The transcript explicitly states: "It is actually providing you a synchronous I/O multiplexing over asynchronous input." The inputs arrive asynchronously, but select() provides a synchronous way to multiplex between them.

---

### Q7. What does the select() system call return when the timeout expires without any file descriptor becoming ready?

A) -1  
B) 0  
C) 1  
D) The number of file descriptors  

**Answer: B) 0**

**Explanation:** As per the transcript: "0 means that the timeout has happened." A return of -1 means error, and greater than 0 means the number of sockets with events pending.

---

### Q8. In the select() system call, the first parameter (nfds) is:

A) The total number of file descriptors in the system  
B) The highest numbered file descriptor in any of the three sets, plus 1  
C) The number of read file descriptors only  
D) Always set to 0  

**Answer: B) The highest numbered file descriptor in any of the three sets, plus 1**

**Explanation:** The transcript states: "In the select system call you are providing the number of file descriptor, it is the highest number of file descriptor in any of the three sets plus 1."

---

### Q9. In a TCP client program, which system call initiates the three-way handshake with the server?

A) bind()  
B) listen()  
C) accept()  
D) connect()  

**Answer: D) connect()**

**Explanation:** The transcript states: "It initializes the connection to the server using the TCP three way handshaking procedure" when describing the connect() call. The client calls connect(), and the server calls accept() to complete the handshake.

---

### Q10. When a client tries to connect to a port where no server is running, what happens?

A) The connection is silently dropped  
B) The client receives a "Connection Refused" error  
C) The client waits indefinitely  
D) The client connects but receives no data  

**Answer: B) The client receives a "Connection Refused" error**

**Explanation:** The transcript describes this demo scenario: "If you try to connect to a port, where the server is not running... it will get a connection refuse message from the connect call, because none of the server is currently running on that port."

---

*End of Lecture 25 Study Guide*
