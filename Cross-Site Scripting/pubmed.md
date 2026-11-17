## Title
Reflected XSS - PubMed: National Library of Medicine Search Builder

## Description
**Affected URL:** https://pubmed.pwnedbox.site/

The search functionality is vulnerable to Reflected Cross-Site Scripting (XSS). User-controlled input through the `term` parameter is reflected into the search results page without proper output encoding, allowing injected JavaScript to execute in the visitor's browser.The issue occurs when specially crafted search input is processed and displayed in the response. Proper output encoding is required to prevent this behavior.

## Steps to Reproduce


1. Open the following crafted URL in a browser:

   https://pubmed.pwnedbox.site/?term=Priyanshxop%27%22%3C%3E%3Cscript%3Ealert(1)%3C%2Fscript%3E

2. Allow the page to load with the crafted payload supplied through the `term` parameter.
3. Observe the JavaScript alert displaying `1`.
4. Dismiss the alert.
5. Inspect the page source and locate the search results warning message.
6. Verify that the injected payload is reflected in the warning message without proper HTML output encoding.
   
## POC (Proof of Concept)

The behavior was reproducible during testing in the provided lab environment.

Screenshot 1 – XSS payload entered in the search box.

<img width="1916" height="960" alt="image" src="https://github.com/user-attachments/assets/3376c568-ebb5-4ba9-8657-fd1ce30a4d45" />

Screenshot 2 – JavaScript alert successfully executed.

<img width="1912" height="970" alt="image" src="https://github.com/user-attachments/assets/236f321e-dd20-442c-b830-195ad6ccaf98" />

The alert confirms that the browser interpreted the reflected input as executable JavaScript during testing.

Screenshot 3 – Reflected payload visible in the page source.

<img width="1911" height="960" alt="image" src="https://github.com/user-attachments/assets/460b13e5-27d9-4ee5-9934-8f9081a28735" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
