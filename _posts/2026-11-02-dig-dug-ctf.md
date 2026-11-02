---
layout: post
title: "Dig Dug CTF Write-up"
date: 2026-11-02
link: https://tryhackme.com/room/digdug
link_text: "Dig Dug"
categories: ctf-writeups
---

This CTF instructs us to `dig` the target machine to get started.

![](/assets/writeup_assets/digdug_ctf_writeup/dig.png)
*Figure 1: dig [target IP]*

The hint also says it only responds to a request from `givemetheflag.com`. Add this URL to the `/etc/hosts` file.

![](/assets/writeup_assets/digdug_ctf_writeup/hosts.png)
*Figure 2: Updated etc/hosts*

Now, `dig` the target IP again with `dig @[target IP] givemetheflag.com`. The `@` tells `dig` to skip looking in `/etc/resolv.conf` and to treat the IP as a name server.

![](/assets/writeup_assets/digdug_ctf_writeup/flag.png)
*Figure 3: Flag*

The flag appears, so the CTF is complete!
