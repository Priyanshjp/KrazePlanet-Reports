## Title
Reflected XSS - Category Filter


## Description
**Affected URL:** https://shop.pwnedbox.site/

The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the `sort` GET parameter. User-controlled input is reflected into the page's sorting label and category navigation links without proper output encoding.

During testing, a crafted payload containing a mixed-case script tag and a Unicode escape sequence successfully executed JavaScript in the browser. This demonstrates that an attacker can inject executable JavaScript through a specially crafted URL.


## Steps to Reproduce

1. Open the following crafted URL in a browser:

   `https://shop.pwnedbox.site/?category=all&sort=Priyanshxop%27%22%3E%3CScRipT%3Ea\u006cert(1)%3C/ScRipT%3E`

2. Observe the JavaScript alert displaying `1`.
3. Dismiss the alert.
4. Inspect the page source.
5. Verify that the supplied payload is reflected in the sorting label and category filter links.

## POC (Proof of Concept)

Screenshot 1 – Shop page showing the original category and sorting options.

<img width="1918" height="967" alt="image" src="https://github.com/user-attachments/assets/e07875b9-2393-45d9-9169-6df1bc55f2c0" />

Screenshot 2 – JavaScript alert displaying 1 after opening the crafted URL.

<img width="1918" height="957" alt="image" src="https://github.com/user-attachments/assets/710dc389-aa48-49b9-9fd2-96d4109c682a" />

Screenshot 3 – Page source showing the reflected payload in the sorting label and category navigation links.

<img width="1917" height="961" alt="image" src="https://github.com/user-attachments/assets/889a5265-b0b2-4fdb-a584-543db9b963a6" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
