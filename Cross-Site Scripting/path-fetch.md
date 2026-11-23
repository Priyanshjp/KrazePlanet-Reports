## Title
Reflected XSS - Path Based

## Description
**Affected URL:** https://path-fetch.pwnedbox.site/

A reflected Cross-Site Scripting (XSS) vulnerability was identified in the URL path of the KrazePlanetLabs lab page. User-controlled path content is reflected into the HTML form's `action` attribute without proper output encoding. By crafting a URL containing an XSS payload, JavaScript executes in the browser when the page is loaded. The issue is demonstrated by a JavaScript alert and confirmed through the page source.

## Steps to Reproduce

1. Open the following crafted URL in a browser: https://path-fetch.pwnedbox.site/labs%20Priyanshxop'%22%3C%3E%3CScRipT%3Ealert(1)%3C/ScRipT%3E
2. Allow the page to load.
3. Observe the JavaScript alert displaying `1`.
4. Dismiss the alert.
5. View the page source.
6. Locate the form's `action` attribute and observe that the supplied path content is reflected without proper encoding.


## POC (Proof of Concept)

Screenshot 1: Crafted URL containing the path-based XSS payload.

<img width="1918" height="965" alt="image" src="https://github.com/user-attachments/assets/d0e05317-905f-4495-a921-5007043a2358" />

Screenshot 2: JavaScript alert displaying 1, confirming script execution.

<img width="1918" height="957" alt="image" src="https://github.com/user-attachments/assets/f55f3112-2926-47c7-9078-78a16eb08c3e" />

Screenshot 3: Page source showing the payload reflected in the form's action attribute.

<img width="1918" height="951" alt="image" src="https://github.com/user-attachments/assets/93d79438-3eea-4d17-a4fa-f4d39ff488c0" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
