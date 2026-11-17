## Title
Reflected XSS - FreshCart: Online Grocery Store

## Description
**Affected URL:** https://freshcart.pwnedbox.site/

The search functionality is vulnerable to Reflected Cross-Site Scripting (XSS). User-controlled input through the `q` parameter is reflected in the search results page without proper output encoding. A crafted search query can execute JavaScript in a visitor's browser when the resulting URL is accessed. Successful execution was confirmed using a JavaScript alert.

## Steps to Reproduce

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   https://freshcart.pwnedbox.site/?q=Priyanshxop%27%22%3C%3E%3Cscript%3Ealert(1)%3C%2Fscript%3E

2. Allow the page to load with the crafted `q` parameter.
3. Observe the JavaScript alert displaying `1`.
4. Click **OK** to dismiss the alert.
5. Inspect the page source.
6. Locate the reflected input inside the search status message.
7. Verify that the supplied payload is reflected and JavaScript execution occurs.
## POC (Proof of Concept)

Screenshot 1 – XSS payload entered in the product search box.

<img width="1918" height="966" alt="image" src="https://github.com/user-attachments/assets/d0b3f8cc-28bd-4416-9463-b1aa0a2493b8" />

Screenshot 2 – JavaScript alert successfully executed.

<img width="1912" height="962" alt="image" src="https://github.com/user-attachments/assets/5e67c25d-e90d-40d4-8f81-638211496f04" />

Screenshot 3 – Reflected payload visible in the page source.

<img width="1918" height="955" alt="image" src="https://github.com/user-attachments/assets/20c8bdfa-245c-4a2e-8f8f-f0d9287d9314" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
