---
layout: post
title: "cwe80-storedxss-samarium-header"
date: 2026-06-05
categories: vulnerability-research
---

**Description**

In Samarium 0.9.6, a stored cross-site scripting (XSS) vulnerability exists in the `/cms/post` component. When adding a **Heading** component to a post, a text entry is presented that allows for arbitrary JavaScript code execution. It is possible to steal cookies and data, deface the web page, or redirect users by exploiting this vulnerability.
- Vulnerable Endpoint: `POST /livewire/update with cms.dashboard.webpage-content-create-heading`
- Vulnerable Parameter: **heading**
- Payload Examples: `<script>alert(1)</script>`, `<script>alert(document.cookie)</script>`

---
**CVSS**

| CVSS Version | Score | Severity | Vector String                                                   |
| ------------ | ----- | -------- | --------------------------------------------------------------- |
| 3.1          | 7.6   | High     | CVSS:3.1/AV:N/AC:L/PR:H/UI:N/S:C/C:H/I:L/A:N                    |
| 4.0          | 7.0   | High     | CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:P/VC:H/VI:H/VA:N/SC:N/SI:N/SA:N |

---
**Steps to Reproduce**
1. Login to `/dashboard` as admin
		![](/assets/vulnerability_assets/cwe80-storedxss-samarium-header/login.png)

2. Navigate to **CMS > Posts**
		![](/assets/vulnerability_assets/cwe80-storedxss-samarium-header/gotoposts.png)

3. Create a new post
		![](/assets/vulnerability_assets/cwe80-storedxss-samarium-header/createnewpost.png)

4. Enter a name for the post and click **Submit**
		![](/assets/vulnerability_assets/cwe80-storedxss-samarium-header/newpost.png)

5. Go to **Add content**, select **Heading**
		![](/assets/vulnerability_assets/cwe80-storedxss-samarium-header/addheading.png)

6. Enter payload into field: `<script>alert(1)</script>`
		![](/assets/vulnerability_assets/cwe80-storedxss-samarium-header/payloaddrop.png)

7. Navigate back to `/` and scroll to the bottom of the page, then click **Read More** on the blog post to execute the payload
		![](/assets/vulnerability_assets/cwe80-storedxss-samarium-header/openpost.png)

8. View payload execution
		![](/assets/vulnerability_assets/cwe80-storedxss-samarium-header/payload.png)


---
**Recommendations**

1. Encode special characters such as `<`, `>`, `"`, and `&` so they will not be treated as executable code
2. Reject inputs into the vulnerable fields that include these suspicious characters
3. If the characters must be accepted, use a trusted HTML sanitizer such as the [OWASP Java HTML Sanitizer](https://owasp.org/www-project-java-html-sanitizer/)
4. Implement a content security policy that restricts script execution to trusted sources
5. Cookies should have the `HttpOnly` flag set to prevent JavaScript code from using the cookie, and the `Secure` flag should be set to force `HTTPS` over `HTTP`

*Credit: [OWASP](https://cwe.mitre.org/data/definitions/79.html)*
