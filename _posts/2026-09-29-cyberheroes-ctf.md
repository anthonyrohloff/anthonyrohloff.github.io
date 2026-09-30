---
layout: post
title: "CyberHeroes CTF Write-up"
date: 2026-09-29
link: https://tryhackme.com/room/cyberheroes
link_text: "CyberHeroes"
categories: ctf-writeups
---

This room provides a hint in the instructions:

*Navigate to the following URL using the AttackBox: http://10.66.159.221*

It can be deduced that the flag must be found on a web page.

---

After spinning up the machine and navigating to the website, another hint can be found on the **About** page:

*...find the vuln on our login page and login...*

The page to search is the login page. View the page source to find the credentials.

![](/assets/writeup_assets/cyberheroes_ctf_writeup/view_page_source.png)
*Figure 1: View Page Source*

The credentials can be found on line 128.

![](/assets/writeup_assets/cyberheroes_ctf_writeup/creds.png)
*Figure 2: Credentials*

The username is plainly written as `h3ck3rBoi`, but the password is passed to a `ReverseString` function. Reverse the string to get the password. The credentials are `h3ck3rBoi:SuperSecret@12345`.

The flag appears immediately after logging in.

![](/assets/writeup_assets/cyberheroes_ctf_writeup/flag.png)
*Figure 3: Flag*
