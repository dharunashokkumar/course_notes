# Lecture 8: Application Layer — IV (HTTP, HTML, TELNET)

**Course:** Computer Networks and Internet Protocol  
**Instructor:** Prof. Soumya Kanti Ghosh, IIT Kharagpur

---

## What This Lecture Covers

This lecture focuses on **HTTP (Hypertext Transfer Protocol)** in detail. The professor also briefly mentions HTML and TELNET but the main teaching in this lecture is about HTTP — how it works, its request-response structure, methods, status codes, headers, and examples.

---

## Concept 1: Quick Recap — Client-Server Model and the 5-Tuple

### Simple Explanation

Before jumping into HTTP, the professor reminds us about the **client-server model** discussed in earlier lectures. In this model, a **server program** runs on a system and listens on a **particular port**. A **client** from the same system or another system sends a request to that server.

Every connection on the internet is uniquely identified by **five things** (called a **5-tuple**):

1. **Server IP address** — Where the server machine is located
2. **Server port number** — Which door on the server to knock
3. **Client IP address** — Where the client machine is located
4. **Client port number** — Which door on the client side is used
5. **Protocol ID** — Which protocol is being used (e.g., TCP, UDP)

If **any one** of these five values is different, it creates a **unique, separate connection**. That is why when you open multiple tabs in a browser, each tab's request goes to the correct tab — they all have different client port numbers.

### Exam Important Points

- The 5-tuple uniquely identifies each connection.
- Even if you open many pages from the same browser to the same server, each connection is unique because the client port will differ.

### Common Confusions

- Students often think only IP addresses identify a connection. Remember: **all five elements together** make it unique.

---

## Concept 2: Connection-Oriented vs Connectionless Communication

### Simple Explanation

Some communication in networking is **connection-oriented** (like TCP) — a proper connection is set up before data flows. Some communication is **connectionless** (like UDP) — data is just sent without a formal connection.

The professor highlights the beauty of **layering**: each layer only talks to its **peer layer** on the other side. It does not worry about what happens in the layers below or above. If a lower layer does not support reliability, the upper layer can add its own mechanism.

### Exam Important Points

- TCP = connection-oriented, UDP = connectionless.
- Each layer communicates only with its peer at the same layer on the other side.
- Reliability can be handled at different layers depending on need.

---

## Concept 3: HTTP — Hypertext Transfer Protocol (Overview)

### Simple Explanation

**HTTP** is the protocol that allows **web documents** to be communicated over the network. It is the **foundation of the World Wide Web (WWW)**.

Think of it this way: when you type a website address in your browser and press Enter, it is HTTP that carries your request to the server and brings the web page back to you.

**Key relationships:**
- **Web Server = HTTP Server** (the machine/program that hosts the website)
- **Web Browser = HTTP Client** (your browser — Chrome, Firefox, Internet Explorer, etc.)

The server and client can be on the same machine, the same network, or completely different networks across the world. The only requirement is that there must be some **connectivity** between them.

### Two Versions of HTTP

| Version | RFC Number | Key Feature |
|---------|-----------|-------------|
| HTTP 1.0 | RFC 1945 | Basic version, non-persistent connection by default |
| HTTP 1.1 | RFC 2616 | **Persistent connection by default** |

**Persistent connection** means: in HTTP 1.1, the connection between client and server stays open for multiple requests. In HTTP 1.0, a new connection was made for every single request.

### Exam Important Points

- HTTP is an **application-layer protocol**.
- HTTP is the basis of the World Wide Web (WWW).
- Web Server = HTTP Server; Web Browser = HTTP Client.
- HTTP 1.0 → RFC 1945; HTTP 1.1 → RFC 2616.
- HTTP 1.1 specifies **persistent connection by default**.

### Common Confusions

- HTTP 1.0 does NOT have persistent connection by default. Only HTTP 1.1 does.
- HTML is NOT a protocol — it is a **language**. HTTP is the protocol.

---

## Concept 4: HTTP Characteristics — Lightweight and Transport Independent

### Simple Explanation

HTTP is described as an **application-level protocol** with the **lightness and speed** necessary for distributed, hypermedia information systems.

Why does it need to be lightweight? Because:
- Data is stored on **distributed, heterogeneous systems** (different machines, different operating systems, different locations).
- There are **bandwidth constraints** on the network backbone.
- The data format must be **generic and easily parsable** so that any browser on any system can read it.

**Transport Independence:**
- HTTP **generally runs over TCP** (by default).
- However, the **HTTP protocol itself does NOT depend on any specific transport layer**. It does not say "TCP is mandatory."
- In practice, TCP is the predominant transport protocol used with HTTP.

### Exam Important Points

- HTTP is lightweight — designed for distributed hypermedia systems.
- HTTP is **transport independent** in theory, but practically uses **TCP**.
- The data format must be generic so heterogeneous systems can interoperate.

### Common Confusions

- "Transport independent" does NOT mean HTTP never uses TCP. It means HTTP's design does not force a specific transport protocol. In real use, TCP is the default.

---

## Concept 5: HTTP Request-Response Model

### Simple Explanation

HTTP has a very **simple structure**:
1. The **client sends a request**.
2. The **server sends back a reply**.

That's it! It is a basic **request-response** mechanism.

**Important details:**
- HTTP can support **multiple request-reply exchanges** over a **single TCP connection** (especially in HTTP 1.1).
- The **well-known port** for HTTP is **port 80**.
- If you do not specify any port in the URL, the browser automatically uses **port 80**.
- You CAN use other ports. For example, `xyz.com:7126` means you are telling the browser to connect to port 7126 instead of 80.

### Real-World Example (from transcript)

When you type `www.iitkgp.ac.in` in the browser:
1. The browser first converts the **domain name to an IP address** (using DNS).
2. Since no port is specified, it uses **port 80 by default**.
3. The request is sent, and the server responds with the web page.

If the server runs on a different port, you must specify it: `www.example.com:7126`

### Exam Important Points

- HTTP uses a **request-response** model.
- Default HTTP port = **80**.
- HTTP 1.1 supports multiple request-reply over a single TCP connection.
- If no port is specified in URL, port 80 is used by default.

### Common Confusions

- Port 80 is the default, but it is NOT the only port HTTP can use.

---

## Concept 6: Components of HTTP — Client, Server, URL, Cookies

### Simple Explanation

The professor identifies the **four main parties/components** in the HTTP ecosystem:

1. **Client (Browser)** — Sends the request.
2. **Server** — Responds to the request.
3. **URL (Uniform Resource Locator)** — The address of the resource you want to access.
4. **Cookies** — Small pieces of data used to **remember previous information** or help maintain a **session**.

The professor mentions that primarily what you need is: a **URL** for identifying the resource, a **browser** for making the request, and a **server** for responding.

### Exam Important Points

- Four components of HTTP communication: Client, Server, URL, Cookies.
- Cookies help in **session maintenance** and remembering previous data.

---

## Concept 7: HTTP Client (Browser) Architecture

### Simple Explanation

The browser (HTTP client) has a **controller** that supports different protocols:
- HTTP
- FTP
- TELNET
- SMTP
- and others

Depending on the type of request, the controller uses the appropriate protocol.

The browser can handle different types of content:
- **Static HTML pages** — simple, pre-made pages.
- **JavaScript** — brings dynamicity (interactive behavior) to the page.
- **Java programs/Applets** — programs that run inside the browser.
- **Client-side scripting** — things that get checked/processed at the browser end (e.g., validating that a roll number field contains only numbers).
- **Server-side scripting** — things that get processed at the server end (e.g., fetching student data from a database).

### Exam Important Points

- The browser's controller supports multiple protocols (HTTP, FTP, TELNET, SMTP).
- Client-side scripting handles things like input validation without contacting the server.
- Server-side scripting processes requests on the server and returns results.

---

## Concept 8: URL — Uniform Resource Locator

### Simple Explanation

The URL is the address you type in the browser. Its structure is:

```
Protocol://Host(or IP):Port/Path
```

**Example:** `http://www.example.com:8080/images/photo.jpg`

- **Protocol** — http, ftp, etc.
- **Host/IP** — The server's domain name or IP address.
- **Port** — The port number (optional if it's the default port 80 for HTTP).
- **Path** — The location of the resource on the server.

If the HTTP server is running on port 80 (default), you do NOT need to specify the port in the URL.

### Exam Important Points

- URL stands for **Uniform Resource Locator**.
- URL format: `Protocol://Host:Port/Path`
- Port is optional if default port is used (80 for HTTP).

---

## Concept 9: Types of Web Documents — Static, Dynamic, Active

### Simple Explanation

Web documents are grouped into **three broad categories**:

**1. Static Documents:**
- You request a page, and you get back a **fixed, pre-made page**.
- The content does not change based on the request.
- Example: A page that shows the same list of student names every time.

**2. Dynamic Documents:**
- You request something, some **program executes on the server**, and a page is **generated dynamically** based on your request.
- The content changes depending on what you ask for.
- Example: You send your roll number, and the server returns YOUR specific marks. If a different roll number is sent, a different page is returned.
- A popular technique for this is **CGI (Common Gateway Interface)**.
- Also achieved through **server-side scripting** — scripts inside the HTML document run on the server.

**3. Active Documents:**
- Processing happens **at the client side (browser)**.
- The server sends a program (like a **Java Applet** or **JavaScript**) that runs on YOUR browser.
- Example: JavaScript checks if the roll number you entered is numeric. If you type a letter, it immediately tells you "Invalid input" — without contacting the server.
- Active documents use **client-side scripting**.

### Summary Table

| Type | Where processing happens | Example |
|------|------------------------|---------|
| Static | No processing — pre-made page | A fixed information page |
| Dynamic | Server side (CGI, server-side scripts) | Sending roll number, getting marks back |
| Active | Client side (Java Applet, JavaScript) | Input validation in browser |

### Exam Important Points

- Three types: **Static, Dynamic, Active**.
- Dynamic uses **CGI (Common Gateway Interface)** or **server-side scripting**.
- Active uses **Java Applets** or **client-side scripting (JavaScript)**.
- Static = fixed page; Dynamic = server generates page; Active = client-side processing.

### Common Confusions

- **Dynamic ≠ Active**. Dynamic means the SERVER does the work. Active means the CLIENT (browser) does the work.
- CGI is specifically related to **dynamic** documents, not active ones.

---

## Concept 10: HTTP Transaction — Request, Response, and Statelessness

### Simple Explanation

An HTTP transaction is simple:
1. The **HTTP server** (also called **HTTPD — HTTP daemon**) runs on the server machine.
2. The **HTTP client** (browser) sends a request.
3. The server sends back a response.

**Every request gets a response.**

**Key point:** HTTP request-response is **stateless**. This means the server does **NOT remember** what happened in the previous request. Each request is treated independently.

If you want the server to remember something (like your login session), you need **additional mechanisms** (like cookies).

### Exam Important Points

- HTTPD = HTTP Daemon = the server process.
- HTTP is **stateless** — it does not remember previous interactions.
- Cookies and other mechanisms are needed to maintain state.

### Common Confusions

- Stateless does NOT mean the server cannot remember anything ever. It means HTTP itself does not have built-in memory. External tools like cookies provide this.

---

## Concept 11: HTTP Message Format — Request and Response

### Simple Explanation

**HTTP Request Message format:**

| Part | Description |
|------|-------------|
| **Request Line** | Contains: Request Method + URL + HTTP Version |
| **Header Lines** | Additional information (multiple lines) |
| **Blank Line** | Separates headers from body |
| **Body** | The actual data (may not always be present) |

**HTTP Response Message format:**

| Part | Description |
|------|-------------|
| **Status Line** | Contains: HTTP Version + Status Code + Status Phrase |
| **Header Lines** | Additional information (multiple lines) |
| **Blank Line** | Separates headers from body |
| **Body** | The actual content/document (may not always be present) |

### Request Line Structure

```
[Request Method]  [URL]  [HTTP Version]
Example: GET /images/photo.jpg HTTP/1.1
```

### Status Line Structure

```
[HTTP Version]  [Status Code]  [Status Phrase]
Example: HTTP/1.1 200 OK
```

### Exam Important Points

- Request message has: Request Line → Headers → Blank Line → Body.
- Response message has: Status Line → Headers → Blank Line → Body.
- Body may not always be present in either message.
- Request line = Method + URL + Version.
- Status line = Version + Status Code + Status Phrase.

---

## Concept 12: HTTP Methods

### Simple Explanation

HTTP defines several **methods** (also called actions). These tell the server what the client wants to do:

| Method | What It Does |
|--------|-------------|
| **GET** | Requests a document from the server |
| **HEAD** | Requests information ABOUT a document, but NOT the document itself |
| **POST** | Sends some information from the client TO the server |
| **PUT** | Sends a document from the server to the client (uploads/replaces a resource) |
| **TRACE** | Echoes the incoming request (used for debugging) |
| **CONNECT** | Reserved (for future use) |
| **OPTION** | Inquires about the available options on the server |

The professor says the **most popular/widely used** methods are: **GET, HEAD, POST, PUT**.

### Exam Important Points

- Know all 7 methods and their actions.
- GET = retrieve document; POST = send data to server; HEAD = get info about document only; PUT = upload/replace.
- Most widely used: GET, HEAD, POST, PUT.
- CONNECT is reserved.

### Common Confusions

- **GET vs POST:** GET retrieves data FROM server. POST sends data TO server.
- **GET vs HEAD:** GET gets the full document. HEAD gets only the information (headers) about the document, NOT the document body.

---

## Concept 13: HTTP Status Codes

### Simple Explanation

When the server sends a response, it includes a **status code** — a number that tells the client what happened with the request. These codes are grouped into **five series**:

### 1xx — Informational

| Code | Phrase | Meaning |
|------|--------|---------|
| 100 | Continue | Initial part of request received, client may continue |
| 101 | Switching | Server is switching protocols as requested by client |

### 2xx — Success

| Code | Phrase | Meaning |
|------|--------|---------|
| 200 | OK | Request is successful |
| 201 | Created | A new URL has been created |
| 202 | Accepted | Request accepted, but not immediately acted upon |
| 204 | No Content | There is no content in the body |

### 3xx — Redirection

| Code | Meaning |
|------|---------|
| 301 | Moved Permanently |
| 302 | Moved Temporarily |
| 304 | Not Modified |

### 4xx — Client Error

| Code | Phrase | Meaning |
|------|--------|---------|
| 400 | Bad Request | Syntax error in the request |
| 401 | Unauthorized | Request lacks proper authentication |
| 403 | Forbidden | Service is denied |
| 404 | Not Found | Document not found |
| 405 | Method Not Allowed | The method used is not allowed |
| 406 | Not Acceptable | Content not acceptable |

### 5xx — Server Error

| Code | Phrase | Meaning |
|------|--------|---------|
| 500 | Internal Server Error | Server encountered an error |
| 503 | Service Unavailable | Service temporarily unavailable, may be requested later |

### Quick Memory Aid

| Series | Category |
|--------|----------|
| 1xx | Informational |
| 2xx | Success |
| 3xx | Redirection |
| 4xx | Client Error |
| 5xx | Server Error |

### Exam Important Points

- **200 = OK** (most common success code).
- **404 = Not Found** (most commonly seen error by users).
- 4xx = Client-side errors; 5xx = Server-side errors.
- Know the series categories: 1xx=Informational, 2xx=Success, 3xx=Redirection, 4xx=Client Error, 5xx=Server Error.
- 204 = No Content (body is empty).

### Common Confusions

- 4xx errors are CLIENT errors (the problem is with the request). 5xx errors are SERVER errors (the problem is at the server side).
- 301 (Moved Permanently) vs 302 (Moved Temporarily) — both are redirection, but one is permanent and the other is temporary.

---

## Concept 14: HTTP Headers

### Simple Explanation

HTTP messages carry **headers** — lines of additional information. The format is:

```
Header-Name: Header-Value
```

There are **four categories** of headers discussed in the lecture:

### A. General Headers

| Header | Description |
|--------|-------------|
| Cache-control | Specifies information about caching |
| Connection | Shows whether the connection should be closed or not |
| Date | Shows the current date |
| MIME-version | Shows the MIME version used |
| Upgrade | Specifies the preferred communication protocol |

### B. Request Headers (sent by client)

| Header | Description |
|--------|-------------|
| Accept | Shows the medium/format the client can accept |
| Accept-charset | Shows the character set the client can handle |
| Accept-encoding | Shows the encoding scheme the client can handle |
| Accept-language | Shows the language the client can accept |
| Authorization | Authorization credentials |
| If-match | Sends the document only if it matches a given tag |
| If-range | Sends only the portion of the document that is missing |
| If-unmodified-since | Sends the document if not changed since specified date |
| Referrer | Specifies the URL of the linked document |
| User-agent | Identifies the client program |

### C. Response Headers (sent by server)

| Header | Description |
|--------|-------------|
| Accept-range | Shows if server accepts the range requested by client |
| Age | Shows the age of the document |
| Public | Shows the supported list of methods |
| Retry-after | Specifies the date after which the server is available |
| Server | Shows the server name and version number |

### D. Entity Headers (about the content/body)

| Header | Description |
|--------|-------------|
| Allow | Lists valid methods that can be used with a URL |
| Content-encoding | Specifies the encoding scheme |
| Content-language | Specifies the language |
| Content-length | Shows the length of the document |
| Content-range | Specifies the range of the document |
| Content-type | Specifies the media type |
| Etag | Gives an entity tag |
| Expires | Gives the date and time when contents may change |
| Last-modified | Gives the date and time of the last change |
| Location | Specifies the location of the created or moved document |

### Exam Important Points

- Headers are in the format: `Header-Name: value`
- Four categories: General, Request, Response, Entity.
- **Content-length** tells the length of the document.
- **Content-type** tells the media type.
- **Accept** tells what format the client can handle.
- **User-agent** identifies which browser/program is making the request.

---

## Concept 15: HTTP Example 1 — GET Method

### Simple Explanation

The professor walks through a **GET request example**:

**Client sends this request:**
```
GET /usr/bin/image1 HTTP/1.1
Accept: image/gif
Accept: image/jpeg
```

- The method is **GET** (retrieve a document).
- The URL path is `/usr/bin/image1`.
- The HTTP version is **1.1**.
- The **Accept headers** tell the server: "I can accept images in GIF or JPEG format."
- There is **no body** in this request.

**Server responds:**
```
HTTP/1.1 200 OK
Date: Mon, 07-Jan-05 13:12:14 GMT
Server: Challenger
MIME-version: 1.0
Content-length: 2048

(Body of the document)
```

- Status code **200** means the request was **successful (OK)**.
- The **Date** header shows when the response was sent.
- The **Server** header shows the server name (Challenger).
- The **MIME-version** is 1.0.
- The **Content-length** is 2048 (size of the body in bytes).
- The **body** contains the actual image data.

### Exam Important Points

- GET request may have headers but typically has **no body**.
- The response includes status line + headers + body.
- 200 OK = success.

---

## Concept 16: HTTP Example 2 — POST Method

### Simple Explanation

The professor shows a **POST request example**:

- The client wants to **send data to the server**.
- Method used is **POST**.
- The request line shows: POST, URL, and HTTP version 1.1.
- There are **four lines of headers**.
- The **request body contains the input information** (the data being sent).

**Server responds:**
- Status code **200 OK**.
- The response body contains a **CGI (Common Gateway Interface) document** — a dynamically generated page based on the input.

### Exam Important Points

- POST is used to **send data from client to server**.
- POST request HAS a body (unlike GET which typically does not).
- The response to a POST can be a CGI-generated dynamic document.

---

## Concept 17: HTTP and Timeouts

### Simple Explanation

The professor briefly mentions that HTTP has its own **timing issues**. When you request a page, there is a question of **how long should you wait** for the response.

HTTP has a **timeout mechanism** — if the server does not respond within a certain time, the connection may be dropped.

This is important because:
- Data is distributed across the network.
- Different data has different sizes (payload).
- Network conditions vary.

### Exam Important Points

- HTTP has timeout mechanisms.
- Timeout is needed because of varying network conditions and data sizes.

---

## Concept 18: Connecting to HTTP Server Using TELNET

### Simple Explanation

The professor demonstrates that you can connect to an HTTP server using **TELNET** (a remote login protocol). This shows that HTTP is based on simple text commands.

**Example from transcript:**
```
$ telnet www.mhhe.com 80
Trying 198.45.24.104...
Connected to www.mhhe.com (198.45.24.104)

GET /engcs/compsei/forouzan HTTP/1.1
From: forouzanbehrouz@fhda.edu

HTTP/1.1 200 OK
Date: Thu, 28 Oct 2004 16:27:46 GMT
Server: Apache/1.3.9 (Unix)
MIME-version: 1.0
Content-Type: text/html
```

**What is happening here:**
1. TELNET connects to the server (`www.mhhe.com`) at **port 80**.
2. Once connected, the user manually types an HTTP GET request.
3. The server responds with **200 OK** and the document.

This proves that **HTTP is text-based** — any program that can send text to port 80 can communicate with an HTTP server. TELNET was used here just to show this.

### Exam Important Points

- You can use TELNET to connect to an HTTP server on port 80.
- This works because HTTP is a **text-based protocol**.
- The server listening at port 80 responds to the HTTP request regardless of which client sent it.

---

## Concept 19: HTTP Summary (from the lecture)

The professor summarizes HTTP as follows:

- HTTP is a **Hypertext Transfer Protocol**.
- The **server listens** on a particular port (default 80).
- The **client knows** the server's IP and port, connects to it, and gets the result.
- There are different **status codes** for different situations (success, error, etc.).
- **404** is the most commonly seen error (document not found).
- HTTP can handle **different types of media**: text, images, video, voice — any hypermedia.
- Because the formatting follows a **generic, standard format**, any browser can parse and display the content.
- This is the **beauty of HTTP** — it enables access across the internet regardless of the system.

---

## Concept 20: Brief Mention of HTML and TELNET

The professor mentions that:
- **HTML** is NOT a protocol — it is a **language** (Hypertext Markup Language). It works together with HTTP.
- **TELNET** will be discussed in detail in a subsequent lecture. It is primarily used for **remote login**.
- The professor showed TELNET being used to connect to an HTTP server to demonstrate that HTTP commands are text-based.

### Exam Important Points

- HTML = Language, NOT a protocol.
- TELNET = Protocol used for remote login (detailed discussion in later lecture).

---

---

# 10 MCQs from Lecture 8

---

**Q1.** What is the default port number for HTTP?

A) 25  
B) 21  
C) 80  
D) 443

**Answer: C) 80**  
**Explanation:** As stated in the lecture, the well-known TCP port for HTTP servers is port 80. If no port is specified in the URL, the browser uses port 80 by default.

---

**Q2.** Which version of HTTP specifies a persistent connection by default?

A) HTTP 0.9  
B) HTTP 1.0  
C) HTTP 1.1  
D) HTTP 2.0

**Answer: C) HTTP 1.1**  
**Explanation:** The lecture clearly states that HTTP version 1.1 (RFC 2616) specifies a persistent connection by default. HTTP 1.0 does not have persistent connections by default.

---

**Q3.** HTTP is primarily a _________ protocol.

A) Transport layer  
B) Network layer  
C) Application layer  
D) Data link layer

**Answer: C) Application layer**  
**Explanation:** The lecture describes HTTP as an "application-level protocol with the lightness and speed necessary for distributed, hypermedia information systems."

---

**Q4.** Which HTTP method is used to send data from the client to the server?

A) GET  
B) HEAD  
C) POST  
D) TRACE

**Answer: C) POST**  
**Explanation:** As described in the lecture's HTTP methods table, POST "sends some information from the client to the server." GET retrieves data, HEAD gets info about a document, and TRACE echoes requests.

---

**Q5.** What does HTTP status code 404 mean?

A) Bad Request  
B) Unauthorized  
C) Not Found  
D) Internal Server Error

**Answer: C) Not Found**  
**Explanation:** The lecture specifically identifies 404 as "Not Found" — the document was not found on the server. The professor also calls it the "most commonly seen error."

---

**Q6.** Which type of web document involves processing at the client side (browser)?

A) Static document  
B) Dynamic document  
C) Active document  
D) CGI document

**Answer: C) Active document**  
**Explanation:** The lecture defines three types: Static (no processing), Dynamic (server-side processing), and Active (client-side processing using Java Applets or JavaScript).

---

**Q7.** CGI (Common Gateway Interface) is associated with which type of web document?

A) Static  
B) Dynamic  
C) Active  
D) Passive

**Answer: B) Dynamic**  
**Explanation:** The lecture explains that CGI is used for dynamic documents — a request is sent to the server, a program executes via CGI, and a dynamically generated HTML page is returned.

---

**Q8.** HTTP status codes in the 5xx series indicate:

A) Informational messages  
B) Success  
C) Client-side errors  
D) Server-side errors

**Answer: D) Server-side errors**  
**Explanation:** The lecture categorizes status codes as: 1xx = Informational, 2xx = Success, 3xx = Redirection, 4xx = Client Error, 5xx = Server Error. Examples of 5xx include 500 (Internal Server Error) and 503 (Service Unavailable).

---

**Q9.** Which of the following uniquely identifies a connection in the client-server model?

A) Only IP addresses of client and server  
B) IP address and port of server only  
C) A 5-tuple: Server IP, Server Port, Client IP, Client Port, Protocol ID  
D) Only the URL

**Answer: C) A 5-tuple: Server IP, Server Port, Client IP, Client Port, Protocol ID**  
**Explanation:** The lecture explicitly states that five elements — Server IP, Server Port, Client IP, Client Port, and Protocol ID — together uniquely identify a connection. If any one differs, it is a unique connection.

---

**Q10.** HTTP is described as "transport independent." What does this mean according to the lecture?

A) HTTP does not use any transport layer protocol  
B) HTTP works only with UDP  
C) HTTP protocol itself does not depend on a specific transport layer, though it generally uses TCP  
D) HTTP creates its own transport mechanism

**Answer: C) HTTP protocol itself does not depend on a specific transport layer, though it generally uses TCP**  
**Explanation:** The lecture says HTTP is transport independent — the protocol itself does not mandate a specific transport layer. However, it generally takes place over a TCP connection. TCP is the predominant underlying protocol used with HTTP.

---

*End of Lecture 8 Notes*
