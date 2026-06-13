---
layout: post
title: "The Legal Considerations of Penetration Testing"
date: 2026-02-25
categories: blog-posts
---

From: Anthony Rohloff  
To: Klein

## BLUF

Penetration testing is a crucial step in system hardening, but it is critical that guidelines must be
followed to ensure the legality of the operations, both from the side of the organization and the
penetration tester.

## Background

Penetration testing, commonly referred to as pen testing, is the practice of launching controlled
cyberattacks against a computer system. Then, the results of the test are used to “harden” or improve
the security posture of the system. Penetration testing is a subset of a wider field known as ethical
hacking. Professionals in this field employ extensive computer security services like malware analysis,
risk assessment, and compliance (“What is Penetration Testing?”).
There are five steps to a penetration test. First, reconnaissance entails gathering information
about the target. This can be anything from network infrastructure to physical security measures. Then,
a pen tester would begin scanning. This is a way to enumerate the potential vulnerabilities and
entrances into the network. After finding all the open ports and examining the infrastructure, that
information is used to identify vulnerabilities related to what was found. This gives the pen tester attack
vectors to attempt to exploit the network. The fourth step is when these attempts are made. The attacker
uses different tools to simulate real-world attacks and records the findings. Finally, these findings are
gathered into a report for the organization to use in its cybersecurity hardening efforts (“Understanding
the Five Phases of the Penetration Testing Process”).

Some organizations are subject to regulations that are best complied to via penetration testing.
The Computer Fraud and Abuse Act and the Electronic Communications Privacy Act define the law in
regards to the access to protected computer systems and the interception and disclosure of electronic
communications. It is best practice to document all tests performed in detail to have proof of
compliance or a clear understanding of current shortcomings. In addition to the requirement to comply
to these regulations, organizations must also consider the cost of a penetration test gone awry. There
could be damage to production systems or an escaped malware that wreaks havoc on unaffiliated
internet users, usually happening in the fourth step of the penetration testing process: exploitation
(Gilliam).

## Analysis

An example of 18 USC 1030, or the Computer Fraud and Abuse Act, in action during a pen
testing exercise happened in 1999 when a security practitioner by the name of Scott Moulton
performed a network scan on the Cherokee County, Georgia 911 system. Claims of a slow-down in the
network led to criminal charges under the aforementioned law. It is stipulated in the law that any
interference of a computer system is grounds for conviction. Since a network scan slow a network
down, even if it is by a minuscule amount, the Georgia Bureau of Investigation decided it had enough
evidence to charge Moulton with the crime. The court found Moulton not guilty, citing there was a lack
of malicious intent and actual damage (“Scott Moulton and Network Installation Computer Services,
Inc. V. VC3”). This case was one of the first to use the Computer Fraud and Abuse Act, and it shaped
the interpretation of it in a way that protects pen testers.
However, if a test is conducted outside of the agreed-upon scope, then the previously mentioned
precedent may not apply. United States v. Joseph Sullivan was a case regarding “bug bounty”
programs. A bug bounty program is offered by a company to entice hackers to find vulnerabilities in
their system and disclose them to the company for payment. These programs were being used to cover
up cyberattacks by retroactively claiming a real breach into the network was a bug bounty finding,
paying off the attackers, and having them sign NDAs to prevent reputational damage (Archer). Joseph
Sullivan was the CISO of Uber in 2016, and he used this method to cover up a data breach. The court
ruled in favor of the prosecution in this case, and set a potentially worrying precedent for pen testers.
Even if a bug was found in good faith, if it fell outside of the scope the bug bounty program for the
related organization, one could be found guilty of violating the CFAA (Mascellino). Intent only protects
pen testers when they are explicitly authorized and operating within scope.

## Recommendations

Both organizations and pen testers need to be diligent when conducting penetration testing
operations. Organizations are responsible for keeping their data secure, so they need to hire pen testers,
but they need to ensure the pen testers they hire will work within scope and within the constraints of
the law. Pen testers themselves need to protect themselves by ensuring they are always within scope
and are not causing real-world damages with their tests. It would be best to always follow a penetration
testing framework from a respected organization like NIST or OWASP to help keep testing effective
and legal.

Anthony Rohloff

## Works Cited

- Archer, Jerry. “A Court Ruling on Bug Bounties Just Made the Internet Less Safe.” Infosecurity Magazine, 25 July 2025.
- Gillam, Jason. “What Are the Ethical and Legal Considerations for Penetration Testing?” Secure Ideas, 9 Mar. 2023.
- Mascellino, Alessandro. “Uber’s Former Security Chief Convicted of 2016 Data Breach Cover-Up.” Infosecurity Magazine, 6 Oct. 2022.
- “Scott Moulton and Network Installation Computer Services, Inc. V. VC3.” Internetlibrary.com, 6 Nov. 2000.
- “Understanding the Five Phases of the Penetration Testing Process.” Cybersecurity Exchange, EC-Council, 28 Mar. 2022.
- “What Is Penetration Testing?” IBM.com, IBM, 24 Jan. 2023.

