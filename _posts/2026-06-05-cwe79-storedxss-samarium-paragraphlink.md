---
layout: post
title: "cwe80-storedxss-samarium-paragraphlink"
date: 2026-06-05
categories: vulnerability-research
---


**Description**

In Samarium 0.9.6, a stored cross-site scripting (XSS) vulnerability exists in the `/cms/post` component. When adding a **Paragraph** component to a post, a text entry is presented that allows a link to be entered. The `javascript:` tag is able to be used to execute arbitrary code upon click. It is possible to steal cookies and data, deface the web page, or redirect users by exploiting this vulnerability.
- Vulnerable Endpoint: `POST /livewire/update with cms.dashboard.webpage-content-create-paragraph`
- Vulnerable Parameter: **paragraph**
- Payload Examples: `javascript:alert(1)`, `javascript:alert(document.cookie)`

---
**CVSS**

| CVSS Version | Score | Severity | Vector String                                                   |
| ------------ | ----- | -------- | --------------------------------------------------------------- |
| 3.1          | 4,8   | Medium   | CVSS:3.1/AV:N/AC:L/PR:H/UI:R/S:C/C:L/I:L/A:N                    |
| 4.0          | 4.6   | Medium   | CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:A/VC:L/VI:N/VA:N/SC:N/SI:N/SA:N |

---
**Steps to Reproduce**

1. Login to `/dashboard` as admin
		![](/assets/vulnerability_assets/cwe79-storedxss-samarium-paragraphlink/login.png)

2. Navigate to **CMS > Posts**
		![](/assets/vulnerability_assets/cwe79-storedxss-samarium-paragraphlink/gotoposts.png)

3. Create a new post
		![](/assets/vulnerability_assets/cwe79-storedxss-samarium-paragraphlink/createnewpost.png)

4. Enter a name for the post and click **Submit**
		![](/assets/vulnerability_assets/cwe79-storedxss-samarium-paragraphlink/newpost.png)

5. Go to **Add content**, select **Paragraph**
		![](/assets/vulnerability_assets/cwe79-storedxss-samarium-paragraphlink/addparagraph.png)

6. Click the link icon, enter `javascript:alert(document.cookie)` into the field, click **Link**, then click **Save**
		![](/assets/vulnerability_assets/cwe79-storedxss-samarium-paragraphlink/payloaddrop.png)

7. The payload can be executed by clicking on it right away
		![](/assets/vulnerability_assets/cwe79-storedxss-samarium-paragraphlink/payloadadmin.png)

8. Navigate back to `/` and scroll to the bottom of the page, then click **Read More** on the blog post to execute the payload from the user's perspective
		![](/assets/vulnerability_assets/cwe79-storedxss-samarium-paragraphlink/openpost.png)

9. View payload execution
		![](/assets/vulnerability_assets/cwe79-storedxss-samarium-paragraphlink/payload.png)

---
**Recommendations**

1. Reject inputs into the vulnerable field that include `javascript:`
2. Implement a content security policy that restricts script execution to trusted sources
3. Cookies should have the `HttpOnly` flag set to prevent JavaScript code from using the cookie, and the `Secure` flag should be set to force `HTTPS` over `HTTP`

*Credit: [OWASP](https://cwe.mitre.org/data/definitions/79.html)*
