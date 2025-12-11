![](assets/Pasted%20image%2020251211104338.png)

This Challenge is basically teaches the fundamentals of linux. 
## Learning Objectives

- Learn the basics of the Linux command-line interface (CLI)
- Explore its use for personal objectives and IT administration
- Apply your knowledge to unveil the Christmas mysteries


---

# **Working With the Linux CLI 

When you connect to the machine, there’s no GUI — just a terminal.  
But that’s the whole point: Linux servers are meant to be controlled through the command line.

Here are the core things you do in this task:

---

## **1. Run Basic Commands**

You start by testing a simple command:

```
echo "The Terminal is So Cool!"
```

`echo` prints out whatever the input is 

Then you check what files exist:

```
ls
```

`ls` = listing directory 

You’ll see things like `README.txt` and the `Guides` folder.

To read the README:

![](assets/Pasted%20image%2020251211105842.png)

This reveals that McSkidy hid a security guide somewhere.

---

## **2. Navigating Directories**

Move into the Guides directory:

```
cd Guides
```

`cd` = Change Directory. It is always used to change from one directory( Folder) to another 

Check what’s inside:

```
ls
```

Looks empty… but that’s the trick.

---

## **3. Finding Hidden Files**

Linux hides files that start with a dot (`.`).  
To see everything including hidden stuff  use:

```
ls -la
```

`-la` = list all directory ( including hidden directories )

Now `.guide.txt` shows up.

Read it:

```
cat .guide.txt
```

`cat` = this command is used to print or to see contents from a file

This guide tells you to check logs and search for anything related to “eggs”.

![](assets/Pasted%20image%2020251211110116.png)



---

## **4. Checking Log Files**

Move into the log directory:

```
cd /var/log
```

`/var/log` is the standard directory in Linux where system and application logs are stored.  
It contains records of system events, user activity, errors, and security-related events.  
Logs here are essential for troubleshooting, monitoring, and investigating security incidents.

McSkidy hinted at failed logins, so you search inside `auth.log`:

```
grep "Failed password" auth.log
```

This shows multiple failed SSH attempts from _HopSec_ — suspicious.


![](assets/Pasted%20image%2020251211110514.png)


- `grep` is a command-line tool used to **search for specific text patterns inside files**.  
    Here, it filters all lines containing the string `"Failed password"` in `auth.log`.  
    This helps quickly identify failed login attempts instead of scrolling through the entire file manually.
    
    You can learn more about `grep` here: [GNU Grep Documentation](https://www.gnu.org/software/grep/manual/grep.html)

---

## **5. Searching for Malicious Files**

The attackers might’ve dropped malware in the SOC‑mas account.  
Search for anything containing “egg”:

```
find /home/socmas -name *egg*
```

This reveals the malicious script `eggstrike.sh`.

- `find` is a Linux command used to **search for files and directories** based on name, type, size, modification date, and other criteria.
    
- Here, `-name *egg*` tells `find` to look for any file or directory that has “egg” in its name.
    
- This is useful for locating hidden or suspicious files left by attackers.
    

Learn more about `find` here: [GNU Find Documentation](https://www.gnu.org/software/findutils/manual/html_mono/find.html)

---

## **6. Analyzing the Eggstrike Script**

Go to the directory:

![](assets/Pasted%20image%2020251211110944.png)
The script:

- Reads `wishlist.txt`
    
- Sorts and extracts unique items
    
- Sends them to `/tmp/dump.txt`
    
- Deletes the original Christmas wishlist
    
- Replaces it with a malicious “eastmas” version
    

In short: **the script sabotages Christmas orders**.


---

## **7. Important Shell Features Used**

- **`|` (pipe)**: Sends the output of one command into another command.  
    Example: `cat file.txt | sort`  
    Learn more about pipes
    
- **`>`**: Overwrites a file with new output.  
    Example: `echo "Hello" > file.txt`
    
- **`>>`**: Appends output to the end of a file.  
    Example: `echo "Hello again" >> file.txt`
    
- **`rm`**: Deletes files.  
    Example: `rm file.txt`
    
- **`mv`**: Renames or moves files/directories.  
    Example: `mv oldname.txt newname.txt`
    

For a general guide on these and other basic Linux operations: [Linux Command Line Basics](https://linuxcommand.org/lc3_learning_the_shell.php)

---

## 8. Root 

During the challenge, we also touched deeper system‑level stuff like **root access**, **protected files**, and **command history**. Here’s the quick breakdown:

### **`cat /etc/shadow`**

- Tries to read the file that stores **hashed passwords** for all system users.
    
- Normal users **cannot** read it → _Permission denied_.
    
- Only **root** can touch this file.
    

🔗 Learn `cat`:  
[https://man7.org/linux/man-pages/man1/cat.1.html](https://man7.org/linux/man-pages/man1/cat.1.html)

---

### **Root User (`sudo su`, `whoami`, `exit`)**

- **Root** = the god‑mode user of Linux. Unlimited control.
    
- `sudo su` → switch to root.
    
- `whoami` → check which user you’re currently running as.
    
- `exit` → go back to the previous user.
    

🔗 Learn `sudo`:  
[https://man7.org/linux/man-pages/man8/sudo.8.html](https://man7.org/linux/man-pages/man8/sudo.8.html)

---

### **Bash History (`history`, `.bash_history`)**

- Linux automatically logs your commands in `~/.bash_history`.
    
- `history` → shows a numbered list of all commands you’ve run.
    
- You can also `cat ~/.bash_history` to see the raw file.
    

This is super useful in forensics — attackers forget that **their mistakes stay in the history**.

🔗 Learn `history`:  
https://www.gnu.org/software/bash/manual/bash.html#Bash-History-Facilities


![](assets/Pasted%20image%2020251211113055.png)

---
## **Final Thoughts**

This challenge highlighted the power of the Linux command-line interface. By combining basic navigation commands like `ls`, `cd`, and `pwd` with file inspection tools such as `cat` and `grep`, we were able to uncover hidden files and investigate system logs. The `find` command helped locate suspicious files, while pipes (`|`), redirection (`>` and `>>`), and file management commands (`rm`, `mv`) allowed us to process and manipulate data efficiently. Switching to the root user with `sudo su` and checking command history with `history` gave further insight into system activity. Overall, mastering these commands and their combinations is essential for effective system analysis and incident response.


