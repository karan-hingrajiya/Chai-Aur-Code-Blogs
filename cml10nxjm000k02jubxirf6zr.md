---
title: "TCP Working: 3-Way Handshake & Reliable Communication"
seoTitle: "Understanding the TCP 3-Way Handshake Process"
seoDescription: "Learn about TCP, its reliable communication through the 3-Way Handshake, and how it ensures data integrity and order"
datePublished: Fri Jan 30 2026 15:07:01 GMT+0000 (Coordinated Universal Time)
cuid: cml10nxjm000k02jubxirf6zr
slug: tcp-working-3-way-handshake-and-reliable-communication
tags: chaiaurcode, chaicode, chaicohort, chai-code

---

## Introduction :

If you’ve ever sent a WhatsApp message and seen those double blue ticks, or if you’ve ever downloaded a file and watched the progress bar hit 100%, you have **TCP** to thank.

When I first started learning backend development, networking felt like magic. How does a movie stream from a server in California to my laptop in India without turning into a glitchy mess?

The answer isn't magic. It's a protocol. Specifically, the **Transmission Control Protocol (TCP)**.

Today, we are going to break down TCP—what it is, why we need it, and exactly how it works (with diagrams!).

---

## Part 1: Why Do We Even Need TCP?

Imagine you want to mail a 500-page book to your friend, but the post office has a rule: **You can only mail single pages.**

So, you rip the pages out and mail 500 individual envelopes.

* **Problem 1:** Some envelopes might get lost.
    
* **Problem 2:** They might arrive in the wrong order (Page 499 comes before Page 1).
    
* **Problem 3:** Some might get damaged or soaked in rain.
    

Without TCP, the internet works exactly like this chaotic post office (that's actually how **UDP** works!).

**TCP** is the solution. It’s the strict librarian who numbers every page, checks them off a list, and demands a receipt for every single delivery. If a page is missing, TCP freezes everything and screams, "SEND PAGE 42 AGAIN!" until the book is perfect.

---

## Part 2: The Setup (The 3-Way Handshake)

Before TCP sends any real data (like a cat meme or a bank password), it forces the two computers to agree to a connection. This is the most famous concept in networking: **The 3-Way Handshake**.

Think of it like starting a phone call. You don't just start shouting your story. You check if the other person is listening first.

### The Steps:

1. **SYN (Synchronize):** The Client (your laptop) sends a packet to the Server saying, *"Hey, I want to connect! Let's sync up."*
    
2. **SYN-ACK (Synchronize-Acknowledge):** The Server replies, *"I hear you! I am ready to sync too."*
    
3. **ACK (Acknowledge):** The Client replies back, *"Great, I got your confirmation. Let's go."*
    

Only *after* step 3 does the actual data start flowing.

**Visualizing the Handshake:**

Plaintext

```bash
      Client (You)                            Server (Google)
          |                                        |
          |   1. SYN (Can I talk to you?)          |
          |--------------------------------------->|
          |                                        |
          |   2. SYN-ACK (Yes! I'm listening)      |
          |<---------------------------------------|
          |                                        |
          |   3. ACK (Okay, connecting now!)       |
          |--------------------------------------->|
          |                                        |
    [Connection Established - Safe to send data]
```

---

## Part 3: The Conversation (Reliability & Ordering)

Now the connection is open. But how does TCP ensure nothing gets lost? It uses two secret weapons: **Sequence Numbers** and **Acknowledgments (ACKs)**.

Every byte of data sent is numbered. It’s like numbering the pages of that book we mailed earlier.

* **Client:** "Here is data packet #1 (Bytes 1-100)."
    
* **Server:** "I received up to byte 100. Please send starting from 101." (This is the ACK).
    

### What happens if a packet gets lost?

This is where TCP shines. If the Client sends Packet #2 but never hears a reply (ACK) from the Server, it assumes the packet was eaten by the internet.

It doesn't panic. It just waits a specific amount of time (Timeout) and then **resends** it automatically.

**Visualizing Packet Loss:**

Plaintext

```bash
      Client                                   Server
          |                                        |
          |      Packet 1 (SEQ 100)                |
          |--------------------------------------->|
          |      <ACK 200 (Got it!)                |
          |<---------------------------------------|
          |                                        |
          |      Packet 2 (SEQ 200)                |
          |--------X (LOST IN TRANSIT)             |
          |                                        |
    (Wait...)                                      |
    (Wait...)                                      |
    (Timeout!)                                     |
          |                                        |
          |      Packet 2 (Resending SEQ 200)      |
          |--------------------------------------->|
          |      <ACK 300 (Got it now!)            |
          |<---------------------------------------|
```

---

## Part 4: The Breakup (Connection Termination)

All good things must come to an end. When the data transfer is finished (like when a webpage is fully loaded), the connection needs to be closed to free up your computer's memory.

We can't just hang up abruptly—there might be data still traveling in the wire! So, we use a polite goodbye, often called the **4-Way Handshake**.

1. **FIN (Finish):** Client says, *"I'm done sending data."*
    
2. **ACK:** Server says, *"Okay, I received your request to stop."*
    
3. **FIN:** Server says, *"I'm also done sending data. Goodbye."*
    
4. **ACK:** Client says, *"Goodbye."*
    

That’s it i came with the most beginner friendly way and with some basic analogy so, it will become easy for most beginner to understand also i took help from Ai how TCP works i gain knowledge from it and i read some articles about it so i can explain this in very easy way that i understand it in future too.

---

Connect With Me Here :

* [LinkedIn](https://www.linkedin.com/in/hingrajiya-karan-82a78b2a4/)
    
* [Twitter or X](https://x.com/karan1211hk)
    
* [GitHub](https://github.com/karan-hingrajiya?tab=repositories)
    

Thanks for reading my blog. happy reading!!!