---
layout: post
title: "Copyright Law and Licensing of Computer Operating Systems"
date: 2026-01-16
categories: blog-posts
---

From: Anthony Rohloff  
To: Klein

## BLUF

Copyright law and licensing affects the rights of users to copy, modify, or distribute software,
including operating systems, in the United States via title 17 of the U.S. Code. The main legal
differences between common operating systems like Windows, MacOS, Linux, and BSD stem from the
rights granted from licenses such as GPL and BSD.

## Background

Copyright law in the United States is defined in Title 17 of the U.S. Code. In section 102(a),
copyright protection "in general" can be applied to any work of authorship in any medium, including
"literary works" like software. Software, or a "computer program", is defined in the document as "a set
of statements or instructions to be used directly or indirectly in a computer in order to bring about a
certain result" in section 101. However, the protection does not extend to the idea behind the software,
only the implementation method, which is explained in section 102(b). These rights and limitations as
they pertain to computer programs explicitly are also explained in greater depth in section 117.

Software can also have licenses. Licenses determine what rights are granted to users of a piece of
software. For example, Windows and MacOS use a proprietary license that prohibits copying,
modifying, or distributing it without the consent of the parent company (Gisonna). Linux, or more
specifically the Linux kernel, uses a GNU General Public License that allows for sharing and
modification of the source code under the agreement that any derivative work will use the same or an
equivalent license (Free Software Foundation). Finally, BSD is a less common operating system that
uses its own license called the BSD license. It is a simple license with three clauses proclaiming any
redistributions of the software must retain the BSD license if distributed in source-code form, or
reproduce the license if distributed in binary form, and the copyright holder and contributors names
must be included, but may not be used to promote derivations of the distribution without consent. The
main difference between the BSD and GPL licenses is the GPL license requires source code to be
distributed, while BSD allows for the distribution of only the binary program (Open Source Initiative).
To summarize, proprietary licenses prohibit any modifications or distributions, GPL allows for
modifications, but requires the source code to be public, and the BSD license only requires credit to be
given to the authors of the code.

## Analysis

In the terms of use provided by Microsoft for Windows 11, section 2(a) covers licensing. One is not able
to "purchase a copy" of Windows 11 from Microsoft. It states that "the software is licensed, not sold."
When buying a key from Microsoft, one is actually only buying a license to use one instance of
Windows 11 on one device for use by one person at a time. If Windows 11 is acquired by any means
other than the intended method, "you do not have a license to use the software." Similarly, Apple grants
a "limited, non-exclusive license to install, use and run one (1) copy of the Apple Software on a single
Apple-branded computer at any one time." This license is slightly more restricting than the Microsoft
one since it requires the software be installed on an Apple computer, while Microsoft makes no
specification on the required hardware. These companies reserve the right to change or revoke the
software at any time for any reason. Users are at the whim of the corporations.

In the case of open-source software like Linux, the user has significantly more freedom. The GNU
General Public License, or GPL, self-proclaims itself as a "strong copyleft," meaning it satisfies the four
essential freedoms a piece of free software should have. These freedoms are the ability to use the
software for any purpose, change the software as needed, share the software, and share the changes
made to the software. The license guarantees that any derivative version or distribution of the software
will remain free forever. It's a forced collaboration agreement that nobody will take the software away.
If someone did want to charge for software under the GPL license, it would only take one person buying
that software and distributing it to render the paid version useless since the source code must be
included (Free Software Foundation).

The BSD license, on the other hand, does not afford users that peace of mind. This short license is close
to a no-holds-barred approach to software licensing. Someone could take software using this license that
is freely available and start to sell it as proprietary software with a proprietary license, with or without
changes, and with or without the source code. However, this does not stop the propagation of the free
version it stemmed from (Miller). This is actually the historical origin of many components of the
proprietary operating system MacOS; it is a derivative of early versions of the BSD operating system
(BSD Overview). To summarize this license model in a sentence, as long as the BSD 3-clause passage is
attached to the software in some way, anything is allowed, and the distributing party is protected from
lawsuits.

Different licensing models carry tradeoffs. Proprietary software can offer centralized support,
integration, and convenience, but can also increase vendor dependence and reduce user control. The
drawbacks to this model are dependence on the software provider when it comes to pricing and updates
and the inability to fix or troubleshoot issues without the help of the providing company (Open-Source
vs. Proprietary Software Pros and Cons). There is also the issue of security. Open-source software is
often more secure than proprietary software because a wider range of experts can find issues within the
code. Proprietary software can only be improved at the rate the developers can find and fix bugs, while
open-source software has a much larger pool of experts to improve it and patch issues (Coggill).

## Recommendations

Each license has its merits, and one should not always be preferred over the other. The proprietary
model works for Microsoft and Apple because they have the support and infrastructure to build out new
services and create their own ecosystems for maximum convenience for users. Linux and the Free
Software Foundation fight for the little guy. They want to create a community-based, collaborative
software world. For smaller, grassroots projects, the GPL license is perfect. If Linux was proprietary, it
would have never grown into the giant it is in the world of operating systems today. Finally, the BSD
license works for people who want to create something, but do not necessarily want to maintain, update,
or own it going forward. In regards to the BSD license, the creator of Linux, Linus Torvalds, said "Over
the years, I've become convinced that the BSD license is great for code you don't care about." (Bhartiya)
This points to the idea that someone using the BSD license wanted to build something useful and leave
it there for anyone to use, modify, or distribute with no restrictions.

Anthony Rohloff

## Works Cited

- Bhartiya, Swapnil. “Linus Torvalds Says GPL Was Defining Factor in Linux’s Success.” CIO, 27 Aug. 2016.
- “BSD Overview.” Apple Developer Documentation.
- Coggill, Henry. “Is Open-Source as Secure as Proprietary Software?” Ubuntu, 8 Feb. 2023.
- Free Software Foundation. “The GNU General Public License v3.0.”
- Gisonna, Nicholas. “Proprietary Software.” Britannica, 19 Dec. 2023.
- Microsoft. “Microsoft License Terms: Windows 11.” Apr. 2024.
- Miller, Kristin. “Open Source Software Licenses 101: The BSD 3-Clause License.” FOSSA, 25 Mar. 2021.
- Open Source Initiative. “The 3-Clause BSD License.”
- Optimus Info. “Open-Source vs. Proprietary Software Pros and Cons.” 2015.
- Apple. “Software License Agreement for macOS Tahoe 26.”
- U.S. Copyright Office. “Copyright Law of the United States (Title 17).” Dec. 2024.

