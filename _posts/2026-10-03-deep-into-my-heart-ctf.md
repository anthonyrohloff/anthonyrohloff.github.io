---
layout: post
title: "Hidden Deep Into my Heart CTF Write-up"
date: 2026-10-03
link: https://tryhackme.com/room/lafb2026e9
link_text: "Hidden Deep Into my Heart"
categories: ctf-writeups
---

The lab instructions say the vulnerable web application is located at `http://[target ip]:5000`.

The first step is to run an Nmap scan.

`nmap -sS -A -p- -T4 [target IP]`

- `-sS`: runs a TCP SYN scan
- `-A`: enables OS detection, version detection, script scanning, and traceroute
- `-p-`: sets the port range from 1-65535
- `-T4`: sets the aggressiveness of the scan to a 4 out of 5

![](/assets/writeup_assets/deep_into_my_heart_ctf_writeup/nmap.png)
*Figure 1: Relevant part of Nmap scan*

Notice the `http-robots.txt` Nmap script found "1 disallowed entry" in the `/robots.txt` file. Go see what it is.

![](/assets/writeup_assets/deep_into_my_heart_ctf_writeup/robots.txt.png)
*Figure 2: Disallowed entry in robots.txt*

The path is `/cupids_secret_vault/`. The web page doesn't expose any information, so run `dirb` at that path:

`dirb http://[target IP]:5000/cupids_secret_vault`

![](/assets/writeup_assets/deep_into_my_heart_ctf_writeup/dirb.png)
*Figure 3: /cupids_secret_vault/administrator path found*

There is a login page at `/cupids_secret_vault/administrator`. In `/robots.txt` there was a comment `# cupid_arrow_2026!!!`. This could be a password. Try to login with some common usernames like `administrator` and `admin`.

The credentials are `admin:cupid_arrow_2026!!!`. The flag appears immediately after logging in.

![](/assets/writeup_assets/deep_into_my_heart_ctf_writeup/flag.png)
*Figure 4: Flag*
