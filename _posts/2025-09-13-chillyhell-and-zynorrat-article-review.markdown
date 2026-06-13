---
layout: post
title: "CHILLYHELL and ZynorRat Article Review"
date: 2025-10-13
categories: blog-posts
---

The CHILLYHELL backdoor and ZynorRAT malwares are dangerous pieces of code that enable
malicious actors to remotely access macOS, Windows, and Linux systems. A backdoor or RAT
evades security measures and gain unauthorized access to a computer system. They are often
difficult to detect and are used to deliver a hostile payload. A RAT, or remote access trojan, enables
a bad actor to remotely control a system through a command-and-control server. It can be thought
of as a type of backdoor. Often, an attacker will try to install a RAT on a machine to then use that
access to drop a payload, such as a keylogger or spyware.

The CHILLYHELL malware family originated from an attack on Ukrainian officials in 2022 by a
threat actor tracked by Google Mandiant as UNC4487. The backdoor, written in C++, was used to
gain access to a car insurance site that officials were required to use. Then, the MATANBUCHUS
malware, a malware-as-a-service loader, was deployed, and access to infected systems was sold at
lucrative prices. A new variant of CHILLYHELL appeared on VirusTotal in May, raising concerns
among Apple engineers.

The backdoor works by attempting to establish persistence in three different methods: installing
itself as a LaunchAgent, installing itself as a LaunchDaemon, or injecting a command into the
user’s shell configuration file. For reference, a LaunchAgent is invoked when the user logs in, and a
LaunchDaemon is invoked when the system boots. Then, the malware contacts a hard-coded IP of a
command-and-control server using HTTP or DNS. After that, the code becomes a listener for
instructions from the server. Once installation is completed, the connection allows for updated
malware versions to be fetched, payloads to be downloaded, data to be extracted, and brute-force
attacks to be run.

Similarly, a new family of RATs for Windows and Linux called ZynorRAT appeared on VirusTotal
in July. This malicious code works in conjunction with a Telegram bot called “@Iraterrorsbot” to
gain unauthorized access to infected machines. This bot communicates using Turkish, pointing to an
origin country of the attacker. Further analysis of the bot shows the service Dosya.co as a payloaddelivery method. Both versions of the malware are exceptionally similar and can perform a number
of commands.

ZynorRAT, across both its Windows and Linux variants, works by using Telegram as a commandand-control server, simply connecting infected machines to the attacker’s account. The attacker is
then execute commands to:

1. Enumerate directories
2. Exfiltrate files
3. Profile the system
4. Run the “ps” Linux command
5. Kill processes
6. Take screenshots
7. Establish persistance

To sum up, CHILLYHELL and ZynorRAT highlight the ever-evolving landscape of malware on the
internet. The article from thehackernews.com titled “CHILLYHELL macOS Backdoor and
ZynorRAT RAT Threaten macOS, Windows, and Linux Systems” provides a detailed synopsis of the
dangers surrounding these specific malware samples and their histories.

### Sources

Main Source: https://thehackernews.com/2025/09/chillyhell-macos-backdoor-and-zynorrat.html

Secondary Sources:
- https://thehackernews.com/2025/07/hackers-leverage-microsoft-teams-to.html
- https://www.jamf.com/blog/chillyhell-a-modular-macos-backdoor/
- https://malpedia.caad.fkie.fraunhofer.de/details/win.matanbuchus
- https://www.malwarebytes.com/backdoor
- https://www.checkpoint.com/cyber-hub/threat-prevention/what-is-remote-access-trojan/
- https://apple.stackexchange.com/questions/290945/what-are-the-differences-betweenlaunchagents-and-launchdaemons

