# macOS Terminal Basics — A Complete Reference

---

## 1. The Terminal & Bash

### What is Bash?

**Bash** = **B**ourne **A**gain **Sh**ell

A **command interpreter** — a program that lets you talk to your computer by typing text commands instead of clicking around. When you open Terminal on Mac, you're inside a shell.

- Think of it as a **text-based control panel** for your computer
- macOS used Bash as default until 2019, then switched to **Zsh**
- They're nearly identical for everyday use

### Opening Terminal & Bash Commands

```bash
# Open Terminal: Command + Space → type "Terminal" → Enter

# Switch to Bash temporarily
bash

# Set Bash as your default shell permanently
chsh -s /bin/bash

# Check which shell you're currently in
echo $SHELL

# Check Bash version
bash --version
```

---

## 2. Key Tools Explained

### What is chsh?

**chsh** = **Ch**ange **Sh**ell

Changes your **default shell** — which shell opens automatically every time you launch Terminal.

```bash
chsh -s /bin/bash    # set Bash as default
chsh -s /bin/zsh     # set Zsh as default
```

- The `-s` flag means "set shell to this path"
- Takes effect the next time you open Terminal
- `/bin/bash` and `/bin/zsh` are just file locations of those programs on your Mac

### What is Homebrew (brew)?

**Homebrew** is a **package manager** for macOS — an app store for the Terminal. Installs developer tools that macOS doesn't ship with.

```bash
brew install git        # installs Git
brew install python     # installs Python
brew install bash       # installs newer version of Bash
```

- Download and install from [brew.sh](https://brew.sh)
- Mac/Linux only
- You'll use it constantly as a developer

### How They Connect

| Component | Role |
|---|---|
| Terminal | The window |
| Bash / Zsh | The language spoken inside that window |
| chsh | How you choose which language is default |
| Homebrew | How you install new tools to use in that language |

---

## 3. Shell Scripts (.sh files)

### What is a `.sh` file?

A `.sh` file is a **shell script** — a plain text file containing a list of terminal commands, saved so you can run them all at once.

Example `setup.sh`:
```bash
echo "Hello, Basil"
mkdir my-project
cd my-project
git init
```

Instead of typing these 4 commands one by one, you run the whole file in one shot.

- `.sh` = "shell script" (just a file extension)
- Open it in any text editor — it's plain text
- Think of it as a **recipe of terminal commands**

### Running a Shell Script

```bash
bash somefile.sh     # run with Bash
zsh somefile.sh      # run with Zsh
```

The pattern is always: `<interpreter> <file>`

Even if you're in Zsh, typing `bash somefile.sh` launches Bash just for that file, runs every line top to bottom, then exits. Your Zsh session is unaffected.

```
You (in Zsh) → call bash → bash reads file → runs commands → bash exits → back to Zsh
```

### The Shebang (`#!`)

Instead of specifying the interpreter every time, add a **shebang** as the first line:

```bash
#!/bin/bash
echo "Hello, Basil"
mkdir my-project
```

The `#!/bin/bash` tells the OS which program should run the file — so you can just do:

```bash
./somefile.sh        # runs itself, no need to type "bash"
```

---

## 4. Bash vs Zsh

The honest answer: **for everyday use, barely any difference.** 95% of commands work identically in both.

### Key Differences

| Feature | Bash | Zsh |
|---|---|---|
| macOS default | Until 2018 | 2019–now |
| Autocomplete | Basic (filenames only) | Smart (flags, menus, cycling) |
| Customization | Manual | Oh My Zsh ecosystem |
| History across tabs | No | Yes |
| Scripting arrays | 0-indexed | 1-indexed |
| Common on servers | Extremely | Less common |

### Autocomplete Example

```bash
cd Doc[TAB]          # both work the same
git commit --[TAB]   # Zsh shows all flags; Bash doesn't
```

### Practical Advice

- **Day to day** → stick with Zsh (Mac default)
- **Writing scripts** → use `#!/bin/bash` because servers almost always run Bash — keeps scripts portable
- **At Amazon** → servers run Bash. Bash fluency matters more professionally

---

## 5. pip vs brew

Both are **package managers** — tools that download and install software. But they manage very different things.

### pip

**pip** = **P**ip **I**nstalls **P**ackages

Installs **Python libraries only.**

```bash
pip install numpy        # math library
pip install pandas       # data manipulation
pip install fastapi      # web framework
```

- Only speaks Python
- Installs packages your Python code imports
- Comes built-in with Python
- Works on Windows, Mac, Linux

### brew

Installs **entire programs and system tools.**

```bash
brew install python      # the Python language itself
brew install git         # version control
brew install node        # JavaScript runtime
```

- Mac/Linux only
- System-level software
- Must be installed separately

### Side by Side

| | pip | brew |
|---|---|---|
| What it installs | Python packages only | Any software/tool |
| Who uses it | Python developers | Any Mac/Linux user |
| Example | `pip install requests` | `brew install wget` |
| Built into | Python | Must install separately |
| Platform | Cross-platform | Mac/Linux |

### How They Work Together

```bash
brew install python      # brew installs Python itself
pip install pandas       # pip installs libraries into that Python
```

Brew sets up the tools; pip stocks them with Python packages.

### Package Managers Across Platforms

| Purpose | Windows | Mac | Linux |
|---|---|---|---|
| System tools | Chocolatey / winget | Homebrew | apt / yum |
| Python packages | pip | pip | pip |
| JS packages | npm | npm | npm |
| Java packages | Maven | Maven | Maven |

---

## 6. sudo

**sudo** = **S**uper **U**ser **DO**

Runs a command as an **administrator.** Mac asks for your login password before proceeding.

```bash
sudo mkdir /etc/myconfig     # create folder in a protected location
```

### Why It Exists

Your computer has two modes:

**Normal user mode** (always in this by default)
- Can create files in your own folders
- Cannot touch system files

**Superuser / Root mode** (all-powerful admin)
- Can read, modify, delete anything on the system
- No restrictions

sudo lets you **temporarily jump to root** for just one command, then drops back to normal.

### Real World Analogy

- **You** = employee with a keycard to your own floor
- **sudo** = temporarily borrowing the master key
- **Root** = security guard who can open every door

### Example

```bash
mkdir /etc/myfolder
# ❌ Permission denied

sudo mkdir /etc/myfolder
# Password: ••••••••
# ✅ Works
```

### Why Not Always Use sudo?

Because root is **dangerous:**

```bash
sudo rm -rf /     # deletes your entire operating system
```

A typo in normal mode deletes a file. A typo with sudo can **destroy your entire system.**

### When to Use sudo

| Situation | Use sudo? |
|---|---|
| Normal commands, your own files | No |
| Installation failing with "permission denied" | Yes |
| Modifying system folders (`/etc`, `/usr`) | Yes |
| Running your own Python scripts | No |
| Random internet command tells you to sudo | **Be very careful** |

That last row matters — a common attack is tricking people into running malicious commands with sudo. At Amazon this is called **privilege escalation.** Never sudo something you don't understand.

---

## Quick Reference Cheatsheet

```bash
# Shell management
echo $SHELL              # check current shell
chsh -s /bin/bash        # change default shell to Bash
chsh -s /bin/zsh         # change default shell to Zsh

# Running scripts
bash somefile.sh         # run script with Bash
./somefile.sh            # run self-executable script (needs shebang)

# Package managers
brew install <tool>      # install a system tool
pip install <package>    # install a Python library

# Permissions
sudo <command>           # run command as administrator
```
