---
layout: post
title: "Information Gathering CTF 1"
date: 2026-07-21
categories: ctf-writeups
---

# Lab Environment

A website is accessible at **http://target.ine.local**. Perform reconnaissance and capture the following flags.

- **Flag 1:** This tells search engines what to and what not to avoid.
- **Flag 2:** What website is running on the target, and what is its version?
- **Flag 3:** Directory browsing might reveal where files are stored.
- **Flag 4:** An overlooked backup file in the webroot can be problematic if it reveals sensitive configuration details.
- **Flag 5:** Certain files may reveal something interesting when mirrored.

# Tools

- Firefox
- Curl
- HTTrack

---

### Note

In this lab, the flag will follow the format: FLAG1{MD5Hash} OR FL@G1{MD5Hash}. For example, FLAG1{0f4d0db3668dd58cabb9eb409657eaa8}. You need to submit only the MD5 hash string, excluding the braces. For instance: 0f4d0db3668dd58cabb9eb409657eaa8.


---
# Task 1

**Clue: This tells search engines what to and what not to avoid.**

To find this flag, simply navigate to **http://target.ine.local/robots.txt**.

![First flag](/assets/writeup_assets/info-gathering-ctf-writeup/info_ctf_flag_1.png "Figure 1: Finding the first flag in robots.txt")

*Figure 1: Finding the first flag in robots.txt*

---
# Task 2

**Clue: What website is running on the target, and what is its version?**

The `whatweb` tool can be used to find this flag.

`whatweb http://target.ine.local`

![Second flag](/assets/writeup_assets/info-gathering-ctf-writeup/info_ctf_flag_2.png "Figure 2: Finding the second flag with whatweb")
*Figure 2: Finding the second flag with whatweb*

---
# Task 3

**Clue: Directory browsing might reveal where files are stored.**

The `dirb` tool can be used to browse directories.

`dirb http://target.ine.local`

At the end of the `dirb` scan, an interesting directory `/wp-content/uploads` can be found.

![Third flag investigation](/assets/writeup_assets/info-gathering-ctf-writeup/info_ctf_task_3.png "Figure 3: Identifying where to look for the third flag")

*Figure 3: Identifying where to look for third flag*



![Third flag](/assets/writeup_assets/info-gathering-ctf-writeup/info_ctf_flag_3.png "Figure 4: Finding third flag in /wp-content/uploads")


*Figure 4: Finding third flag in /wp-content/uploads*

---
# Task 4

**Clue: An overlooked backup file in the webroot can be problematic if it reveals sensitive configuration details**

To search for common backup file extensions like `.bak` and `.zip`, use `dirb` with the `-X` flag.

`dirb http://target.ine.local -X .bak,.tar.gz,.zip,.sql,.bak.zip`

![Fourth flag investigation](/assets/writeup_assets/info-gathering-ctf-writeup/info_ctf_task_4.png "Figure 5: Finding the backup file")
*Figure 5: Finding the backup file*

Then, `curl` any interesting results.

![Fourth flag](/assets/writeup_assets/info-gathering-ctf-writeup/info_ctf_flag_4.png "Figure 6: Finding the fourth flag with curl")

*Figure 6: Finding the fourth flag with curl*

---
# Task 5

**Clue: Certain files may reveal something interesting when mirrored.**

First, use `httrack` to mirror the site onto the local machine.

`httrack http://target.ine.local --mirror`

![Fifth flag investigation](/assets/writeup_assets/info-gathering-ctf-writeup/httrack_mirroring.png "Figure 7: Using httrack")
*Figure 7: Using httrack*

Then, enter the directory `httrack` created, and `cat` out the `xmlrpx0db0.php` file to find the flag.

![Fifth flag](/assets/writeup_assets/info-gathering-ctf-writeup/info_ctf_flag_5.png "Figure 8: Finding the last flag from the mirrored site")
*Figure 8: Finding the last flag from the mirrored site*
