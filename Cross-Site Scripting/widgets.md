## Title
Reflected XSS - Reddit

## Description
**Affected URL:** https://widgets.pwnedbox.site/

The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the `post_id` GET parameter. User-controlled input is directly inserted into the `id` attribute of the See More Comments button without proper HTML attribute encoding.

A crafted value can break out of the existing attribute and inject executable HTML/JavaScript. During testing, the injected JavaScript successfully displayed a browser alert, confirming the vulnerability.


## Steps to Reproduce

1. Open the following crafted URL in a browser:

   https://widgets.pwnedbox.site/?post_id=%22Priyanshxop%27%22%3C%3E%3Cscript%3Ealert(1)%3C/script%3E

2. Allow the page to load with the crafted `post_id` parameter.
3. Observe the JavaScript alert displaying `1`.
4. Click **OK** to dismiss the alert.
5. Open the page source and locate the See More Comments button.
6. Verify that the injected payload is reflected inside the button's `id` attribute.
7. Confirm that the reflected input resulted in JavaScript execution.

## POC (Proof of Concept)

Screenshot 1 – Crafted post_id payload in the URL and successful JavaScript alert execution.

<img width="1900" height="266" alt="image" src="https://github.com/user-attachments/assets/9ed9d004-0d97-41e5-8e74-8d6e9e15af94" />

Screenshot 2 – Page source showing the injected payload inside the See More Comments button's id attribute.

<img width="1918" height="960" alt="image" src="https://github.com/user-attachments/assets/499c65cc-9754-4249-bfef-c1bcad430ecb" />

Screenshot 3 – Parameter discovery using x8scan, identifying post_id.

<img width="1918" height="967" alt="image" src="https://github.com/user-attachments/assets/e602f2fb-30a4-46fe-92a7-64319628ce4e" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
