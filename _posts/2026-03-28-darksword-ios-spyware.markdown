---
layout: post
title: "DarkSword iOS Spyware Article Review"
date: 2026-03-28
categories: blog-posts
---

Apple is known for its secure software design, but there has been a breach to that standard
recently. The Google Threat Intelligence Group (GITG) has identified a new exploit that leverages
multiple zero-day vulnerabilities on iOS versions 18.4 to 18.7. This full-chain exploit has been dubbed
"DarkSword" by the group, and has three known families of malware: GHOSTBLADE,
GHOSTKNIFE, and GHOSTSABER. The origins of this malware can be traced back to a different
exploit called Coruna that was patched by Apple in 2025. This mutation is even suspected to be part of
state-sponsored Russian hackers' playbooks.

<img src="/assets/blog_assets/darksword/snap.png" style="float: right; margin: 0 0 1em 1em; width: 300px;" />

GITG has identified four countries where the Russians are targeting users. First, Saudi Arabia has
experienced attacks via a Snapchat clone. They equipped this site with the GHOSTKNIFE
payload, which exfiltrates signed-in accounts, messages, browsing data, location history, recordings,
can download files onto the infected device, and can listen to live audio from the device's microphone
via a JavaScript implementation.

Turkey and Malaysia were targets of the GHOSTSABER variant. A commercial surveillance
company called PARS Defense was infected, and the malware was distributed through that company
and its customers. GHOSTSABER is a backdoor that has been tracked by GITG since November 2025.
It also uses JavaScript to communicate to a command-and-control server via HTTP/HTTPS. It has
similar capabilities to GHOSTKNIFE, boasting device and account enumeration, file listing, data
exfiltration, and code execution. A complete list of commands is available in the cited article.

Finally, Ukraine is unlikely to go unscathed with any Russian cyber campaign. The country at
war with Russia is facing GHOSTBLADE, a watering hole variant of DarkSword. This malware is the
least capable of the three, but can still exfiltrate data pertaining to communications, messaging,
financial transactions, location, behavioral data, and more to the C2 server via HTTP/HTTPS. The 
exploit chain of DarkSword can be seen below:

<img src="/assets/blog_assets/darksword/diagram.png" style="display: block; margin: 0 auto;" />

The main flow of the malware starts with the exploit loader, where two files are downloaded from the
command-and-control server, each exploiting different zero-day vulnerabilities. Then, the malware has
a logical fork based on iOS version. There are two different paths to achieve arbitrary code execution,
which then merge back together to perform sandbox escape and escalate local privileges until it is
possible to deploy the final payload. The article from GITG goes into more technical details for each
stage of the execution chain, as well as differences between the strands of DarkSword.

## Works Cited

- Google Threat Intelligence Group. "The Proliferation of DarkSword: IOS Exploit Chain Adopted by Multiple Threat Actors." Google.com, 18 Mar. 2026, cloud.google.com/blog/topics/threat-intelligence/darksword-ios-exploit-chain.
- Schappert, Stefanie. "Dangerous IPhone Hack Code Now Leaked on GitHub – Users Urged to Patch Now." Cybernews, 25 Mar. 2026, cybernews.com/security/angerous-iphone-hack-code-leaked-github-patch-now/. Accessed 28 Mar. 2026.
