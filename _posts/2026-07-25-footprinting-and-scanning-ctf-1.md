---
layout: post
title: "Footprinting and Scanning CTF 1 Write-up"
date: 2026-07-25
link: https://my.ine.com/CyberSecurity/learning-paths/61f88d91-79ff-4d8f-af68-873883dbbd8c/penetration-testing-student
link_text: "INE eJPT Course"
categories: ctf-writeups
---

# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali Linux machine. The target machine will be accessible at **http://target.ine.local**.

**Objective:** Perform reconnaissance on the target and capture all the flags hidden within the environment.

**Flags to Capture:**

- **Flag 1**: The server proudly announces its identity in every response. Look closely; you might find something unusual.
- **Flag 2**: The gatekeeper's instructions often reveal what should remain unseen. Don't forget to read between the lines.
- **Flag 3**: Anonymous access sometimes leads to forgotten treasures. Connect and explore the directory; you might stumble upon something valuable.
- **Flag 4**: A well-named database can be quite revealing. Peek at the configurations to discover the hidden treasure.

# Tools

The best tools for this lab are:

- Nmap
- FTP
- MySQL

---
### Note

In this lab, the flag will follow the format: FLAG1_MD5Hash. For example, FLAG1_0f4d0db3668dd58cabb9eb409657eaa8. You need to submit only the MD5 hash string, excluding the underscore (_). For instance: 0f4d0db3668dd58cabb9eb409657eaa8.

---
# Task 1

**Clue**: The server proudly announces its identity in every response. Look closely; you might find something unusual.

First, the IP for the lab needs to be identified. Use `ip a`, and look under `eth1`.

![](/assets/writeup_assets/footprinting_and_scanning_ctf_1_writeup/ip_a.png)
*Figure 1: Finding the IP needed for the lab*

Then, a basic Nmap scan can be performed for host discovery, `nmap [IP/CIDR]`.

![](/assets/writeup_assets/footprinting_and_scanning_ctf_1_writeup/host_discovery.png)
*Figure 2: Host discovery*

Next, an in-depth port discovery scan can be used to find the first flag in the "Server" field of the `http` service on port 80.

`nmap -sS -A -p- -T4 [IP]`
- `-sS`: TCP SYN scan
- `-A`: Enable OS detection, version detection, script scanning, and traceroute
- `-p-`: Scan all TCP ports 1-65535
- `-T4`: "aggressive" scan to improve performance

![](/assets/writeup_assets/footprinting_and_scanning_ctf_1_writeup/flag_1.png)
*Figure 3: Flag 1, located in the "Server" field of the http service*

---
# Task 2

**Clue:** The gatekeeper's instructions often reveal what should remain unseen. Don't forget to read between the lines.

"The gatekeeper" is probably the `robots.txt` file on the webserver. Navigate to `http://[IP]/robots.txt` to see what the file contains.

![](/assets/writeup_assets/footprinting_and_scanning_ctf_1_writeup/robotstxt.png)
*Figure 4: robots.txt*

The `secret-info` file looks promising. 

![](/assets/writeup_assets/footprinting_and_scanning_ctf_1_writeup/secret_info.png)
*Figure 5: secret-info*

Append `/flag.txt` to the current URL to find the second flag.

![](/assets/writeup_assets/footprinting_and_scanning_ctf_1_writeup/flag_2.png)
*Figure 6: Flag 2 in /secret-info/flag.txt*

---
# Task 3

**Clue**: Anonymous access sometimes leads to forgotten treasures. Connect and explore the directory; you might stumble upon something valuable.

In the in-depth scan used in figure 3, it can be seen that the FTP server allows for anonymous login. Additionally, there is a file called "flag.txt" on the server.

![](/assets/writeup_assets/footprinting_and_scanning_ctf_1_writeup/ftp_anon_allowed.png)
*Figure 7: The FTP server allows for anonymous login, and has flag.txt*

To connect to the server and view the file, complete the following steps:

1. Login to the ftp server with `ftp [IP]`. Enter `anonymous` for the username, and leave the password empty.
2. Use `get flag.txt -` to print the third flag to the terminal.

![](/assets/writeup_assets/footprinting_and_scanning_ctf_1_writeup/flag_3.png)
*Figure 8: Exploiting the anonymous login in the FTP server to get the third flag*

---
# Task 4

**Clue:** A well-named database can be quite revealing. Peek at the configurations to discover the hidden treasure.

The other file on the FTP server is "creds.txt". When output, it shows the credentials `db_admin:password@123`.

![](/assets/writeup_assets/footprinting_and_scanning_ctf_1_writeup/db_creds.png)
*Figure 9: Database credentials*

There is a **mysql** instance running on the target machine as well. Use `mysql -u db_admin -p -h [IP]` to connect to it. Input the password above when prompted. Flag 4 can then be seen by running `SHOW DATABASES;` into the mysql terminal.

![](/assets/writeup_assets/footprinting_and_scanning_ctf_1_writeup/flag_4.png)
*Figure 10: Flag 4 on the mysql server*
