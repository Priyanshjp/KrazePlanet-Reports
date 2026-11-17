## Title
Reflected XSS - String Concatenation Bypass


## Description
**Affected URL:** https://portal.pwnedbox.site/

The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the `cat` GET parameter. During testing, x8scan identified multiple GET parameters. Further testing revealed that the `cat` parameter reflects user-controlled input into the application response without sufficient output encoding.

A crafted payload using a mixed-case script tag and a Unicode escape sequence successfully bypassed the input handling and executed JavaScript in the browser. The execution was confirmed through an alert displaying `1`.


## Steps to Reproduce

1. Open https://portal.pwnedbox.site/?cat=Priyanshxop%27%22%3C%3E%3CScRipT%3Ea\u006cert(1)%3C/ScRipT%3E.
2. Run x8scan to identify the available GET parameters.
3. Identify the `cat` parameter in the scan results.
4. Inject the crafted XSS payload `Priyanshxop%27"<><ScRipT>a\u006cert(1)</ScRipT>` into the `cat` parameter.
5. Open the modified URL in a browser.
6. Observe the reflected input on the webpage.
7. Confirm that a JavaScript alert displaying `1` appears.
8. Inspect the page source to verify the reflected payload.


## POC (Proof of Concept)

Screenshot 1 – x8scan identifies multiple GET parameters on the target application.

<img width="1901" height="277" alt="image" src="https://github.com/user-attachments/assets/fc013876-0696-461c-8699-a385528ea2cc" />

Screenshot 2 – Crafted payload supplied through the cat parameter and reflected on the webpage.

<img width="1918" height="967" alt="image" src="https://github.com/user-attachments/assets/6a9a61d8-50d3-419b-9db6-36a6ea590f96" />

Screenshot 3 – JavaScript alert displaying 1, confirming successful execution.

<img width="1918" height="965" alt="image" src="https://github.com/user-attachments/assets/6fcc46f9-5a0e-4159-bbe5-e15a82f3c045" />

Screenshot 4 – Page source showing the reflected payload.

<img width="1918" height="961" alt="image" src="https://github.com/user-attachments/assets/1b4b19b7-84e5-4cdc-9817-9616507846df" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
