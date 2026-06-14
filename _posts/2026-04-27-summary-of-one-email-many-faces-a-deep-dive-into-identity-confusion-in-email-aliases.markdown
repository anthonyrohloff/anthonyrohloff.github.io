---
layout: post
title: "Summary of \"One Email, Many Faces: A Deep Dive into Identity Confusion in Email Aliases\""
date: 2026-04-27
categories: blog-posts
authors: ["Anthony Rohloff", "Nate Metzger"]
---

## 1 Introduction to Email Aliases

Email addresses often serve as the universal identifier for account management on online platforms. To help protect user privacy, email providers offer email aliasing. An alias allows users to separate different activities by modifying the address while the same base email will receive all of the emails. Alias mechanisms redirect the email to the same inbox as the primary one. This allows users to never expose the base email while still receiving emails.

Even though mail aliasing exists to help users protect their identity, there are some issues with the way they are implemented. Depending on the email provider, different aliases may or may not be supported. Additionally, not all email providers provide documentation on the supported aliases allowed.

The confusion created from email aliasing allows attackers to employ two different abuses on online platforms or users. Alias Multiplicity Abuse (AMulA) refers to the abuse of registering multiple accounts using email aliasing on online platforms. Alias Misidentification Attack (AMisA) refers to attackers exploiting users' assumptions about alias formats across domains.

The purpose of the paper was to perform the first systematic analysis of identity confusion between email providers and online platforms. The following research questions were used to guide both the research and collection of data:

1. How do email providers document their aliasing mechanisms, and how do they compare to the actual aliasing behaviors?
2. How do online platforms interpret and handle email aliases, and how do their practices align with those of email providers?
3. How can adversaries abuse email aliasing mechanisms in real-world attacks, and what countermeasures can mitigate these risks?

To examine alias documentation and implementation, 28 email providers were selected. To evaluate the online platforms' abilities to recognize email aliases, 18 popular platforms that use email addresses as identifiers during account registration were selected. A controlled experiment was conducted, comparing the accuracy of email alias recognition for each platform.

OriginMail, a tool used to detect and normalize aliases to base addresses based on aliasing rules from the 28 providers was developed. By using this tool, alias emails from npm and GitHub were found from publicly accessible data ranging from 2009-2025.

## 2 System/Threat Model

Email aliasing can take many different forms. Because of this, many online platforms do not properly identify an aliased account during account verification. This leads to multiple accounts being developed from the same base email.

Additionally, aliasing confusion also leads to users not being able to identify fraudulent email addresses from valid ones. This raises the risk of phishing attacks as the user often associates the fraudulent email address as valid. The figure below demonstrates how both of these different ideas can be abused from the attacker.

<figure>
  <img src="/assets/blog_assets/email-aliases/fig1.png" style="display: block; margin: 0 auto;" />
  <figcaption style="text-align: center;">Figure 1: Key Risks Stemming from Email Alias Confusion</figcaption>
</figure>

The effects of AMulA can be seen in a multitude of ways. Many online platforms offer free trials to new users. If the platform is not equipped to recognize aliased email addresses, users can exploit the system and gain unlimited access to premium features. Abusers can also sign up for fake accounts to manipulate engagement metrics, such as likes, comments, or shares on social platforms. This can lead to distrust of the platform's reputation with users. Attackers can even bypass API rate limits by creating multiple accounts to scrape data and perform malicious actions.

While harder to quantify, AMisA can be equally harmful for users. Phishing attacks can steal sensitive information from uninformed users. Additionally, valid aliases may be avoided due to a misunderstanding from the receiver. This may lead to valid request not being handled in a timely manner.

## 3 Solutions to Identity Confusion in Email Aliases

The topic of identity confusion in email aliases does not have a one-size-fits-all solution. The research paper attempts to tackle the issue through a few different methods. One is by collaborating with online platforms to increase their security against email aliasing being used against them. To show the ethical consideration of this paper, online platforms were notified the findings relating to lack of effective detection in email aliases. When testing against a known dataset of npm and GitHub accounts, the researchers reached out to show them the attackers found using their OriginMail tool. One significant finding was that a single base email account being contributed to a spam campaign of 139 accounts.

OriginMail is a tool developed to showcase the findings within this study. It was built on the knowledge of the 28 different email providers and serves to detect and normalize aliases. OriginMail is accessible to anyone and can be useful in identifying fraudulent email aliases. An increase in the documentation for each email provider about aliases can go a long way.

Part of the research paper involved a study conducted to see if users could identify real or fake aliases. The research helped uncover trends, but was also used as an opportunity to raise awareness into identify confusion. Awareness of the issue helps participants to be more vigilant on fraudulent aliases.

The research paper also offers suggestions for all groups that are affected by identity confusion. Some of these recommendations include increasing the collaboration between email providers and offering standardized aliasing rules. Another recommendation is to implement more alias checks for platforms that discourage multiple accounts. The paper suggests notifying the base address when a variant is being used for registration attempts. Facebook as an example already utilizes this by sending a password reset email to the base address upon detecting a registration attempt using a known alias.

## 4 Evaluation and Experiment Results

The paper includes a multitude of interesting findings about email providers and aliases that they offer. Email providers can offer aliases in two different ways: syntactic aliases and customized aliases. Syntactic aliases are derived by modifying the syntax of the base address while still being routed to the same inbox. This can include adding characters within the username, and using a different domain suffix. Customized aliases allow users to explicitly create alias addresses that may be structurally unrelated to the base address. For the purposes of the paper, only syntactic aliases were considered.

One observation was that despite SMTP allowing case-sensitive usernames, all providers in the study silently enforce case-insensitive handling. Besides this, the rest of the aliases rules depend on the specific email provider. Gmail, for example, supports adding a dot (.) anywhere within the username. Eclipso would allow users to add special prefix characters to the username like a pound symbol (#) or a question mark (?) and only parse after the separator. Figure 2 outlines the findings of 12 providers and their special alias tolerances.

Moving onto the online platforms, the paper aimed to understand how each handled different versions of email aliasing. Typically when verifying emails during account registration, a three-step verification process is conducted. A validity check ensures that the email follows standard email rules such as the username and domain not having disallowed characters. Then, an alias check is something that each platform may conduct in a unique manner. The success of this check varies and usually only covers basic information such as allowing a plus sign (+) in the username. Finally, a duplication check verifies that the email does not already exist.

<figure>
  <img src="/assets/blog_assets/email-aliases/fig2.png" style="display: block; margin: 0 auto;" />
  <figcaption style="text-align: center;">Figure 2: Summary of Alias implementation of Email Providers</figcaption>
</figure>

The researchers conducted a semi-automatic test to see whether platforms would accept or recognize the email aliases that were generated in the previous part of the paper. The process involved monitoring the HTML element for any error messages when entering or confirming the alias for an existing email address. If a change occurred, it suggests that the platform would not accept the email. The results of this test can be seen in figure 3. Case variation was the most frequently caught alias when attempting to register a new account. The overall findings were that no platform fully defended itself against aliases rules for all 12 providers used. As a result, all are susceptible to Alias Multiplicity Abuse.

<figure>
  <img src="/assets/blog_assets/email-aliases/fig3.png" style="display: block; margin: 0 auto;" />
  <figcaption style="text-align: center;">Figure 3: Different Identities in Online Platforms for Alias Mechanisms</figcaption>
</figure>

The study also wanted to explore the threat that the Alias Misidentification Attack poses. In the study, participants were asked to act as users receiving help requests from a friend. The sender's email could be either a valid alias or a phishing address that resembled the friend's email. Six different email providers were selected for this user study, ranging from providers that offer familiar aliases and those that lack alias mechanisms.

The participants for the study were selected from Prolific, an academic survey platform, as well as Computer Science graduate students. A screening process was also conducted to ensure that the participants were taking the study seriously.

The results from the user study can be seen on the figure below. The major takeaway is that Gmail was the most commonly known alias. Despite this, participants overgeneralized the aliasing rules for Gmail, thinking they allowed more aliases than they actually do. Less common aliasing methods were often misidentified, indicating a general lack of awareness and understanding of acceptable aliases. Perhaps the biggest finding was that higher educated participants and male participants had the highest rate of phishing susceptibility rates. This suggests that when participants know something about aliasing, they often are more prone to believe they can recognize all aliases. Participants that simply reject any alias, while not guessing correctly in the survey, also avoided the phishing attempts at a higher rate.

<figure>
  <img src="/assets/blog_assets/email-aliases/fig4.png" style="display: block; margin: 0 auto;" />
  <figcaption style="text-align: center;">Figure 4: Different Identities in Online Platforms for Alias Mechanisms</figcaption>
</figure>


## 5 Limitations on the Study

There are limitations with the study as completed. Regarding the aliases chosen for the study, the customized aliases were not addressed. This was due to the lack of traceability, as this type of aliasing varied greatly in comparison to syntactic aliases. The researchers note that these aliases also pose the same threats, but that they are less likely to be abused by attackers. Their reasoning behind this is that typically, customized aliases have strict quantity limits by email providers and are more time consuming to make.

Another self-admitted limitation involves the wide range of email alias rules. It is difficult to know if all rules were identified and tested. Due to the lack of documentation on aliasing by some email providers, the researchers had to determine the aliasing rules through trial and error. This also extends to the OriginMail usage, as it is built on the researchers assumptions of the email provider's alias rules. Additionally, the researchers only tested 28 email providers. While they are the most common, there may be other email providers with stronger documentation that were not considered in this study. This would not change the results of the online platforms, but may have allowed the researchers to provide an example of proper aliasing documentation.

Testing for account registration is another limitation of this study. The semi-manual process that was used limited the scope that the study could accomplish. As a result, the researchers could not learn all of the platforms' identity recognition polices towards alias policies. Despite this, the research showed that the 18 tested online platforms can improve their existing aliasing detection mechanisms to defend against attackers.

The OriginMail tool was developed based on the assumption that the researchers knew the aliasing rules for all of the email providers. Additionally, the only tested dataset for the tool in the study was from GitHub and npm's publicly available user email list spanning the years 2009 to 2025. A further test on a wider dataset would have helped to verify the correctness of the ability to identify aliased email addresses.

## 6 Future Research Directions

The next step in addressing email aliasing abuse is to explore the impacts of customized aliases. Their flexible nature may lend itself to different trends than syntactic aliasing. Additionally, the address the mail from the custom alias is from is different than the address a reply will be sent to. This is in contrast to syntactic aliasing, where the base email is always exposed. To improve OriginMail, some type of recognition of custom aliases should be attempted.

A different approach to mitigate email alias abuse would be to develop an open-source framework for email aliasing. Allowing email providers to easily integrate a standard aliasing system would have multiple benefits: more providers would be able to include aliasing capabilities in their product, and users would be better equipped to determine phishing emails from real ones with proper documentation and familiarity. The major downside to this approach is many larger providers like Google and Microsoft would likely not adjust their aliasing system to match that of the potential open-source framework. If done incorrectly, it could further fragment the aliasing landscape.

Finally, a third option would be to develop an open-source email alias recognition code library for platforms to use. This could help platforms with their alias check step as defined in section 4. It would also assist smaller platforms that may not have the resources to integrate manual alias checking in installing protection against alias abuse. This library should have clear documentation and functions with the capability to take in email addresses and return the base email address according to each provider's rule set. Additionally, if an open-source email alias standard was developed, it could be incorporated into this library.

## 7 Conclusion

In conclusion, the research paper revealed significant and widespread identity confusion caused by inconsistencies in email aliasing mechanisms across email providers, online platforms, and users. By examining 28 email providers, the researchers found that only Gmail fully documents its aliasing rules, while 11 providers silently support undocumented alias behaviors. No tested online platforms were able to fully defend against alias-based account creation across providers, leaving all platforms susceptible to Alias Multiplicity Abuse. The real-world impact of this vulnerability was demonstrated through npm, where 139 accounts were derived from one base email address and used for spam.

The user study further highlighted the risks of Alias Misidentification Attack, showing that users with partial knowledge of aliasing mechanisms are overconfident in their ability to detect all aliases. This reckless belief makes them more susceptible to phishing attempts that mirror alias rules for other email providers. Standardization of aliasing was one of the key suggestions to help reduce the effects of the attacks with identity confusing. While the study openly acknowledges limitations to the study, the findings show the need to improve defense mechanisms against alias attacks.

Anthony Rohloff
