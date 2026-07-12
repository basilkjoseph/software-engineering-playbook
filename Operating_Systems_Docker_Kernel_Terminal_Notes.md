# Operating Systems, Python Virtual Environments, Docker, Kernel, Terminal & CLI Notes

## 1. Python Virtual Environments (`venv`)

A Python virtual environment is an **isolated Python installation for a
single project**.

Normally:

``` bash
pip install yt-dlp
```

installs packages into the system Python, making them available to all
Python projects.

Instead:

``` bash
python3 -m venv .venv
source .venv/bin/activate
pip install yt-dlp
```

creates a project-local Python environment.

Typical structure:

``` text
project/
├── .venv/
│   ├── bin/
│   ├── lib/
│   └── ...
└── script.py
```

While activated, both `python` and `pip` refer to the versions inside
`.venv`.

Deactivate:

``` bash
deactivate
```

Delete `.venv` and every installed package disappears.

### Why use virtual environments?

Different projects can require different package versions.

Example:

``` text
Project A
 └── .venv
      └── numpy 1.26

Project B
 └── .venv
      └── numpy 2.0
```

No conflicts occur.

------------------------------------------------------------------------

## 2. Docker

Docker isolates **an entire application environment**, not just Python
packages.

A container can include:

-   Linux userspace
-   Python
-   Java
-   Node.js
-   ffmpeg
-   PostgreSQL
-   Redis
-   Your application

Conceptually:

``` text
Container
├── Linux userspace
├── Python
├── yt-dlp
├── ffmpeg
├── Your code
└── Libraries
```

------------------------------------------------------------------------

## 3. Virtual Environment vs Docker

Virtual environment solves:

> "I don't want Python packages from one project affecting another."

Docker solves:

> "I don't want one application's environment affecting another."

Docker provides much broader isolation.

------------------------------------------------------------------------

## 4. Analogy

### System Python

Everyone shares one kitchen.

### Virtual Environment

Each apartment has its own kitchen, but everyone still lives in the same
building.

### Docker

Each person has their own tiny house with almost everything separated.

------------------------------------------------------------------------

## 5. Can you use a virtual environment inside Docker?

Yes.

``` text
Docker Container
    ↓
Linux
    ↓
Python
    ↓
Virtual Environment
    ↓
Python Packages
```

Many real-world projects do exactly this.

------------------------------------------------------------------------

# Understanding Docker Internals

## Does Docker run only in the cloud?

No.

Docker runs perfectly on your own laptop.

Cloud providers also use Docker, but it is not cloud-specific.

------------------------------------------------------------------------

## Does every container have its own operating system?

Not exactly.

Containers **share the host kernel**.

On Linux:

``` text
Linux Kernel
├── Container A
├── Container B
└── Container C
```

Each container behaves independently while sharing the same kernel.

------------------------------------------------------------------------

## Why isn't this wasteful?

Docker uses a **layered filesystem**.

Example:

``` text
Ubuntu Base Layer (shared)
        │
   ┌────┴────┐
   │         │
Python    Node.js
   │         │
App A     App B
```

The Ubuntu files exist only once.

Containers store only their own modifications in writable layers.

------------------------------------------------------------------------

## Google Docs analogy

Think of many people viewing one shared document.

Everyone references the same original.

Only their personal edits are stored separately.

Docker works similarly.

------------------------------------------------------------------------

## What makes containers separate?

Each container has its own:

-   File system view
-   Processes
-   Network stack
-   Environment variables
-   Installed applications
-   CPU limits
-   Memory limits

Deleting a file inside one container does not delete it in another.

------------------------------------------------------------------------

## Docker vs Virtual Machine

### Virtual Machines

``` text
Laptop
├── VM 1
│   ├── Linux Kernel
│   └── Ubuntu
├── VM 2
│   ├── Linux Kernel
│   └── Ubuntu
```

Each VM contains its own kernel.

------------------------------------------------------------------------

### Docker Containers

``` text
Laptop
└── Linux Kernel
    ├── Container A
    ├── Container B
    └── Container C
```

Containers share the kernel, making them lightweight and fast.

------------------------------------------------------------------------

## Why companies like Docker

Instead of every developer installing identical tools manually, Docker
ensures everyone runs the same environment.

It eliminates the classic problem:

> "It works on my machine."

------------------------------------------------------------------------

# Understanding the Kernel

Imagine a computer containing only hardware:

-   CPU
-   RAM
-   SSD
-   Keyboard
-   Display
-   Wi-Fi

Applications cannot directly control hardware.

The **kernel** acts as the middleman.

------------------------------------------------------------------------

## What is the Kernel?

The kernel is the **core component of an operating system**.

It starts during boot and stays running until shutdown.

Examples:

Saving a file:

``` text
VS Code
   ↓
Kernel
   ↓
SSD
```

Allocating memory:

``` text
Chrome
   ↓
Kernel
   ↓
RAM
```

Playing music:

``` text
Spotify
   ↓
Kernel
   ↓
Speakers
```

Applications always ask the kernel to access hardware.

------------------------------------------------------------------------

## Operating System vs Kernel

An operating system consists of:

``` text
Operating System
├── Kernel
├── System libraries
├── Shell
├── Terminal commands
├── File manager
├── Network tools
└── Other utilities
```

The kernel is only one (but the most important) part.

------------------------------------------------------------------------

## What is Linux?

Strictly speaking, **Linux is the kernel**.

Distributions such as Ubuntu, Debian, Fedora and Arch combine the Linux
kernel with additional software.

``` text
Linux Kernel
        │
 ┌──────┼──────┐
 │      │      │
Ubuntu Debian Fedora
```

------------------------------------------------------------------------

## macOS

macOS uses Apple's **XNU** kernel.

``` text
macOS
├── XNU Kernel
├── Finder
├── Terminal
├── Safari
└── Apple utilities
```

------------------------------------------------------------------------

## Windows

``` text
Windows
├── Windows NT Kernel
├── Explorer
├── Command Prompt
├── PowerShell
└── Windows utilities
```

------------------------------------------------------------------------

## Why Docker cares about the kernel

Containers share the host kernel.

Linux host:

``` text
Linux Kernel
├── Container A
├── Container B
└── Container C
```

On macOS, Docker Desktop runs a small Linux virtual machine because
Linux containers require a Linux kernel.

------------------------------------------------------------------------

## Office analogy

-   Hardware = Office building
-   Kernel = Building manager
-   Operating System = Entire building and facilities
-   Applications = Employees

Employees never directly control electricity or locks.

The building manager does.

------------------------------------------------------------------------

# Terminal, Shell and CLI

Suppose you type:

``` bash
mkdir photos
```

The flow is:

``` text
You
 ↓
Terminal
 ↓
Shell
 ↓
Kernel
 ↓
SSD
```

------------------------------------------------------------------------

## Terminal

The terminal is simply the window where you type commands.

It is just the interface.

It forwards your text to the shell.

------------------------------------------------------------------------

## Shell

The shell interprets commands.

Example:

``` bash
mkdir photos
```

The shell understands the command and asks the kernel to create the
directory.

------------------------------------------------------------------------

## CLI

CLI means **Command-Line Interface**.

Examples:

``` bash
cd Downloads
ls
mkdir project
cp file.txt backup.txt
git status
python app.py
```

------------------------------------------------------------------------

## GUI

GUI means Graphical User Interface.

Creating a folder using Finder or File Explorer is the graphical
equivalent of:

``` bash
mkdir project
```

Both ultimately ask the kernel to create a directory.

------------------------------------------------------------------------

## Why developers love CLIs

### Speed

``` bash
mkdir backend frontend docs tests scripts
```

One command replaces many mouse clicks.

------------------------------------------------------------------------

### Automation

Scripts automate repetitive work.

Example:

``` bash
#!/bin/bash

mkdir project
cd project
git init
python3 -m venv .venv
```

------------------------------------------------------------------------

### Remote servers

Most production Linux servers have no graphical interface.

Developers connect using:

``` bash
ssh user@server
```

Everything is done through the CLI.

------------------------------------------------------------------------

### Precision

Renaming thousands of files becomes easy:

``` bash
for file in *.jpg; do
    mv "$file" "vacation_$file"
done
```

------------------------------------------------------------------------

## Where does the kernel fit?

Example:

``` bash
cat notes.txt
```

Execution:

``` text
You
 │
 ▼
Terminal
 │
 ▼
Shell
 │
 ▼
Kernel
 │
 ▼
SSD
 │
 ▼
Kernel
 │
 ▼
Shell
 │
 ▼
Terminal
 │
 ▼
You
```

Only the kernel communicates directly with hardware.

------------------------------------------------------------------------

## Why VS Code includes a terminal

Developers constantly run:

-   git
-   python
-   docker
-   npm
-   java
-   gradle
-   kubectl

VS Code embeds a terminal so these tools can be used without leaving the
editor.

------------------------------------------------------------------------

## Restaurant analogy

-   You = Customer
-   Terminal = Waiter
-   Shell = Waiter interpreting the order
-   Kernel = Kitchen manager
-   Hardware = Kitchen and chefs

The customer never cooks directly.

Similarly, applications never directly control hardware.

------------------------------------------------------------------------

## Terminal vs Shell

They are different.

**Terminal** - The application/window you type into.

**Shell** - The program running inside the terminal that interprets your
commands.

Examples of shells:

-   zsh
-   bash
-   fish

On modern macOS, Terminal launches the `zsh` shell by default.

------------------------------------------------------------------------

# Key Takeaways

1.  Virtual environments isolate Python packages.
2.  Docker isolates complete application environments.
3.  Containers share a kernel; they do not each contain a full operating
    system.
4.  The kernel is the bridge between software and hardware.
5.  The operating system is much larger than the kernel.
6.  Linux is technically a kernel; Linux distributions package it with
    tools to form a usable operating system.
7.  The terminal is only the interface.
8.  The shell interprets commands.
9.  The kernel performs the actual work on the hardware.
10. Developers love CLIs because they are fast, scriptable, precise, and
    indispensable for working with servers and modern development tools.
