## Title
Reflected XSS - Encoding Bypass Attempts

## Description
**Affected URL:** https://mail.pwnedbox.site/

The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the `cat` GET parameter. During testing, a crafted payload using a mixed-case script tag and Unicode escape sequence successfully bypassed the application's input handling and executed JavaScript in the browser.

The vulnerability occurs because user-controlled input is reflected into the response without sufficient context-aware output encoding. Successful execution was confirmed through a JavaScript alert displaying `1`.

## Steps to Reproduce

1. Open https://mail.pwnedbox.site/?cat=Priyanshxop%27%22%3C%3E%3CScRipT%3Ea\u006cert(1)%3C/ScRipT%3E.
2. Use x8scan to identify the available GET parameters.
3. Identify the `cat` parameter as the testing target.
4. Inject the crafted XSS payload into the `cat` parameter.
5. Load the modified URL in the browser.
6. Observe the JavaScript alert displaying `1`.
7. Inspect the page source to verify the reflected input.

## POC (Proof of Concept)

Screenshot 1 – x8scan identifies the available GET parameters.

<img width="1907" height="302" alt="image" src="https://github.com/user-attachments/assets/3d8b51ae-f975-400f-86c6-b23cc49768fa" />

Screenshot 2 – Crafted payload submitted through the cat parameter.

<img width="1917" height="963" alt="image" src="https://github.com/user-attachments/assets/d1caa509-175b-44ac-ad80-9afac52229ff" />

Screenshot 3 – JavaScript alert displaying 1, confirming successful execution.

<img width="1917" height="952" alt="image" src="https://github.com/user-attachments/assets/7be00404-b80e-4eea-9859-9f7b6191d19b" />

Screenshot 4 – Page source showing the reflected payload.

<img width="1918" height="963" alt="image" src="https://github.com/user-attachments/assets/49be362a-7db6-4613-bd1e-48270c738db5" />

## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
