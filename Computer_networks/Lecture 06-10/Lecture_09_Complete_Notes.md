# Lecture 9: Application Layer — IV (HTTP, HTML, TELNET) (Continued)

**Course:** Computer Networks and Internet Protocol  
**Instructor:** Prof. Soumya Kanti Ghosh, IIT Kharagpur  

---

## Overview of This Lecture

This lecture continues the discussion on HTTP and HTML, and then introduces a new protocol called **TELNET**. The topics covered are:

1. HTTP Proxy Server (brief introduction)
2. HTML — What it is, its elements, tags, structure
3. HTML Tags — Headings, Paragraphs, Links, Images, Divisions
4. HTML Document Structure
5. TELNET Protocol — What it is, how it works
6. TELNET vs. telnet (protocol vs. program)
7. Network Virtual Terminal (NVT)
8. Negotiated Options in TELNET
9. Control Functions in TELNET
10. Command Structure — IAC (Interpret As Command)
11. TELNET Operations and Usage

---

## Concept 1: HTTP Proxy Server (Brief Introduction)

### Simple Explanation

Before jumping into HTML, the lecture briefly mentions an important concept — the **HTTP Proxy Server**.

A proxy server is like a **middleman** that sits between the HTTP client (your browser) and the HTTP server (the website). Instead of your browser directly talking to the web server, it first talks to the proxy server, and the proxy server then talks to the web server on your behalf.

**What can a proxy server do?**

- **Caching** — It can store (cache) copies of web pages. So if you request the same page again, the proxy can give it to you faster without going to the original server.
- **Filtering** — It can control who is allowed to access what. For example, in an office, a proxy can block certain websites.
- **Logging** — It can keep records (logs) of what data was accessed and by whom.

The lecture says this topic will be revisited in more detail later in the course.

### Exam Important Points

- A proxy server acts as an **intermediary** between the HTTP client and server.
- It can perform **caching**, **filtering**, and **logging**.
- This is just an introduction; more details come later.

### Common Confusions

- Proxy server is NOT the same as the web server itself. It is a separate, intermediate server.
- Proxy does not replace the web server — it sits in between client and server.

---

## Concept 2: What is HTML?

### Simple Explanation

**HTML** stands for **Hyper Text Markup Language**.

Think of HTML as a **language that tells the web browser how to display a web page**. When you visit a website, what your browser receives from the server is an HTML file. Your browser then reads the HTML and shows you the formatted page (with text, images, links, colors, etc.).

Key points about HTML:

- It is a **computer language** used to **create web pages**.
- An HTML file is basically a **text file** that contains special instructions called **markup tags** (like `<p>` for paragraph).
- These **tags tell the web browser how to display** the content on the page.
- HTML files can have the extension **.htm** or **.html** (both are valid).

### Real-world Example (from transcript)

When you open any website in a browser (like the IIT Kharagpur homepage mentioned in the lecture), what you see is the result of the browser reading and interpreting the HTML code behind that page.

### Exam Important Points

- HTML = **Hyper Text Markup Language**
- Used to **create web pages**
- HTML file = **text file containing markup tags**
- Tags tell the **browser how to display** the page
- Extension can be **.htm** or **.html**

### Common Confusions

- HTML is NOT a programming language (like C or Java). It is a **markup language** — it describes how content should be displayed, not logic or computation.
- HTML by itself does not make a page "look beautiful." The aesthetic design depends on the person creating the page (and often CSS is used for styling, though the transcript does not go into CSS details).

---

## Concept 3: HTML Elements and Tags

### Simple Explanation

HTML is built using **tags**. Tags are the building blocks of any HTML page.

**Key rules about tags:**

1. Tags are surrounded by **angle brackets**: `< >`
2. Tags usually come in **pairs** — there is a **start tag** and an **end tag**.
   - Example: `<p>` is the start tag, `</p>` is the end tag.
   - The slash `/` in the end tag marks it as a closing tag.
3. Whatever is written **between** the start and end tag is called the **element content**.
4. Tags are **not case sensitive** — `<P>` and `<p>` mean the same thing. But the new standard recommends using **lower case**.

### Exam Important Points

- Tags are surrounded by **angle brackets `< >`**.
- Tags come in **pairs**: start tag `<tag>` and end tag `</tag>`.
- Content between tags = **element content**.
- Tags are **not case sensitive**, but **lower case is the new standard**.

### Common Confusions

- Not ALL tags come in pairs. Some tags are **empty tags** (self-closing), like the image tag `<img>`. This is explained later.
- The **end tag** always has a `/` before the tag name.

---

## Concept 4: Structure of an HTML Document

### Simple Explanation

Every HTML document follows a basic structure. Think of it like the skeleton of a human body — every page has the same skeleton.

Here is the basic structure:

```
<html>
  <head>
    <title> Title of the Page </title>
  </head>
  <body>
    ... Page content goes here ...
  </body>
</html>
```

Let's understand each part:

- **`<html>` ... `</html>`** — This is the **container for the entire document**. Everything goes inside this. It tells the browser: "This is an HTML document."
- **`<head>` ... `</head>`** — This is the **header section**. It contains information about the page, like the **title**.
- **`<title>` ... `</title>`** — This defines the **title of the page**, which appears on the browser tab (like "Indian Institute of Technology Kharagpur").
- **`<body>` ... `</body>`** — This is where the **actual content** of the page goes — the text, images, links, everything that you see on the screen.

There is also a **Document Type Definition (DTD)** that appears as the first line of the code. It declares which version of HTML is being used. The DTD defines what elements and attributes can be used in the HTML document.

### Exam Important Points

- `<html>` = Container for the **whole document**
- `<head>` = **Header** of the page (contains title, metadata)
- `<title>` = **Title** of the page (shown on browser tab)
- `<body>` = **Content** of the page (what user sees)
- **DTD (Document Type Definition)** = first line, declares the HTML version

### Common Confusions

- The `<head>` section is NOT displayed on the page. It is metadata (information about the page, like the title).
- The `<body>` section is what you actually see on screen.

---

## Concept 5: Common HTML Tags

### 5a. Heading Tags

HTML provides **six levels of headings** — from `<h1>` (the largest/most important) to `<h6>` (the smallest/least important). This is similar to heading levels in Microsoft Word.

```
<h1> Heading 1 </h1>
<h2> Heading 2 </h2>
...
<h6> Heading 6 </h6>
```

### 5b. Paragraph Tag `<p>`

The `<p>` tag is used to define a **paragraph**. It automatically **inserts a line space** before and after the paragraph.

```
<p> This is a paragraph. </p>
```

### 5c. Link Tag (Anchor Tag) `<a>`

The **anchor tag `<a>`** is used to create **hyperlinks** — clickable links that take you to another page or location.

**Three types of links:**

1. **Link to a page in the same folder** — The linked file is in the same directory.
2. **Link to a page in a different folder** — The linked file is in a different directory.
3. **Link to an outside webpage on the Internet** — The link goes to a completely different website.

**How does it look?**

```
<a href="http://www.iitkgp.ac.in"> Go to IITKgp home page </a>
```

**Two components of the anchor tag:**
- **Address** (the URL inside `href`) — This is where the link points to.
- **Text/Description** — This is what the user sees as clickable text on the page.

### 5d. Image Source Tag `<img>`

The `<img>` tag is used to **insert an image** on the page.

**Important:** This is an **empty tag** — it has **no closing tag**.

```
<img src="url" alt="description of image" />
```

- **`src`** = Points to the **location (URL)** of the image file.
- **`alt`** = Provides a **text description** of the image (used by screen readers and displayed when the image cannot load).

**File location of images:**
- **Same folder:** Just write the file name, e.g., `"samplePic.gif"` (this is a document-relative link).
- **Different folder:** Specify the folder path, e.g., `"/images/samplePic.gif"`.

### 5e. Division Tag `<div>`

The `<div>` tag defines a **division or section** in the HTML document. It is used to **group elements together** so you can apply formatting or style to that entire group.

Example: You can set a color for a whole section using `<div>`.

### Exam Important Points

- **6 heading levels**: `<h1>` to `<h6>`
- `<p>` = paragraph, inserts line space before and after
- `<a>` = anchor/link tag, has **href** (address) and **display text**
- Three types of links: same folder, different folder, outside webpage
- `<img>` = image tag, **empty tag (no closing tag)**
- `<img>` has **src** (source URL) and **alt** (alternative text description)
- `<div>` = division/section, used to group elements for styling

### Common Confusions

- The `<img>` tag does NOT have a closing `</img>` tag. It is self-closing.
- The `<a>` tag text is what the user clicks on, but the actual destination is inside `href`.
- `<div>` by itself does not display anything special — it is a grouping tool.

---

## Concept 6: Page Design and Customization

### Simple Explanation

The lecture mentions that after creating a basic HTML page, you can customize it further:

- **Text color** — Change the color of text
- **Link color** — Change the color of hyperlinks
- **Background color** — Change the background color of the page
- **Font size** — Change how big or small the text is
- **Type of font** — Choose different font styles

The lecture emphasizes that **designing a web page is more about the aesthetic sense** of the person who designs it. The technology (HTML syntax) is there, but making a page appealing requires good design skills.

### Exam Important Points

- HTML allows customization of colors (text, link, background) and fonts (size, type).
- Page design quality depends on the designer's aesthetic sense, not just HTML syntax.

---

## Concept 7: Introduction to TELNET

### Simple Explanation

Now we move to a completely new protocol — **TELNET**.

**What is TELNET?**

TELNET is an application layer protocol that allows you to **remotely log in** to another computer/server over a network. Once connected, you can work on that remote machine as if you were sitting right in front of it.

Think of it this way: You are sitting at your computer at home, but using TELNET, you can connect to a server at your college/office and use that server's terminal — run programs, access files, and so on.

**How does it work?**

- There is a **TELNET client** (on your machine) and a **TELNET server** (on the remote machine).
- The client sends your input (commands) to the server.
- The server processes the commands and sends the output back to the client.
- The server side runs a **TELNET daemon (telnetd)** — a background process that listens for incoming TELNET connections.

**Key point from transcript:** The **protocol** is called **TELNET** (capital letters), and the **program/application** is also called **telnet** (small letters).

### Exam Important Points

- TELNET is used for **remote login** to another system.
- Works on **client-server model**.
- Server runs a **telnet daemon (telnetd)**.
- TELNET = protocol name; telnet = program name.

### Common Confusions

- TELNET is NOT the same as HTTP. HTTP is for viewing web pages; TELNET is for remotely logging into and using another machine.
- You need **authentication** (username and password) to log in via TELNET. It is not open access.

---

## Concept 8: TELNET vs. telnet (Protocol vs. Program)

### Simple Explanation

The lecture makes an important distinction:

| | TELNET (uppercase) | telnet (lowercase) |
|---|---|---|
| **What is it?** | A **protocol** | A **program/application** |
| **Definition** | Provides a general, **bidirectional**, **eight-bit byte-oriented** communications facility | A program that **supports the TELNET protocol over TCP** |

**Key points:**

- TELNET is the protocol that defines the rules for bidirectional, 8-bit byte-oriented communication.
- telnet is the actual software/program you run on your computer that follows this protocol.
- **Many application protocols are built upon the TELNET protocol.** This means TELNET serves as a base, and other applications can "piggyback" on it.

### Exam Important Points

- TELNET = protocol; **bidirectional, 8-bit byte-oriented** communication facility
- telnet = program that supports TELNET protocol **over TCP**
- TELNET is a **connection-oriented** service (because it uses TCP)
- Many application protocols are **built upon TELNET**

### Common Confusions

- "Bidirectional" means data flows **both ways** — from client to server AND from server to client.
- "8-bit byte-oriented" means it transmits data in units of 8-bit bytes.

---

## Concept 9: The TELNET Protocol — Technical Details

### Simple Explanation

Here are the key technical details of the TELNET protocol:

- **Reference:** RFC 854 (the official document defining TELNET)
- **Uses TCP connection** — so it is **connection-oriented** and reliable.
- **Well-known port: 23** — This is the default port for TELNET. But you CAN use TELNET on other ports too (just like HTTP's default is port 80 but can be used on other ports).
- **Data and control go over the SAME connection** — Unlike FTP (which uses two separate connections — port 20 for data and port 21 for control), TELNET uses only **one single TCP connection** for both data and commands.

### Exam Important Points

- TELNET is defined in **RFC 854**
- Uses **TCP** (connection-oriented)
- Default port = **23**
- **Data and control travel on the SAME connection** (unlike FTP which has separate connections)

### Common Confusions

- **FTP uses TWO connections** (port 20 for data, port 21 for control). **TELNET uses ONE connection** for both. This is a very important difference for exams.
- Port 23 is the default, but TELNET CAN be used on other ports.

---

## Concept 10: Network Virtual Terminal (NVT)

### Simple Explanation

The **Network Virtual Terminal (NVT)** is a very important concept in TELNET.

**Problem:** Different computers may have different types of terminals (different keyboard layouts, different character sets, different control codes). If a TELNET client and server have different terminal types, how will they communicate properly?

**Solution:** NVT!

NVT is an **intermediate representation of a generic terminal**. It provides a **standard language** for communication of terminal control functions.

**How does it work?**

- Both the client side and server side have an NVT.
- The client converts its local terminal's data into the NVT format and sends it.
- The server receives the NVT format data and converts it into its local terminal's format.
- This way, **no matter what type of terminal** the client or server has, they can communicate through the common NVT standard.

**Architecture (from transcript):**

```
TCP → NVT → Server Processes
```

The underlying TCP provides the reliable connection. Over that, NVT provides the standard terminal representation. And above that, the actual server processes run.

### Exam Important Points

- NVT = **Network Virtual Terminal**
- It is an **intermediate representation of a generic terminal**
- Provides a **standard language for communication of terminal control functions**
- Solves the problem of different terminal types between client and server
- Both client and server have NVT
- Underlying transport = **TCP**

### Common Confusions

- NVT is NOT a physical terminal. It is a **virtual (software-based) standard** that both sides agree upon.
- Without NVT, escape characters and control codes could cause problems between different terminal types.

---

## Concept 11: Negotiated Options

### Simple Explanation

The lecture explains that all NVTs support a **minimal set of capabilities** — a basic set of features that every terminal must support.

However, **some terminals have more capabilities** than this minimal set. For example, some terminals might support special characters, colors, or other advanced features.

**Key points about options:**

- The **set of options is NOT part of the TELNET protocol** itself. This is important! Options are kept separate from the protocol so that **new terminal features can be added without changing the TELNET protocol**.
- The **two endpoints (client and server) negotiate** a set of **mutually acceptable options** before communication starts.

**Examples of negotiated options:**

- **Line mode vs. Character mode** — In character mode, each character is sent as it is typed. In line mode, the entire line is sent at once.
- **Echo modes** — Whether the server should echo back what the user types.
- **Character set** — For example, **EBCDIC vs. ASCII** — both sides must agree on which character encoding to use.

### Exam Important Points

- All NVTs support a **minimal set of capabilities**
- Options are **NOT part of the TELNET protocol** — kept separate for flexibility
- New features can be added **without changing the protocol**
- Two endpoints must **negotiate mutually acceptable options**
- Examples: **Line mode vs. Character mode**, **Echo modes**, **EBCDIC vs. ASCII**

### Common Confusions

- Options being separate from the protocol is a **design choice for flexibility** — it makes TELNET more adaptable.
- Both sides must AGREE on the options. If they don't agree, communication will have problems.

---

## Concept 12: Control Functions

### Simple Explanation

TELNET includes support for a series of **control functions** that are commonly supported by servers. These provide a **uniform mechanism** for communication of control functions.

Here are the control functions mentioned in the lecture:

| Abbreviation | Full Name | What It Does |
|---|---|---|
| **IP** | Interrupt Process | **Suspend or abort** the currently running process |
| **AO** | Abort Output | **Stop sending output** to the user's terminal (but the process may still run) |
| **AYT** | Are You There | **Check if the system is still running** — like a "are you alive?" check |
| **EC** | Erase Character | **Delete the last character** that was sent |
| **EL** | Erase Line | **Delete all input** in the current line |

### Exam Important Points

- **IP** = Interrupt Process → Suspend/abort process
- **AO** = Abort Output → Send no more output to user's terminal
- **AYT** = Are You There → Check if system is still running
- **EC** = Erase Character → Delete last character sent
- **EL** = Erase Line → Delete all input in current line
- These provide a **uniform mechanism** for control communication

### Common Confusions

- **AO (Abort Output)** does NOT necessarily stop the process itself — it just stops the output from being sent to the terminal.
- **IP (Interrupt Process)** actually stops/suspends the process.
- **EC** deletes only the **last character**; **EL** deletes the **entire current line**.

---

## Concept 13: Command Structure — IAC (Interpret As Command)

### Simple Explanation

This is a very important concept for understanding how TELNET sends commands.

**Key principle:** In TELNET, **all commands AND data flow through the SAME TCP connection**. So TELNET needs a way to tell the difference between data and commands.

**Solution:** The **IAC (Interpret As Command)** character.

Here is how it works:

- All TELNET commands start with a special character called **IAC**.
- The **IAC code is 255** (decimal).
- So when the TELNET layer sees a byte with value 255, it knows: "The next byte is a command, not data."

**But what if the data itself contains the value 255?**

- If **255 needs to be sent as data** (not as a command), then it must be **followed by another 255**.
- So: two consecutive 255s (255 + 255) = "this is just the data value 255, not a command."

**The rules:**

1. If IAC (255) is found and the **next byte is also IAC (255)** → A single byte of data value 255 is passed to the application/terminal. (This is an escape mechanism.)
2. If IAC (255) is found and the **next byte is any other code** → The TELNET layer interprets this as a **command** and executes it accordingly.

### Exam Important Points

- All commands and data flow on the **SAME TCP connection**
- Commands start with **IAC (Interpret As Command)**
- **IAC code = 255**
- If data itself is 255 → it must be **followed by another 255** (so: 255, 255 = data value 255)
- IAC + IAC (255 + 255) → **single byte of data** passed to application
- IAC + any other code → **interpreted as a command**

### Common Confusions

- IAC is NOT a separate connection — it is a special byte WITHIN the same data stream.
- The "255 followed by 255" rule is an **escape mechanism** — it prevents confusion between data and commands.
- This is fundamentally different from FTP, which avoids this problem by using **two separate connections**.

---

## Concept 14: TELNET Operations and Usage

### Simple Explanation

The **telnet program** (lowercase) is a very useful tool. It is a **generic TCP client** that you can use to interact with many different servers.

**How does telnet (the program) work?**

- It **sends whatever you type** to the TCP socket (connection).
- It **prints whatever comes back** from the TCP socket.
- It is useful for **testing TCP servers** that use ASCII-based protocols.

**Syntax:** `telnet hostname port`

For example: `telnet skg.cse.edu 7` — This connects to the server `skg.cse.edu` on port 7.

**Common servers you can test with telnet (mentioned as running by default on many Unix/Linux systems):**

| Server | Port |
|---|---|
| **Echo server** | Port **7** |
| **Discard server** | Port **9** |
| **Daytime server** | Port **13** |
| **Chargen server** | Port **19** |

**Echo server example from transcript:**

When you connect to port 7 (echo server) and type something, the server simply sends back (echoes) exactly what you typed. This is useful as a **first-level check** to verify if the TELNET server at the other end is running and responding properly.

**Other uses of telnet:**

- You can test a **mail server (SMTP)** by doing: `telnet mailserver 25` (SMTP uses port 25).
- TELNET acts as a **carrier protocol** — it establishes the connection, and other applications can piggyback on it.

### Exam Important Points

- telnet is a **generic TCP client**
- Sends what you type, prints what comes back
- Useful for **testing ASCII-based TCP servers**
- **Echo server = port 7**, **Discard = port 9**, **Daytime = port 13**, **Chargen = port 19**
- Can be used to test other servers too (like SMTP on port 25)
- TELNET is a **carrier protocol** — other applications can piggyback on it

### Common Confusions

- The echo server at port 7 just sends back what you type — it is for **testing**, not for actual work.
- When you telnet to a server, you still need **authentication** (login + password) to actually use the remote system.

---

## Concept 15: TELNET Authentication and Access

### Simple Explanation

The lecture emphasizes that when you use TELNET to connect to a remote server:

- The server **prompts you for a login and password**.
- You must be **authenticated** — meaning you need a valid username and password on that remote system.
- Once logged in, you can **browse directories**, **execute programs**, and do anything that your **permission set allows**.
- You can TELNET to:
  - The **same system** (localhost)
  - A system in the **same network**
  - A system in a **different network** (over the internet)
- The connection **stays active** as long as you are working or until an error occurs.

### Exam Important Points

- TELNET requires **authentication** (login + password)
- User can only access what their **permissions allow**
- Can TELNET to same system, same network, or different network
- Connection persists until user quits or error occurs

---

## Concept 16: Lecture Summary (from the transcript)

The lecture concludes by summarizing:

1. **HTTP and HTML** — HTTP (Hypertext Transfer Protocol) makes the World Wide Web work. HTML (Hypertext Markup Language) is the language in which web documents are written. The browser has an **HTML interpreter/parser** that reads the HTML tags and displays the page accordingly. This makes information exchange possible over the network.

2. **TELNET** — A connection-oriented protocol for **remotely connecting** to another machine. It is a simple, "vanilla" type of protocol that allows other applications to **piggyback** on it because it establishes a reliable TCP connection.

3. **Coming next** — The course will discuss SMTP (for email) and SNMP (for network management) in subsequent lectures.

---

---

# 10 MCQs — Strictly from Lecture 9

---

### Q1. What does HTML stand for?

(A) Hyper Transfer Markup Language  
(B) Hyper Text Markup Language  
(C) Hyper Text Machine Language  
(D) High Text Markup Language  

**Answer: (B)**  
**Explanation:** As stated in the lecture, HTML stands for **Hyper Text Markup Language**. It is a markup language used to create web pages.

---

### Q2. Which of the following is the default port number for TELNET?

(A) 80  
(B) 21  
(C) 23  
(D) 25  

**Answer: (C)**  
**Explanation:** The lecture clearly states that the popular/well-known TELNET port is **port 23**. Port 80 is HTTP, port 21 is FTP control, and port 25 is SMTP.

---

### Q3. In TELNET, data and control information travel over:

(A) Two separate TCP connections  
(B) The same TCP connection  
(C) A UDP connection  
(D) One TCP and one UDP connection  

**Answer: (B)**  
**Explanation:** The lecture states that all TELNET commands and data flow through the **same TCP connection**. This is unlike FTP which uses two separate connections.

---

### Q4. What is the IAC code value in TELNET?

(A) 128  
(B) 127  
(C) 256  
(D) 255  

**Answer: (D)**  
**Explanation:** The lecture states that the IAC (Interpret As Command) code is **255**. If 255 needs to be sent as data, it must be followed by another 255.

---

### Q5. What is the Network Virtual Terminal (NVT)?

(A) A physical terminal device  
(B) An intermediate representation of a generic terminal  
(C) A specific type of Linux terminal  
(D) A protocol for video calls  

**Answer: (B)**  
**Explanation:** The lecture defines NVT as an **intermediate representation of a generic terminal** that provides a standard language for communication of terminal control functions.

---

### Q6. The `<img>` tag in HTML is an example of:

(A) A paired tag with opening and closing  
(B) An empty tag with no closing tag  
(C) A header tag  
(D) A division tag  

**Answer: (B)**  
**Explanation:** The lecture explicitly states that the image source tag is an **empty tag — no closing tag**. It is self-closing.

---

### Q7. Which TELNET control function is used to check if the system is still running?

(A) IP (Interrupt Process)  
(B) AO (Abort Output)  
(C) AYT (Are You There)  
(D) EC (Erase Character)  

**Answer: (C)**  
**Explanation:** The lecture states that **AYT (Are You There)** is used to check to see if the system is still running — like an "are you alive?" check.

---

### Q8. The echo server in Unix/Linux systems runs on which port by default?

(A) 9  
(B) 13  
(C) 19  
(D) 7  

**Answer: (D)**  
**Explanation:** The lecture states that the echo server runs on **port 7**. Discard is port 9, daytime is port 13, and chargen is port 19.

---

### Q9. In TELNET, if IAC (255) is sent followed by another IAC (255), what does it mean?

(A) It is interpreted as a command  
(B) It is interpreted as a single byte of data (value 255) passed to the application  
(C) The connection is terminated  
(D) It signals an error  

**Answer: (B)**  
**Explanation:** The lecture states that if IAC is found and the next byte is also IAC, a **single byte of data is presented to the application/terminal**. This is the escape mechanism for sending the data value 255.

---

### Q10. Which of the following is NOT a negotiated option in TELNET according to the lecture?

(A) Line mode vs. Character mode  
(B) Echo modes  
(C) Character set (EBCDIC vs. ASCII)  
(D) Encryption algorithm  

**Answer: (D)**  
**Explanation:** The lecture mentions three examples of negotiated options: **line mode vs. character mode**, **echo modes**, and **character set (EBCDIC vs. ASCII)**. Encryption algorithm is NOT mentioned as a negotiated option in this lecture.

---

## What Else Is Covered in This Course (Other Lectures)

The course continues with more application layer protocols in upcoming lectures, including:

- **SMTP** (Simple Mail Transfer Protocol) — for email
- **SNMP** (Simple Network Management Protocol) — for network management

These are mentioned at the end of Lecture 9 as topics for subsequent discussions.

---

*This completes the full explanation of Lecture 9. Every concept covered in the transcript has been explained above.*
