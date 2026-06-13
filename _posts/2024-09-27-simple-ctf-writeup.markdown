---
layout: post
title: "Simple CTF Write-up"
date: 2024-09-27
categories: ctf-writeups
---

## Question 1: How many services are running under port 1000?

Run the following Nmap command:
```bash
nmap -T5 -p- -A - [target machine IP]
```
- `-T5`: Set the scan speed (1-5, with 5 being the fastest)
- `-p-`: Scan all ports
- `-A`: Enable OS detection and version detection

Optionally, you can store the output in a text file using:
```bash
-oN nmap_output.txt
```

![Nmap Scan of Target Machine](/assets/writeup_assets/simple-ctf-writeup/figure_1.png "Figure 1")
*Figure 1: Nmap Scan of Target Machine*

As we can see from the scan, there are **two ports** running under port 1000: **port 21** and **port 80**. The answer is 2.

---

## Question 2: What is running on the higher port?

By examining the Nmap scan output, we see that **SSH** is running on the higher port, which is **port 2222**.

---

## Question 3: What's the CVE you're using against the application?

Run the following Dirb command to scan directories:
```bash
dirb http://[target machine IP]
```

![Dirbuster Output](/assets/writeup_assets/simple-ctf-writeup/figure_2.png "Figure 2")
*Figure 2: Dirbuster Output*

In the output, we see there is a directory called **/simple**. By navigating to `http://[target machine IP]/simple`, we can gather more information.

![CMS Made Simple Version](/assets/writeup_assets/simple-ctf-writeup/figure_3.png "Figure 3")
*Figure 3: CMS Made Simple Version*

Scrolling to the bottom of the page, we find that the CMS is **CMS Made Simple** version **2.2.8**. 

By searching for vulnerabilities for this version, we discover the CVE to be **CVE-2019-9053**.

---

## Question 4: To what kind of vulnerability is the application vulnerable?

The application is vulnerable to an **SQL Injection (SQLI)**, as shown by the information found about the CVE.

---

## Question 5: What's the password?

In the Nmap scan (Figure 1), we saw that **anonymous login** is allowed for FTP. We log into the FTP service to investigate.

![FTP Investigation](/assets/writeup_assets/simple-ctf-writeup/figure_4.png "Figure 4")
*Figure 4: FTP Investigation*

We find that a user named **mitch** likely has a weak password. Using Hydra to crack the password:
```bash
hydra -l mitch -P [location of your rockyou.txt file] ssh://[target machine IP]:2222
```
- `-l`: Specifies the login name (in this case, mitch)
- `-P`: Points to the password list file (rockyou.txt)

![Cracked Password](/assets/writeup_assets/simple-ctf-writeup/figure_5.png "Figure 5")
*Figure 5: Cracked Password*

Almost immediately, Hydra finds that **mitch's password** is **"secret"**.

---

## Question 6: Where can you login with the details obtained?

The answer is **SSH**. You can verify it by running:
```bash
ssh mitch@[target machine IP] -p 2222
```

---

## Question 7: What's the user flag?

![User Flag](/assets/writeup_assets/simple-ctf-writeup/figure_6.png "Figure 6")
*Figure 6: User Flag*

The user flag is **"G00d J0b keep up!"**.

---

## Question 8: Is there any other user in the home directory? What's his name?

![Other User in Home Directory](/assets/writeup_assets/simple-ctf-writeup/figure_7.png "Figure 7")
*Figure 7: Other User in Home Directory*

The other user in the home directory is **sunbath**.

---

## Question 9: What can you leverage to spawn a privileged shell?

![Sudo -l Result](/assets/writeup_assets/simple-ctf-writeup/figure_8.png "Figure 8")
*Figure 8: Sudo -l Result*

We can see that **mitch can use vim with sudo**. This will be our method to escalate privileges.

---

## Question 10: What is the root flag?

Run the following command to open vim with sudo:
```bash
sudo vim
```
Once inside vim, type the following command to spawn a shell:
```
:!bash
```

Find the root flag!

![Root Flag](/assets/writeup_assets/simple-ctf-writeup/figure_9.png "Figure 9")
*Figure 9: Root Flag*

