---
layout: post
title: "Web Application Penetration Testing CTF 1 Write-up"
date: 2026-09-07
link: https://my.ine.com/CyberSecurity/learning-paths/61f88d91-79ff-4d8f-af68-873883dbbd8c/penetration-testing-student
link_text: "INE eJPT Course"
categories: ctf-writeups
---
# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali Linux machine. The target website is accessible at **http://target.ine.local**.

**Objective**: Identify web application vulnerabilities in the target website and capture all the flags hidden within the environment.

**Useful wordlists:**

```
/usr/share/wordlists/dirb/common.txt 
/usr/share/seclists/Usernames/top-usernames-shortlist.txt 
/root/Desktop/wordlists/100-common-passwords.txt
```

**Flags to Capture:**

- **Flag 1:** Sometimes, important files are hidden in plain sight. Check the root ('/') directory for a file named 'flag.txt' that might hold the key to the first flag.
- **Flag 2:** Explore the structure of the server's directories. Enumeration might reveal hidden treasures.
- **Flag 3:** The login form seems a bit weak. Trying out different combinations might just reveal the next flag.
- **Flag 4:** The login form behaves oddly with unexpected inputs. Think of injection techniques to access the 'admin' account and find the flag.

# Tools

The best tools for this lab are:

- Nmap
- Gobuster
- Hydra

---

### Note

In this lab, the flag will follow the format: FLAG1_MD5Hash. For example, FLAG1_0f4d0db3668dd58cabb9eb409657eaa8. You need to submit only the MD5 hash string, excluding the underscore (_). For instance: 0f4d0db3668dd58cabb9eb409657eaa8.

---
# Task 1

**Clue:** Sometimes, important files are hidden in plain sight. Check the root ('/') directory for a file named 'flag.txt' that might hold the key to the first flag.

Use the `/view_file` path to navigate to the root directory and get the flag. Set the `file` parameter to `../../../flag.txt`.

![](/assets/writeup_assets/web_application_penetration_testing_ctf_1_writeup/flag1.png)
*Figure 1: Flag 1*

Flag 1 is `c64cac1177724beb9221c07fb256f983`.

---
# Task 2

**Clue:** Explore the structure of the server's directories. Enumeration might reveal hidden treasures.

Use `dirb target.ine.local` to brute-force common paths.

![](/assets/writeup_assets/web_application_penetration_testing_ctf_1_writeup/dirb.png)
*Figure 2: Dirb tool found /secured path*

The `/secured` path looks interesting. Investigate it.

![](/assets/writeup_assets/web_application_penetration_testing_ctf_1_writeup/secured.png)
*Figure 3: Secured path*

The path has `"flag.txt"` inside of it. Append `/flag.txt` to the end of the current path to get flag 2.

![](/assets/writeup_assets/web_application_penetration_testing_ctf_1_writeup/flag2.png)
*Figure 4: Flag 2*

Flag 2 is `ce1f504af60e4595a1cd4c0943c630e9`.

---
# Task 3

**Clue:** The login form seems a bit weak. Trying out different combinations might just reveal the next flag.

Use **Hydra** to brute-force the login.

`hydra -L /usr/share/seclists/Usernames/top-usernames-shortlist.txt -P /root/Desktop/wordlists/100-common-passwords.txt target.ine.local http-post-form "/login:username=^USER^&password=^PASS^:Invalid username or password"`

![](/assets/writeup_assets/web_application_penetration_testing_ctf_1_writeup/guest_creds.png)
*Figure 5: Guest's credentials found using Hydra*

The credentials are `guest:butterfly1`. Login using the web form.

![](/assets/writeup_assets/web_application_penetration_testing_ctf_1_writeup/flag3.png)
*Figure 6: Flag 3*

The 3rd flag is `9936bfd2c1e2471285952d23c0ec5ce3`.

---
# Task 4

**Clue:** The login form behaves oddly with unexpected inputs. Think of injection techniques to access the 'admin' account and find the flag.

SQL injection can be used to bypass the authentication on the login page. Enter `admin'--` as the username and anything as the password to find the final flag.

![](/assets/writeup_assets/web_application_penetration_testing_ctf_1_writeup/flag4.png)
*Figure 7: Flag 4*

The final flag is `86d9083285a84686b1857a2aa376076e`.
