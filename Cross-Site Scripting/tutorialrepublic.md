## Title
Reflected XSS - TutorialRepublic: Web Development Reference Search

## Description
**Affected URL:** https://tutorialrepublic.pwnedbox.site/?q=

The search functionality is vulnerable to Reflected Cross-Site Scripting (XSS). The application reflects user-supplied input from the `q` parameter into the search result page without proper output encoding. As a result, an attacker can inject JavaScript that executes in a visitor's browser when the crafted URL is opened.

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   https://tutorialrepublic.pwnedbox.site/?q=Priyanshxop%27%22%3C%3E%3Cscript%3Ealert(1)%3C%2Fscript%3E

2. Allow the page to load with the crafted payload supplied through the `q` parameter.
3. Observe the JavaScript alert displaying `1`.
4. Dismiss the alert.
5. Inspect the page source and locate the search result message.
6. Verify that the injected payload is reflected in the message without proper HTML output encoding.
7. Confirm that the injected JavaScript executes in the browser.
   
## POC (Proof of Concept)
Screenshot 1 – Payload entered in the search box.

<img width="1918" height="966" alt="image" src="https://github.com/user-attachments/assets/3ce077e3-8ea5-48df-ab38-63c384b3432b" />

Screenshot 2 – JavaScript alert execution.

<img width="1918" height="967" alt="image" src="https://github.com/user-attachments/assets/9770d9de-6cc9-4e4a-b2be-31af721c5f91" />

Screenshot 3 – Reflected payload visible in page source.

<img width="1917" height="965" alt="image" src="https://github.com/user-attachments/assets/0671fd3d-d206-41eb-8616-9eeffca1db34" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
