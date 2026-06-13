---
layout: post
title: "Crack the Hash CTF Write-up"
date: 2024-09-29
categories: ctf-writeups
---

To crack these hashes, we need to determine the type for each hash. We can use [**hashID**](https://github.com/psypanda/hashID) to identify them.

---

## Task 1:

---

Follow the instructions to install **hashID**, and run it on the first hash.

![Hash 1 identification](/assets/writeup_assets/crack-the-hash-writeup/figure_1.png "Figure 1: Hash 1 identification")
*Figure 1: Hash 1 identification*

We receive several potential hash types. Let's start testing these in **hashcat**. MD5 is a common hash type, so we'll try that first.

```bash
hashcat -a 0 '48bb6e862e54f2a795ffc4e541caed4d' -m 0 /usr/share/wordlists/rockyou.txt
```

- **`-a`**: Specifies the attack mode (0 for a straight attack)
- **`-m`**: Specifies the hash type (0 for MD5)

We need a wordlist for **hashcat** to use, so we also pass in **rockyou.txt**. 

> **Note**: Information regarding these parameters can be found by using `hashcat -h`.

**Result**: 
```
easy
```

---

Repeat this process for the second hash. The top response indicates it's a **SHA-1**.

```bash
hashcat -a 0 'CBFDAC6008F9CAB4083784CBD1874F76618D2A97' -m 100 /usr/share/wordlists/rockyou.txt
```

**Result**:
```
password123
```

---

For the third hash, the top option is **Snefru-256**, with **SHA-256** and **RIPEMD-256** following. 

![Hash 3 identification](/assets/writeup_assets/crack-the-hash-writeup/figure_2.png "Figure 2: Hash 3 identification")
*Figure 2: Hash 3 identification*

We can try **Snefru-256**, but it's likely to be **SHA-256** or **RIPEMD-256**, so we will start there. Looking through the hashcat help menu, we find that SHA-256 has the ID **1400**.

```bash
hashcat -a 0 '1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032' -m 1400 /usr/share/wordlists/rockyou.txt
```

**Result**:
```
letmein
```

---

On the next hash, we find that it could either be **Blowfish (OpenBSD)**, **Woltlab Burning Board 4.x**, or **bcrypt**. Both Blowfish and bcrypt have the ID **3200** in hashcat, so we will try that. 

Before we begin, note that this hash can take a long time to crack. However, we have a hint that the password is **4 characters long**, so let's modify **rockyou.txt** to match this specification.

![Python script to modify rockyou.txt](/assets/writeup_assets/crack-the-hash-writeup/figure_3.png "Figure 3: Python script to modify rockyou.txt")
*Figure 3: Python script to modify rockyou.txt*

This script takes **rockyou.txt** as input and writes all **4-character** lines to **rockyou_4.txt**.

Now, let's run hashcat with this modified wordlist.

```bash
hashcat -a 0 '279412f945939ba78ce0758d3fd83daa' -m 3200 rockyou_4.txt
```

**Result**: 
```
bleh
```

---

For the final hash in Task 1, we again start with **MD5**.

```bash
hashcat -a 0 '279412f945939ba78ce0758d3fd83daa' -m 0 /usr/share/wordlists/rockyou.txt
```

![MD5 fail](/assets/writeup_assets/crack-the-hash-writeup/figure_4.png "Figure 4: MD5 fail")
*Figure 4: MD5 fail*

This attempt fails, so we move on to **MD4**.

```bash
hashcat -a 0 '279412f945939ba78ce0758d3fd83daa' -m 900 /usr/share/wordlists/rockyou.txt
```

After checking online using [dcode.fr](https://www.dcode.fr/md4-hash), we confirm it is **MD4**.

**Result**: 
```
Eternity22
```

---

## Task 2:

To ensure **hashcat** works correctly, we will put each hash in a text file from now on.

![Text file and hashcat command](/assets/writeup_assets/crack-the-hash-writeup/figure_5.png "Figure 5: Text file and hashcat command")
*Figure 5: Text file and hashcat command*

```bash
hashcat -m 1400 -a 0 hash.txt rockyou.txt
```

**Result**: 
```
paule
```

---

For the next hash, we get many options. After exhausting most of the list, we find that it is an **NTLM hash**.

```bash
hashcat -m 1000 -a 0 hash.txt rockyou.txt
```

**Result**:
```
n63ump8lkf4i
```

---

For the penultimate hash, let's create a **rockyou.txt** list with only **6-character passwords**.

![Edited python script to modify rockyou.txt](/assets/writeup_assets/crack-the-hash-writeup/figure_6.png "Figure 6: Edited python script to modify rockyou.txt")
*Figure 6: Edited python script to modify rockyou.txt*

Run hashcat with the new wordlist:

```bash
hashcat -m 1800 -a 0 hash.txt rockyou_6.txt
```

**Result**: 
```
waka99
```

---

Lastly, we identify that the final salt is some kind of **SHA-1**. Consult the [hashcat wiki](https://hashcat.net/wiki/doku.php?id=example_hashes) to figure out the appropriate code to use.

![Hashcat examples](/assets/writeup_assets/crack-the-hash-writeup/figure_7.png "Figure 7: Hashcat examples")
*Figure 7: Hashcat examples*

Our format matches the highlighted line, so we will use **`-m 160`**.

```bash
hashcat -m 160 -a 0 hash.txt rockyou.txt
```

**Result**: 
```
481616481616
```

---

Congratulations! This box has been completed.

