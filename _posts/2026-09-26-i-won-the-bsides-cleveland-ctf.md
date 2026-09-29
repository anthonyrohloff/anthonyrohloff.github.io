---
layout: post
title: "I Won the BSides Cleveland CTF"
date: 2026-09-26
categories: blog-posts
---

On Saturday, September 26th, my team of two won the CTF at BSides Cleveland 2026. The competition featured 16 teams, and my partner had never competed in a CTF before. There were three tracks to choose from, and I was able to take down most of the second and third tracks while helping my teammate work through the first.

![](/assets/blog_assets/bsides_cle_2026/bsides_cle_ctf.png)

The first track required testing against a Linux machine. The target had an exposed SMB service, where we found a vulnerable share named `backup`. Downloading the contents of the share revealed a list of users on the system. From there, a brute-force attack against the exposed SSH service granted access to a standard user account. The user's desktop held a clue pointing to the root password's location, which we swiftly found to root the machine.

The second track focused on a Windows host where the goal was to gain access through a vulnerable version of Oracle GlassFish. I identified CVE-2017-1000028 as the exploit vector and successfully gained filesystem access using `msfconsole`. Since the exploit provided elevated privileges out of the box, the remaining objectives fell into place quickly.

The third track targeted another Windows machine. I began by gathering exposed information on the target and noticed an HTTP service running on port 8080. The port was running an application called **SentryHD**, which allowed administrators to create privileged host accounts through the web portal. After running `dirb` against the web server, I uncovered the admin credentials and logged in. To exploit the host, I uploaded and executed a Python script via the **SentryHD** portal to create a new user account with administrator privileges. Using this new account, I completed the final box as time expired in the competition.
