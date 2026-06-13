---
layout: post
title: "How the OWASP Top 10 Applies to the Federal Trade Commission Act"
date: 2026-04-13
categories: blog-posts
---

From: Anthony Rohloff  
To: Klein

## BLUF

The OWASP Top 10 spreads awareness about the most critical security risks to web
applications. It is a data-informed, community-approved document that can help companies avoid run-
ins with the FTC that can result in fines and other burdens.

## Background

OWASP (Open Worldwide App Security Project) is a nonprofit organization with the ultimate
goal of "no more insecure software" ("About the OWASP Foundation"). Perhaps their most popular
work is a list of the Top 10 web application security risks, updated every 4 years. The latest installment
of this list was revealed in 2025. It includes:

1. Broken Access Control
2. Security Misconfiguration
3. Software Supply Chain Failures
4. Cryptographic Failures
5. Injection
6. Insecure Design
7. Authentication Failures
8. Software or Data Integrity Failures
9. Security Logging and Alerting Failures
10. Mishandling of Exceptional Conditions

These items are listed in order of importance. For example, broken access control occurred over 1.8
million times during OWASP's research, with 100 percent of applications tested having an instance of
broken access control. Mishandling of exceptional conditions was found about 750 thousand times, less
than half as often as broken access control. This list serves as a standard for developers to follow when
creating and maintaining web applications. Developers have an agreed-upon place to start testing their
application when they follow the OWASP Top 10 ("OWASP Top 10:2025").

To properly understand the Top 10, the methodology used to create the list must be explained.
First, OWASP gathers data on web applications and categorizes any weaknesses into a list. The
categories are structured to focus on the root cause of the weakness, rather than the symptom. However,
OWASP notes that sometimes the symptom was listed in special cases. 12 categories are initially
selected as the top issues for web application security. OWASP uses data to determine the 8 categories
that are most important to include in the final list. The next step is the community survey. In this step,
professionals in the field are asked to vote on what they see as the forefront of threats in today's cyber
landscape. This is done to avoid the sluggishness of bureaucracy in logging, categorizing, and
publishing weaknesses in official databases. Those who have "boots on the ground" can see the
emerging threats much more quickly than the dataset can. These professionals pick the final 2
categories to include in the list, rounding out the Top 10. This list is then published and used as a
standard for the next four years ("Introduction – OWASP Top 10:2025 RCI").

These standards help protect organizations from security breaches and data leaks, which can
also help protect against violating Section 5 of the Federal Trade Commission Act, 15 U.S. Code § 45.
Companies can be held responsible for "unfair or deceptive business practices" that (1) are likely to
cause "substantial injury" to consumers, (2) consumers cannot reasonably avoid the injury, and (3) the
injury is not outweighed by the benefits to consumers or competition. According to the FTC, Congress
had intended for a broad interpretation of the act. They have the capacity to file complaints against and
even sue companies that have lackluster cybersecurity practices (Denny).

## Analysis

1. Broken Access Control

    This web application flaw enables users to perform actions outside of their intended
    permissions. OWASP provides an extensive list of common vulnerabilities that fall into
    this category, including violating the principle of least privilege, where actions should be
    restricted to certain users or groups, but they are accessible to anyone. Privilege
    escalation, where a user can expand their permissions unexpectedly, is another example.
    A third case is applications that allow for "force browsing", or allowing users to access a
    page they are not authorized for by typing in the URL to that page directly. To prevent
    this vulnerability from being exploited, another list of preventative measures is given by
    OWASP. Deny by default, logging failures, and using trusted access control frameworks
    and toolkits are highlighted. These measures are important to implement, but the method
    of implementation is paramount to its success. OWASP says, "access control is only
    effective when implemented in trusted server-side code or serverless API's, where the
    attacker cannot modify the access control check or metadata" ("OWASP Top 10:2025").

    In 2025, a company called Illuminate Education faced an FTC suit for leaking over 10
    million students' identities. The FTC accused the company of "failing to implement
    reasonable access controls that safeguard students' personal information…" The result
    of the case was an FTC order for Illuminate Education to clean up its cybersecurity
    practices, including deleting unneeded information, establishing a comprehensive
    information security program, and notifying the FTC about any data breaches.
    Violations of this agreement could result in a fine of up to $51,744 ("FTC Takes Action
    against Education Techonlogy Provider for Failing to Secure Students' Personal Data").

2. Security Misconfiguration

    A security misconfiguration occurs when a service is set up in a way that allows for
    exploitation. A few examples of this are unnecessary open ports, security features being
    disabled, and default credentials not being changed. OWASP recommends a minimal
    platform construction with identical production and development environments to
    ensure accurate testing, along with periodic reviews and updates of the platform
    ("OWASP Top 10:2025").

    The popular website hosting service GoDaddy faced an FTC enforcement action after
    several data breaches. The company did not use multi-factor authentication and had
    insecure connections to customer data. GoDaddy must implement a new, comprehensive
    security program and hire independent third-party assessors to ensure proper
    configurations ("FTC Finalized Order with GoDaddy over Data Security Failures").

3. Software Supply Chain Failures

    This entry was highly ranked in the community survey. It is a broad category spanning
    from outdated software to ensuring human code review. There is significant overlap with
    security misconfiguration here. The recommended prevention measures include
    removing unused dependencies and use thorough change management processes
    ("OWASP Top 10:2025").

    In January of 2022, the FTC issued a warning to companies to remediate the Log4j
    security vulnerability. Log4j is a popular logging framework written in Java that was
    being exploited by attackers. The FTC notes that failure to upgrade to a secure version
    or mitigate the risk in another way could be in violation of the law ("FTC Warns
    Companies to Remediate Log4j Security Vulnerability").

4. Cryptographic Failures

    Previously ranked second, this entry warns against using old or weak cryptographic
    algorithms, default or weak keys, or predictable random number generation.
    Preventative measures include using the latest algorithms, limiting stored data, and
    storing keys in secure hardware systems, like hardware security modules ("OWASP Top
    10:2025").

    The video conference platform Zoom claimed it had end-to-end encryption
    implemented, but when investigated, it was found the real system was not as advertised.
    Similar to previously mentioned cases, a security program was required, as well as data
    storage regulations. The company is subject to fines if these measures are not
    implemented ("FTC Requires Zoom to Enhance Its Security Practices as Part of
    Settlement").

5. Injection

    This category encompasses a large number of vulnerabilities, many of which are low-
    impact but collectively significant. It is one of the most widely-tested categories due to
    its straightforward nature. An injection attack allows user input to be interpreted as
    something other than what it is supposed to be interpreted as. For example, entering an
    SQL command into a vulnerable sign-in page could enable the user to run arbitrary SQL
    commands. An application is vulnerable when data is not validated or escape characters
    can be abused, among others. Safe API's are available that limit the effectiveness of
    these attacks. There are also ways to verify inputs and disable dangerous character
    inputs ("OWASP Top 10:2025").

    In 2003, the clothing brand Guess's website was vulnerable to SQL injection attacks,
    despite assuring customers their data was safe. Data leaks occurred, and the FTC gave
    its standard ruling of a security program overhaul and third-party verification on a yearly
    basis, subject to significant fines if violated. This case also applies to [4] cryptographic
    failures, since Guess claimed to store sensitive information in an "unreadable, encrypted
    format at all times," but the data was available to be accessed in plain text from the
    website via an SQL injection attack ("Guess Settles FTC Security Charges; Third FTC
    Case Targets False Claims about Information Security").

6. Insecure Design

    This category has to do with security design flaws in software rather than insecure
    implementation, which is what all the other categories are about. OWASP calls for the
    software community to embrace pre-coding activities such as application design and
    requirements writing. A secure development lifecycle is also essential. Prevention of
    design flaws include integrating checks in an application and segmenting as much as
    possible, following known secure design structures, and threat model throughout the
    design process ("OWASP Top 10:2025").

    Illusory Systems, a blockchain infrastructure company, let its insecure design cost
    consumers $186 million. Inadequately tested code was pushed to production and
    promptly abused. The FTC quickly took notice and ordered an improved security
    program, third-party reviews, and all lost funds to be returned to customers, citing
    insecure coding practices, no process for handling vulnerabilities, and insufficient
    incident response capabilities. This puts Illusory Systems in a difficult situation, as 186
    million dollars is a significant amount of money to come up with, and each case where
    money has not been returned results in a $50,000 fine ("FTC Will Require Illusory
    Systems to Return Money Stolen by Hackers and Implement an Information Security
    Program").

7. Authentication Failures

    An authentication failure occurs when a user is able to force a system to recognize an
    invalid user as legitimate. This allows for brute-force password cracking attempts, weak
    or default passwords, and allowing for weak fallbacks if multi-factor authentication is
    not available. This can be prevented by enforcing multi-factor authentication, password
    management software, and weak password checks ("OWASP Top 10:2025").

    TikTok is a prime example of authentication failure. The social media giant got around
    its age limit of 13 years and up by allowing users to create accounts through third parties
    like Google and Facebook. This resulted in millions of underage accounts being created
    without parental consent, directly correlating with the weak fallback point on the
    OWASP authentication failures page. The company was fined 5.7 million dollars and
    forced to improve age verification processes ("FTC Investigation Leads to Lawsuit
    against TikTok and ByteDance for Flagrantly Violating Children's Privacy Law").

8. Software or Data Integrity Failures

    This category relates to untrusted code being treated as valid. This often occurs when
    applications rely on external plugins, libraries, or modules. Additionally, many of these
    external content packages automatically update, which can lead to an unknown
    vulnerability being added to an application via a library it depends on. Attackers could
    create a content package that is useful, distribute it to people, and then inject malicious
    code in an update and gain access to sensitive data. This can be prevented by using
    trusted, well-known repositories, ensuring code is reviewed before it is added to
    production, and segmenting the application as much as possible ("OWASP Top
    10:2025").

    Lenovo was fined 3.2 million by the FTC after installing adware called VisualDiscovery
    on its products in 2014 to 2015. The program acted as a man-in-the-middle between
    users and the websites they visited, and used the data gathered to display ads. The
    implementation of this adware was not secure, and enabled others to take over the
    adware and have it harvest credit card numbers, social security numbers, and other
    sensitive pieces of information. An improved security program was ordered, and fines to
    be issued if protocols are violated ("Lenovo Settles FTC Charges It Harmed Consumers
    With Preinstalled Software on Its Laptops That Compromised Online Security").

9. Security Logging and Alerting Failures

    As an incredibly difficult category to test for, security logging and alerting failures may
    be underrepresented in the Top 10. This was the other entry from the community survey,
    proving to be important in the real world. In the absence of logging and alerts, attacks
    can go unnoticed, allowing malicious actors to place footholds in secure networks and
    exfiltrate data freely. Logs should always be made on login, failed login, or on high-
    value transactions, and the logs should be backed up to an off-site storage location. Logs
    should always be monitored, and the application should be able to escalate alerts for
    attacks in real-time ("OWASP Top 10:2025").

    A strong case for logging and alerting failures is Wyndham Worldwide Corp., which
    allowed three separate data breaches totaling over 10.6 million dollars in fraudulent
    charges. The company failed to act on multiple data breaches, violating other
    recommendations of the Top 10 such as [1] broken access control, [2] security
    misconfiguration, and [4] cryptographic failures. As if the lax security measures were
    not enough, they also failed to monitor these breaches and did not investigate and
    improve practices properly, allowing for multiple instances of data leakage that resulted
    in over 600,000 credit card numbers being lost (Denny).

10. Mishandling of Exceptional Conditions

    This final category accounts for improper handling of error states and other abnormal
    conditions. There are three ways this can cause a security risk: (1) the application does
    not prevent an unusual situation from happening, (2) it does not identify the situation as
    it is happening, and (3) it responds poorly to the situation afterwards. The preventative
    measures then become apparent: catch errors and handle them gracefully, log and alert
    on failures and errors, and implement automated responses to potential issues ("OWASP
    Top 10:2025").

    A 575 million dollar settlement was made between Equifax and the FTC. The company
    was warned about a vulnerability in a Java framework for building web applications.
    There was a free update that patched it, and all the company had to do was update their
    version. Their failure to complete the update is a mishandling of exceptional conditions
    because the critical vulnerability failed unsafely, allowing for hundreds of millions of
    social security numbers and credit card numbers to be leaked. The FTC required Equifax
    to pay the 575 million dollars to damage remediation efforts, along with the standard
    annual third-party assessments and improved security program (Fair).

## Recommendations

The OWASP Top 10 is a strong standard to follow when developing a web application. There
are certainly other attack vectors hackers can utilize outside of this list, but these will be the ones they
will likely try first and most frequently. It was shown that a lack of consideration for these categories
can result in large fines and restrictions on companies. Proper cybersecurity measures are orders of
magnitude less expensive than the fines some of these companies faced, so hiring cybersecurity
professionals that utilize a framework like OWASP's Top 10 is paramount to the success of a company,
especially as attacks become more frequent and more advanced.

Anthony Rohloff

## Works Cited

- "15 U.S. Code § 45 - Unfair Methods of Competition Unlawful; Prevention by Commission." LII / Legal Information Institute, www.law.cornell.edu/uscode/text/15/45.
- "About the OWASP Foundation." Owasp.org, OWASP, owasp.org/about/.
- Denny, William R. "Cyber Center: Cybersecurity as an Unfair Practice: FTC Enforcement under Section 5 of the FTC Act." Americanbar.org, 2016, www.americanbar.org/groups/business_law/resources/business-law-today/2016-june/cybercenter-cyber-security-as-an-unfair-practice/.
- Fair, Lesley. "$575 Million Equifax Settlement Illustrates Security Basics for Your Business." Federal Trade Commission, 22 July 2019, www.ftc.gov/business-guidance/blog/2019/07/575-million-equifax-settlement-illustrates-security-basics-your-business.
- "FTC Finalizes Order with GoDaddy over Data Security Failures." Federal Trade Commission, May 2025, www.ftc.gov/news-events/news/press-releases/2025/05/ftc-finalizes-order-godaddy-over-data-security-failures.
- "FTC Investigation Leads to Lawsuit against TikTok and ByteDance for Flagrantly Violating Children's Privacy Law." Federal Trade Commission, 2 Aug. 2024, www.ftc.gov/news-events/news/press-releases/2024/08/ftc-investigation-leads-lawsuit-against-tiktok-bytedance-flagrantly-violating-childrens-privacy-law.
- "FTC Requires Zoom to Enhance Its Security Practices as Part of Settlement." Federal Trade Commission, Nov. 2020, www.ftc.gov/news-events/news/press-releases/2020/11/ftc-requires-zoom-enhance-its-security-practices-part-settlement.
- "FTC Takes Action against Education Technology Provider for Failing to Secure Students' Personal Data." Federal Trade Commission, Dec. 2025, www.ftc.gov/news-events/news/press-releases/2025/12/ftc-takes-action-against-education-technology-provider-failing-secure-students-personal-data.
- "FTC Warns Companies to Remediate Log4j Security Vulnerability." Federal Trade Commission, Jan. 2022, www.ftc.gov/policy/advocacy-research/tech-at-ftc/2022/01/ftc-warns-companies-remediate-log4j-security-vulnerability.
- "FTC Will Require Illusory Systems to Return Money Stolen by Hackers and Implement an Information Security Program." Federal Trade Commission, 15 Dec. 2025, www.ftc.gov/news-events/news/press-releases/2025/12/ftc-will-require-illusory-systems-return-money-stolen-hackers-implement-information-security-program. Accessed 14 Apr. 2026.
- "Guess Settles FTC Security Charges; Third FTC Case Targets False Claims about Information Security." Federal Trade Commission, 18 June 2003, www.ftc.gov/news-events/news/press-releases/2003/06/guess-settles-ftc-security-charges-third-ftc-case-targets-false-claims-about-information-security. Accessed 14 Apr. 2026.
- "Introduction - OWASP Top 10:2025 RC1." Owasp.org, OWASP, 2025, owasp.org/Top10/2025/0x00_2025-Introduction/.
- "Lenovo Settles FTC Charges It Harmed Consumers with Preinstalled Software on Its Laptops That Compromised Online Security." Federal Trade Commission, 4 Sept. 2017, www.ftc.gov/news-events/news/press-releases/2017/09/lenovo-settles-ftc-charges-it-harmed-consumers-preinstalled-software-its-laptops-compromised-online.
- "OWASP Top 10:2025." Owasp.org, OWASP, 2025, owasp.org/Top10/2025/.
