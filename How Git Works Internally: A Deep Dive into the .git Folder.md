---
title: "How Git Works Internally: A Deep Dive into the .git Folder"
seoTitle: "Git Internal Mechanics: Exploring the .git Folder"
seoDescription: "Explore the internal workings of Git by diving into the hidden `.git` folder, revealing how Git manages your project history"
datePublished: Sat Jan 17 2026 13:25:16 GMT+0000 (Coordinated Universal Time)
cuid: cmkicazqn000602l51n885sa6
slug: how-git-works-internally-a-deep-dive-into-the-git-folder
tags: git-in-depth-flow-how-git-internally-woks-git-folder-beginner-friendly-git-in-depth-flow

---

most of the people know how to use git and how git and github works to gather but is it only thing you want to know about git and github ever wonder how git actually works internally this topic makes me curious every time that how git works actually internally what’s inside of .git folder so we will know today what’s beneath the .git folder how git actually works.

this topic is related to people who knows how to git and github works. if you don’t know about git and github how its works what it is. i made different blog of explaining this basics of what is git how people used to work before git and what is version control system its actually pretty easy to understand so i made most beginner friendly blog about it [**<mark>here</mark>**](https://hashnode.com/post/cmkghfpx6000i02l7h659f6c8) you can check it out that first.

## Introduction

Now, let’s dive deeper into the `.git` folder and understand how it is created and why it plays such an important role in Git.

When we start working on a project and want Git to **track our code**, the first step is to initialize Git inside the project directory. We do this using the command:

```bash
git init
```

When this command is executed, **Git** creates a special directory named `.git` inside the project folder. This `.git` directory is the heart of Git—it is where Git stores everything related to version control.

---

## Why Can’t We See the `.git` Folder?

If you run the normal command:

```bash
ls
```

you will notice that the `.git` folder does **not** appear in the output. This is because the `.git` directory is a **hidden folder**.

In Unix-based systems, any file or folder that starts with a dot (`.`) is considered hidden by default. The `ls` command only lists visible files in the current directory.

To view hidden files and folders, we use:

```bash
ls -a
```

Now you will see entries like:

* `.` (current directory)
    
* `..` (parent directory)
    
* `.git`
    

This confirms that the `.git` folder exists—it is just hidden from normal view.

---

## Why Is the `.git` Folder Hidden?

The `.git` folder contains **critical internal data** that Git uses to function properly. Whenever you run commands such as:

* `git add`
    
* `git commit`
    
* `git log`
    

Git stores all related information **inside the** `.git` directory.

This includes:

* complete commit history
    
* file snapshots
    
* metadata about the project
    
* author information
    
* branch references
    
* pointers like `HEAD`
    

In simple words, **your entire project history lives inside the** `.git` folder.

Because this data is extremely important, Git keeps the `.git` directory hidden to prevent users from accidentally modifying or deleting internal files. Any manual change inside this folder can corrupt the repository and break Git’s tracking mechanism.

---

## Why Git Manages `.git` for You

Git hides the `.git` folder because:

* users should not manually edit internal files
    
* Git needs full control over history management
    
* accidental changes could destroy commit history
    

If the `.git` folder is deleted, Git completely forgets the project’s history.

That is why Git hides this folder by default and expects users to interact with it **only through Git commands**, not manual edits.

That is why Git provides **commands** as the only safe way to interact with the data stored inside `.git`.

---

## Understanding the `.git` Folder (Step by Step) :

Now that we know the `.git` folder is the heart of **Git**, let’s actually **look inside it** and understand **how Git knows where we are and what our current commit is**.

We’ll move slowly and explain **every step**, assuming the reader is a beginner.

---

## What Happens When We List the `.git` Folder?

Inside your project, run:

```bash
ls .git
```

### What this command does

* `ls` → lists files and folders
    
* `.git` → tells the terminal to list contents of the `.git` directory
    

### Typical output looks like this:

```bash
HEAD
config
objects
refs
logs
index
```

You don’t need to understand everything right now.  
For now, we’ll focus on **three important things**:

* `HEAD`
    
* `refs`
    
* `objects`
    

---

## How Git Knows Where You Are (HEAD)

Whenever you ask Git questions like:

* “Which branch am I on?”
    
* “What is my current commit?”
    

Git starts from `HEAD`.

### Let’s read the HEAD file

```bash
cat .git/HEAD
```

### What does `cat` mean?

* `cat` → prints the content of a file to the terminal
    

### Output:

```bash
ref: refs/heads/main
```

---

## What Does `ref: refs/heads/main` Mean?

This line tells us something very important:

> **HEAD is not a commit. HEAD is a pointer.**

It means:

* You are currently on the **main branch**
    
* Git should look inside `refs/heads/main` to find the latest commit
    

---

### Small Summary So Far

* `HEAD` → tells Git *where to look*
    
* It points to a branch, not directly to a commit
    
* Branch = pointer to a commit
    

## Exploring the `refs` Folder

Now let’s follow the path Git told us.

```bash
ls .git/refs
```

### Output:

```bash
heads
tags
```

### What this means

* `heads` → local branches
    
* `tags` → tags (we’ll ignore this for now)
    

---

## Inside the `heads` Folder

```bash
ls .git/refs/heads
```

### Output:

```bash
main
```

This file is named after your branch.

Now let’s read it.

```bash
cat .git/refs/heads/main
```

### Output (example):

```bash
ab12345f9c8d7e6a...
```

---

## What Is This Hash?

This long string is the **commit hash** of your current commit.

So now we know:

> **Branch → points to a commit hash**

### Mental Model (Very Important)

```bash
HEAD
 ↓
refs/heads/main
 ↓
commit hash
```

Git follows pointers step by step.

so here main is shows the hashcode of which commit we are on right now.

## Why This Hash Is Not Enough Yet

Right now, we only have:

* a commit hash
    

But a commit contains much more information:

* author name
    
* commit message
    
* timestamp
    
* parent commit
    
* tree (snapshot of files)
    

To get all this information, Git goes to the `objects` folder.

## Understanding the `objects` Folder

```bash
ls .git/objects
```

### Output (example):

```bash
09 1a ab c3 f7 ...
```

Each folder name:

* is made of **first two characters of a hash**
    

This is how Git organizes data efficiently.

---

## Finding Our Commit Object

Let’s assume our commit hash is:

```bash
ab12345f9c8d7e6a...
```

Git will:

1. Take first two characters → `ab`
    
2. Go to that folder
    

```bash
cd .git/objects/ab
ls
```

### Output:

```bash
12345f9c8d7e6a...
```

This is the **rest of the hash**.

## Reading the Commit Object (Why It Looks Weird)

Now try:

```bash
cat 12345f9c8d7e6a...
```

You’ll see **unreadable characters**.

Instead of readable text, the terminal prints something like this:

```bash
x��-K�0��QH��+J�R�W�H���/��M�+�I���
��K�U�O���H��N�W�J��
```

### Why?

Because:

* Git stores objects in **compressed binary format**
    
* This makes Git fast and space-efficient
    

Humans are not meant to read this directly.

---

## How Git Reads It (Conceptually)

Internally, Git:

* decompresses the object
    
* understands its structure
    
* extracts:
    
    * commit message
        
    * author
        
    * parent commit
        
    * tree hash
        

This is why Git commands exist—**they interpret** `.git` for us.

## Complete Flow Diagram (Beginner Mental Model) :

```bash
HEAD
 ↓
refs/heads/main
 ↓
commit hash
 ↓
.git/objects/ab/12345...
 ↓
(commit object: metadata + tree + parent)
```

## Why Git Works This Way

This design gives Git:

* fast lookups
    
* strong data integrity
    
* easy branching
    
* reliable history tracking
    

Everything is just **pointers and hashes**.

---

# Decoding Git Objects Properly with `git cat-file -p`

Earlier, we saw that directly reading files inside `.git/objects` shows **unreadable binary data**. That’s because Git stores objects in a compressed format.

So how does **Git itself** read these objects?

The answer is the `git cat-file` command.

---

## What Is `git cat-file`?

`git cat-file` is a **low-level Git command** that lets us inspect Git objects safely.

When we use:

```bash
git cat-file -p <hash>
```

Git:

1. locates the object using the hash
    
2. decompresses it
    
3. prints it in a **human-readable format**
    

---

## Example: Reading a Commit Object

Assume your current commit hash is:

```bash
ab12345f9c8d7e6a...
```

Run:

```bash
git cat-file -p ab12345
```

### Output (Readable!)

```bash
tree 98af23c1e7d4...
parent 67bc91a2d3e8...
author Karan <karan@email.com> 1700000000 +0530
committer Karan <karan@email.com> 1700000000 +0530

Add initial project files
```

---

## What This Output Means

Let’s break it down:

* `tree` → snapshot of directory structure
    
* `parent` → previous commit (history link)
    
* `author` → who wrote the code
    
* `committer` → who committed it
    
* message → commit description
    

—&gt; **This confirms:**  
A commit does NOT store files.  
It stores **references** to other objects.

---

# Understanding Blob, Tree, and Commit Objects (Visually & Practically)

To understand how **Git** works internally, you must understand **three core objects**:

1. **Blob** – stores file content
    
2. **Tree** – stores directory structure
    
3. **Commit** – connects everything together
    

Once these three click, Git internals become **simple and logical**.

---

## Our One Connected Example (We’ll Use This Throughout)

### Project Structure

```bash
project/
 ├── t1.txt
 └── t2.txt
```

### File Contents

```bash
t1.txt → Hello Git
t2.txt → Learning Git Internals
```

Now let’s see **how Git stores this internally**.

---

## 1\. Blob Object (File Content)

### What Is a Blob?

A **blob** stores **only the content of a file**.

Important:

* ❌ No file name
    
* ❌ No folder info
    
* ✅ Only raw data
    

---

### Blob Example

```bash
t1.txt content → "Hello Git"
```

Git creates:

```bash
Blob A
└── "Hello Git"
```

Similarly:

```bash
t2.txt content → "Learning Git Internals"
```

Git creates:

```bash
Blob B
└── "Learning Git Internals"
```

---

### Where Are Blobs Stored?

Inside:

```bash
.git/objects/
```

Git uses:

* first **2 characters** of hash as folder
    
* remaining characters as filename
    

Example:

```bash
.git/objects/ab/12345...
```

**Every blob is a compressed file inside** `.git/objects`.

## 2\. Tree Object (Directory Structure)

### What Is a Tree?

A **tree** represents a **directory**.

It maps:

* file names → blob hashes
    
* folder names → tree hashes
    

Think of a tree as a **directory index**.

---

### Tree Example (Root Folder)

```bash
Tree (root)
 ├── t1.txt → Blob A
 └── t2.txt → Blob B
```

This tree says:

* file name is `t1.txt`
    
* its content is stored in Blob A
    
* file name is `t2.txt`
    
* its content is stored in Blob B
    

---

### Where Are Tree Objects Stored?

Trees are also stored in:

```bash
.git/objects/
```

Just like blobs, but their content describes **structure**, not file data.

---

### How to See a Tree Object (Readable)

```bash
git cat-file -p <tree-hash>
```

Example output:

```bash
100644 blob ab12345 t1.txt
100644 blob cd67890 t2.txt
```

This literally shows:

```bash
file → blob → name
```

---

## 3\. Commit Object (The Connector)

### What Is a Commit?

A **commit** connects:

* a tree (snapshot)
    
* a parent commit (history)
    
* metadata (author, message)
    

A commit does **not store files**.

---

### Commit Object Looks Like This

```bash
commit
├── tree → root tree hash
├── parent → previous commit hash
├── author → Karan
├── date → timestamp
└── message → "Initial commit"
```

---

### Where Is Commit Stored?

Commit objects are also stored in:

```bash
.git/objects/
```

Just like blobs and trees.

---

### How to Read a Commit Object

```bash
git cat-file -p <commit-hash>
```

Output:

```bash
tree 98af23c1...
author Karan <email>
committer Karan <email>

Initial commit
```

---

### One Connected Diagram (Understand This Once)

```bash
Commit
  ↓
Tree (root directory)
  ├── t1.txt → Blob A ("Hello Git")
  └── t2.txt → Blob B ("Learning Git Internals")
```

This entire structure is **one snapshot**.

---

## What Happens When One File Changes?

Suppose we change `t1.txt`:

```bash
Hello Git v2
```

Git creates:

* ❌ Old blob unchanged
    
* ✅ New blob for new content
    
* ✅ New tree
    
* ✅ New commit
    

### Visual Comparison

#### Old Commit

```bash
Tree 1
 ├── t1.txt → Blob A
 └── t2.txt → Blob B
```

#### New Commit

```bash
Tree 2
 ├── t1.txt → Blob C (new)
 └── t2.txt → Blob B (reused)
```

**Git reuses unchanged blobs** → efficient storage.

so this is the basic overview of how git internally works there are many more things that git do but for now we understand here how git works internally if you like the blog please follow.

thanks for reading 👍👍!!
