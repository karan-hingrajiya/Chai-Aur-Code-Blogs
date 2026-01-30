---
title: "How DNS Resolution Works: How to "Dig" for Answers When Websites Break"
seoTitle: "Understanding DNS: Learning 'dig' Command Easy Way"
seoDescription: "Learn how DNS resolution works and use the `dig` command to troubleshoot when websites don't load. Gain insight into internet layers"
datePublished: Fri Jan 30 2026 06:46:50 GMT+0000 (Coordinated Universal Time)
cuid: cml0isoxf000002l7bp6s0foh
slug: how-dns-resolution-works-how-to-dig-for-answers-when-websites-break
tags: chaiaurcode, chaicode, chaicohort, networking-with-chaicode

---

Let’s be real for a second: if you work in tech, or even if you're just learning the ropes, you’ve probably heard the joke: *"It’s not DNS. There’s no way it’s DNS... It was DNS."*

We often say DNS is the "phonebook of the internet." It turns human names (like [`google.com`](http://google.com)) into computer numbers (IP addresses like `142.250.190.46`). Simple, right?

But when a website doesn't load, simply knowing it's a "phonebook" doesn't help you fix it. You need to know how to open that phonebook and check the pages yourself to see where the connection is broken.

To do that, we use a command-line tool called `dig`.

Think of `dig` as your X-ray vision for the internet. It lets you see exactly who your computer is talking to. Today, I’m going to show you how to use it to understand how the internet *actually* works, layer by layer.

### The Mental Model: The 3 Layers of the Internet

Before we type any commands, you need a mental image of the system. DNS isn't one big server somewhere; it's a hierarchy, kind of like a corporate org chart.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769755460418/581c26c8-c082-46c7-9c61-a0ff1f59635d.jpeg align="center")

1. **The Root:** The CEO at the very top.
    
2. **The TLD (Top Level Domain):** The department managers (like .com, .org, .io).
    
3. **The Authoritative Servers:** The specific employees who actually have the answers (the IP addresses).
    

Let’s use `dig` to meet them, starting from the top.

---

### Step 1: The Root (Where it all starts)

Every time you look up a website, the search technically starts at the "Root." In the DNS world, the root is represented by a simple dot (`.`).

Open your terminal and type this:

Bash

```bash
dig . NS +short
```

*(Note: The* `+short` part just tells `dig` to cut the chatter and only show us the answers).

**You'll see a list like this:**

Plaintext

```bash
a.root-servers.net.
b.root-servers.net.
...
m.root-servers.net.
```

**What does this mean?** You just asked the internet, *"Who runs the whole show?"* These servers are the backbone of the internet. They don't know where [`google.com`](http://google.com) is—they're too high-level for that. But they *do* know who manages `.com`.

### Step 2: The TLD (The Managers)

Since we want to find [`google.com`](http://google.com), the Root servers will point us to the `.com` managers. These are called **TLD (Top Level Domain) Servers**.

Let's ask who runs all the `.com` domains:

Bash

```bash
dig com NS +short
```

**You'll see something like:**

Plaintext

```bash
a.gtld-servers.net.
b.gtld-servers.net.
...
```

**What does this mean?** These servers act like traffic cops. They don't have the IP address for Google either. But they have a massive list saying, *"Oh, you want Google? Their specific DNS servers are over there."*

### Step 3: The Source of Truth (Authoritative Servers)

Finally, we get to the servers that actually belong to Google. These are the **Authoritative Name Servers**. This is where the buck stops.

Let's find out who is responsible for [`google.com`](http://google.com):

Bash

```bash
dig google.com NS +short
```

**Output:**

Plaintext

```bash
ns1.google.com.
ns2.google.com.
ns3.google.com.
ns4.google.com.
```

**What does this mean?** Success! We found the specific servers that hold the map for Google's website. If you were looking up your own personal website, this is where you’d see the nameservers for GoDaddy, Namecheap, or Cloudflare.

### Step 4: Getting the Answer (The A Record)

Now that we know *who* has the answer, we can finally ask for the IP address. This is the "A Record" (Address Record).

Bash

```bash
dig google.com +short
```

**Output:**

Plaintext

```bash
142.250.190.46
```

That’s it! That number is the actual destination your browser needs to load the page.

---

### Putting it all together: The "Recursive" Dance

Here is the cool part. When you are just browsing the web in Chrome or Safari, you don't run these commands manually one by one. Your computer hires a "middleman" to do it for you.

This middleman is called a **Recursive Resolver** (usually provided by your ISP or a public DNS like Google's `8.8.8.8`). It does all the running around we just did.

Here is what that conversation looks like behind the scenes:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769755435657/24ad48ef-9ae5-46b2-aad5-34c3b966a21b.jpeg align="center")

1. **You:** "Hey, I need [`google.com`](http://google.com)."
    
2. **Resolver:** "On it." (Goes to Root) "Where is .com?"
    
3. **Root:** "Talk to the TLD servers."
    
4. **Resolver:** (Goes to TLD) "Where is [google.com](http://google.com)?"
    
5. **TLD:** "Talk to [ns1.google.com](http://ns1.google.com)."
    
6. **Resolver:** (Goes to [ns1.google.com](http://ns1.google.com)) "What is the IP?"
    
7. [**ns1.google.com**](http://ns1.google.com)**:** "It's 142.250.190.46."
    
8. **Resolver:** "Here you go, boss."
    

To see how the `dig` commands we just learned map to this process, here's a helpful visual guide:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769755420631/e1e2a3da-1fcd-47db-8e67-d6720798b25c.jpeg align="center")

Once the resolver has the IP address, it gives it to your browser, which can then connect to the web server. This is the final step:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769755403261/c166261f-1463-4f32-adaa-e84b4bf583e8.jpeg align="center")

### Why should you care?

Understanding `dig` isn't just for showing off in the terminal. It's for when things break.

* If `dig . NS` fails? Your internet connection is probably down.
    
* If `dig` [`google.com`](http://google.com) `NS` gives you the wrong servers? You might have configured your domain registrar incorrectly.
    
* If `dig` [`google.com`](http://google.com) gives no IP but the NS records are fine? You likely forgot to create an A-Record.
