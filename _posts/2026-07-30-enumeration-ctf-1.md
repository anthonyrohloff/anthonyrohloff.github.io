---
layout: post
title: "Enumeration CTF 1 Write-up"
date: 2026-07-30
link: https://my.ine.com/CyberSecurity/learning-paths/61f88d91-79ff-4d8f-af68-873883dbbd8c/penetration-testing-student
link_text: "INE eJPT Course"
categories: ctf-writeups
---
# Lab Environment

A Linux machine is accessible at **target.ine.local**. Identify the services running on the machine and capture the flags. The flag is an md5 hash format.

- **Flag 1:** There is a samba share that allows anonymous access. Wonder what's in there!
- **Flag 2:** One of the samba users have a bad password. Their private share with the same name as their username is at risk!
- **Flag 3:** Follow the hint given in the previous flag to uncover this one.
- **Flag 4:** This is a warning meant to deter unauthorized users from logging in.

**Note:** The wordlists located in the following directory will be useful:

- `/root/Desktop/wordlists`

# Tools

- Nmap
- Metasploit
- Hydra
- enum4linux
- smbclient
- smbmap

---

### Note

In this lab, the flag will follow the format: FLAG1{MD5Hash}. For example, FLAG1{0f4d0db3668dd58cabb9eb409657eaa8}. You need to submit only the MD5 hash string, excluding the braces. For instance: 0f4d0db3668dd58cabb9eb409657eaa8.

---
# Task 1

**Clue:** There is a samba share that allows anonymous access. Wonder what's in there!

The first step is to run an Nmap scan.

`nmap -sS -A -p- -T4 target.ine.local`
- `-sS`: runs a TCP SYN scan
- `-A`: enables OS detection, version detection, script scanning, and traceroute
- `-p-`: sets the port range from 1-65535
- `-T4`: sets the aggressiveness of the scan to a 4 out of 5

![](/assets/writeup_assets/enumeration_ctf_1_writeup/nmap.png)
*Figure 1: Nmap scan showing running services and open ports*

The result of the scan shows there is indeed a **samba** service running on ports 139 and 445. Now the service should be enumerated.

![](/assets/writeup_assets/enumeration_ctf_1_writeup/smb_enum.png)
*Figure 2: Initial Samba enumeration (failure)*

The only two shares able to be seen are `print$` and `IPC$`, seen in command 1. The former does not allow anonymous login, seen in command 2. The latter allows login, but gives an error when trying to list files, seen in command 3. The share containing the flag is likely hidden.

The next step is to use the provided `shares.txt` wordlist to brute-force the name of the share. A custom bash script is one method of completing this task:

```
#!/bin/bash

for share_name in $(cat "/root/Desktop/wordlists/shares.txt"); do
        smbclient //target.ine.local/$share_name -N
done
```

This script works by parsing through each line of `shares.txt`, and using the acquired line in `smbclient //target.ine.local/$share_name` in place of `$share_name` until one returns a successful connection.

![](/assets/writeup_assets/enumeration_ctf_1_writeup/flag1.png)
*Figure 3: Using the script, finding the hidden directory, and locating the first flag*

---
# Task 2

**Clue:** One of the samba users have a bad password. Their private share with the same name as their username is at risk!

The `enum4linux` tool can enumerate the users.

![](/assets/writeup_assets/enumeration_ctf_1_writeup/users.png)
*Figure 4: Users*

There are 4 users: **josh, bob, nancy,** and **alice**. Use **metasploit** to search for a module that can help brute-force these passwords.

`search type:auxiliary name:smb`

![](/assets/writeup_assets/enumeration_ctf_1_writeup/msf_search.png)
*Figure 5: MSF console search to find SMB brute-force module*

Now, the options need to be configured:
- `set RHOSTS target.ine.local`
- `set SMBUser alice`
- `set PASS_FILE /root/Desktop/wordlists/unix_passwords.txt`

![](/assets/writeup_assets/enumeration_ctf_1_writeup/msf_options.png)
*Figure 6: Setting options for smb_login module*

Next, run the module.

![](/assets/writeup_assets/enumeration_ctf_1_writeup/alice_creds.png)
*Figure 7: Alice's credentials*

Alice's credentials are `alice:admin`.

![](/assets/writeup_assets/enumeration_ctf_1_writeup/josh_creds.png)
*Figure 8: Josh's credentials*

Josh's credentials are `josh:purple`

Log in to each user's personal share to find the second flag.

![](/assets/writeup_assets/enumeration_ctf_1_writeup/flag2.png)
*Figure 9: Finding flag 2 under josh*

Alice's login throws an error. Josh's shows the second flag.

---
# Task 3

**Clue:** Follow the hint given in the previous flag to uncover this one.

In the bottom of figure 9, the message "**Psst! I heard there is an FTP service running. Find it and check the banner.**" This is the clue for task 3.

In the original Nmap scan in figure 1, an **FTP** service can be seen running on **port 5554**. Try to connect to it.

![](/assets/writeup_assets/enumeration_ctf_1_writeup/ftp_banner.png)
*Figure 10: FTP banner*

The banner notes users **ashley, alice,** and **amanda** need to change their weak passwords. Use `hydra` to test each user:

`hydra -l [USER] -P /root/Desktop/wordlists/unix_passwords.txt -s 5554 target.ine.local ftp`

![](/assets/writeup_assets/enumeration_ctf_1_writeup/alice_ftp_creds.png)
*Figure 11: Alice's FTP credentials*

Alice's credentials are `alice:pretty`. Use them to get the third flag.

![](/assets/writeup_assets/enumeration_ctf_1_writeup/flag3.png)
*Figure 12: Flag 3*

---
# Task 4

**Clue:** This is a warning meant to deter unauthorized users from logging in.

Figure 1 shows **port 22** running **SSH**. Try to login.

`ssh target.ine.local`

![](/assets/writeup_assets/enumeration_ctf_1_writeup/flag4.png)
*Figure 13: Flag 4*

Flag 4 appears immediately.
