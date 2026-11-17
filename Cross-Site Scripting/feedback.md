## Title
Reflected XSS - Script Tag Filter Evasion

## Description
**Affected URL:** https://feedback.pwnedbox.site/

The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the `fname` and `lname` GET parameters. User-controlled input is reflected directly into HTML input fields without proper output encoding. During testing, a mixed-case script tag (`<ScRipT>`) successfully executed JavaScript, demonstrating that the application's filtering mechanism does not adequately handle variations in HTML tag casing. This allows an attacker to inject executable JavaScript through a crafted URL when a victim visits it.

## Steps to Reproduce

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   https://feedback.pwnedbox.site/?fname=Priyanshxop&lname=Priyanshxop%27%22%3C%3E%3CScRipT%3Ealert%281%29%3C%2FScRipT%3E

2. Allow the page to load with the crafted `fname` and `lname` parameters.
3. Observe the JavaScript alert displaying `1`.
4. Click **OK** to dismiss the alert.
5. Inspect the page source.
6. Locate the reflected `fname` and `lname` values.
7. Verify that the supplied payload is reflected inside both input fields without proper HTML encoding.

## POC (Proof of Concept)

Screenshot 1 – Payload entered into both name fields.

<img width="1917" height="972" alt="image" src="https://github.com/user-attachments/assets/06f91aa8-2309-43a0-9293-0732626f6717" />

Screenshot 2 – Reflected payload visible in the page.

<img width="1918" height="966" alt="image" src="https://github.com/user-attachments/assets/f50beb80-4a7c-41f5-976c-9198b2460fbd" />

Screenshot 4 – HTML source showing the reflected payload.

<img width="1918" height="966" alt="image" src="https://github.com/user-attachments/assets/623ab475-b910-4833-9f19-555580761553" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
