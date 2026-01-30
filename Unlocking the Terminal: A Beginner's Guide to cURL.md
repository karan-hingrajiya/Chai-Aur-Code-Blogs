---
title: "Unlocking the Terminal: A Beginner's Guide to cURL"
seoTitle: "Beginner's Guide to cURL: Unlock the Terminal"
seoDescription: "Discover how to use cURL for powerful command-line requests, APIs, and how to make request to server without browser."
datePublished: Fri Jan 30 2026 16:36:53 GMT+0000 (Coordinated Universal Time)
cuid: cml13vhkz002u02juhpkdg05s
slug: unlocking-the-terminal-a-beginners-guide-to-curl
tags: chaiaurcode, chaicode, chaicohort

---

When I first opened a terminal, it felt like I was looking at the Matrix—just a blinking cursor and a black screen. I was used to clicking buttons in a browser.

But then I learned about a tool called **cURL**, and it changed everything.

If you are learning backend development, APIs, or just want to feel like a hacker, you need to know cURL. It is the Swiss Army knife of the internet.

Today, we are going to learn what cURL is, why every programmer uses it, and how to make your very first request.

---

## Part 1: What is a Server (and why do we need to talk to it)?

Before we type any commands, we need to understand what we are actually doing.

The internet is basically a conversation between two computers:

1. **The Client:** This is you (your browser, your phone, or your terminal).
    
2. **The Server:** A computer somewhere else (like Google's or Amazon's) that has the data you want.
    

Usually, you use a **Web Browser** (like Chrome) to talk to a server. You type [`google.com`](http://google.com), and Chrome sends a message saying, "Hey, give me the homepage." The server replies with the website.

**cURL** (Client URL) is just another way to have that conversation, but instead of using a mouse and pretty graphics, you use **text commands** in the terminal.

**Visualizing the Difference:**

Plaintext

```bash
      [ The Browser Way ]                   [ The cURL Way ]
      +-----------------+                   +-----------------+
      |  You click a    |                   |  You type a     |
      |  button.        |                   |  command.       |
      +-------+---------+                   +--------+--------+
              |                                      |
              v                                      v
      +-----------------+                   +-----------------+
      | Chrome sends    |                   | Terminal sends  |
      | a request.      |                   | a request.      |
      +-------+---------+                   +--------+--------+
              |                                      |
              +------------------+-------------------+
                                 |
                                 v
                        +-----------------+
                        |   The Server    |
                        | (Doesn't care!) |
                        +-----------------+
```

The server doesn't care if you are using Chrome or cURL. It just answers the request.

---

## Part 2: Your First cURL Command

Let's stop talking and start coding. Open your terminal (Command Prompt on Windows, Terminal on Mac/Linux) and type this:

Bash

```bash
curl google.com
```

Press Enter.

You will probably see a huge wall of text that looks like gibberish code (`<HTML>...`).

**Congratulations!** You just manually fetched a website without a browser.

**What just happened?**

1. **You:** "Hey cURL, go fetch [`google.com`](http://google.com)."
    
2. **cURL:** Sent a request to Google's server.
    
3. **Google:** "Here is the HTML code for the page."
    
4. **Terminal:** Displayed that raw code.
    

Your browser normally takes that code and turns it into the colorful Google logo and search bar. cURL just shows you the raw "ingredients."

---

## Part 3: Understanding Request & Response

Every time you use cURL, you are engaging in a standard "Request and Response" cycle. This is the heartbeat of the internet.

1. **The Request:** What you ask for.
    
2. **The Response:** What you get back.
    

When you got that wall of text from Google, that was the **Response Body** (the actual content). but there is also a **Status Code** hidden in there (usually `200 OK` if it worked, or `404 Not Found` if it failed).

**The Diagram: The Request Flow**

Plaintext

```bash
   Your Terminal (cURL)                       The Server
          |                                       |
          |  1. Request: "GET /index.html"        |
          |-------------------------------------->|
          |                                       |
          |  2. Processing...                     |
          |   (Server finds the file)             |
          |                                       |
          |  3. Response: "200 OK" + [Data]       |
          |<--------------------------------------|
          |                                       |
    [Terminal displays Data]
```

---

## Part 4: Using cURL with APIs (The Real Power)

Okay, fetching HTML is cool, but programmers mostly use cURL to talk to **APIs** (Application Programming Interfaces).

APIs don't send back messy HTML code. They send back clean, organized data, usually in a format called **JSON**.

Let's try a real API. We will use a free testing service called `JSONPlaceholder`.

Type this command:

Bash

```bash
curl https://jsonplaceholder.typicode.com/posts/1
```

**You should see something like this:**

JSON

```bash
{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
  "body": "quia et suscipit\nsuscipit recusandae consequuntur..."
}
```

See how clean that is? This is exactly how mobile apps get data. When you open Instagram, the app uses something very similar to cURL behind the scenes to fetch the photos and captions as raw data, then it draws them on your screen.

---

## Part 5: GET vs. POST (Asking vs. Sending)

In the examples above, we were just *asking* for data. In the HTTP world, this is called a **GET** request. cURL does a GET request by default.

But what if you want to *send* data? Like posting a tweet or logging in? That is called a **POST** request.

To do this in cURL, we use a "flag" (an option).

* `-X POST`: Tells cURL to use the POST method.
    
* `-d`: Stands for "data".
    

**Conceptual Example:** If you wanted to send a login request, it might look like this:

Bash

```bash
curl -X POST -d "username=karan&password=123" https://mysite.com/login
```

*Note: Don't run this, it's just an example!*

---

## Part 6: Common Beginner Mistakes

I made all of these mistakes when I started. Save yourself the headache!

1. **Forgetting** `https://`:
    
    * *Bad:* `curl` [`google.com`](http://google.com) (Sometimes works, but can fail).
        
    * *Good:* `curl` [`https://www.google.com`](https://www.google.com). Always be specific.
        
2. **Typing the URL wrong**:
    
    * If you see an error like `Could not resolve host`, you probably made a typo in the website name.
        
3. **Being overwhelmed by flags**:
    
    * You will see tutorials with crazy commands like `curl -iH "Accept: application/json" -X POST ...`.
        
    * **Don't panic.** Start simple. Just use the URL first. Add flags only when you know why you need them.
        

That’s it for cURL there is still multiple commands to learns but this is basic cURL you can learn more about terminal and basic commands [here](https://www.geeksforgeeks.org/linux-unix/basic-linux-commands/).

---

Connect With Me Here :

* [LinkedIn](https://www.linkedin.com/in/hingrajiya-karan-82a78b2a4/)
    
* [Twitter or X](https://x.com/karan1211hk)
    
* [GitHub](https://github.com/karan-hingrajiya?tab=repositories)
    

Thanks for reading my blog. happy reading!!!
