---
layout: post
title: "cwe80-storedxss-openmrs-hl7"
date: 2026-05-31
categories: vulnerability-research
---

**Description**

In OpenMRS version 2.4, a stored cross-site scripting (XSS) vulnerability exists in the `/openmrs/admin/hl7/hl7Source.form` component. Arbitrary JavaScript code execution is possible in the context of users' browsers by entering a payload into the **Name** or **Description** field. The code is executed upon visiting the `/openmrs/admin/hl7/hl7Source.list` page. It is possible to steal cookies and data, deface the web page, or redirect users by exploiting this vulnerability.
- Vulnerable Endpoint: `/openmrs/admin/hl7/hl7Source.form`
- Vulnerable Parameters: name and description
- Payloads: `<script>alert("Payload in name field")</script>` and `<script>alert("Payload in description field")</script>`

---
**CVSS**

| CVSS Version | Score | Severity | Vector String                                |
| ------------ | ----- | -------- | -------------------------------------------- |
| 3.1          | 6.5   | Medium   | CVSS:3.1/AV:N/AC:L/PR:H/UI:N/S:U/C:H/I:H/A:N |
| 4.0          | 8.4   | High     | CVSS:4.0/AV:N/AC:L/AT:N/PR:H/UI:P/VC:H/VI:H/VA:N/SC:N/SI:N/SA:N |

---
**Steps to Reproduce**

1. Login as user with the *System Developer* role

2. Navigate to `/openmrs/admin/hl7/hl7Source.list` and click **Add HL7 Source**
		![](/assets/vulnerability_assets/cwe80-storedxss-openmrs-hl7/Pasted%20image%2020260520173531.png)

3. Enter payload into name or description field and click **Save HL7 Source**
		![](/assets/vulnerability_assets/cwe80-storedxss-openmrs-hl7/Pasted%20image%2020260520174003.png)

4. The payload will execute immediately as well as any time `/openmrs/admin/hl7/hl7Source.list` is visited by any user
		![](/assets/vulnerability_assets/cwe80-storedxss-openmrs-hl7/image.png)
		![](/assets/vulnerability_assets/cwe80-storedxss-openmrs-hl7/Pasted%20image%2020260520174102.png)

---
**Recommendations**

1. Encode special characters such as `<`, `>`, `"`, and `&` so they will not be treated as executable code
2. Reject inputs into the vulnerable fields that include these suspicious characters
3. If the characters must be accepted, use a trusted HTML sanitizer such as the [OWASP Java HTML Sanitizer](https://owasp.org/www-project-java-html-sanitizer/)
4. Implement a content security policy that restricts script execution to trusted sources
5. Cookies should have the `HttpOnly` flag set to prevent JavaScript code from using the cookie, and the `Secure` flag should be set to force `HTTPS` over `HTTP`

*Credit: [OWASP](https://cwe.mitre.org/data/definitions/79.html)*
