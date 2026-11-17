## Title
Reflected XSS - RefSeek: Academic & Scientific Search Engine

## Description
**Affected URL:** [https://refseek.pwnedbox.site/](https://refseek.pwnedbox.site/index.php?q=)

The search functionality is vulnerable to Reflected Cross-Site Scripting (XSS). User-controlled input through the `q` parameter is reflected into the search results page without proper output encoding, allowing injected JavaScript to execute in the visitor's browser.

## Steps to Reproduce


1. Open the following crafted URL in a browser:

   https://refseek.pwnedbox.site/index.php?q=Priyanshxop%27%22%3C%3E%3Cscript%3Ealert(1)%3C%2Fscript%3E

2. Allow the page to load with the crafted payload supplied through the `q` parameter.
3. Observe the JavaScript alert displaying `1`.
4. Dismiss the alert.
5. Inspect the page source.
6. Locate the search results heading and result entries.
7. Verify that the injected payload is reflected in these locations without proper HTML output encoding.

## POC (Proof of Concept)

Screenshot 1 – XSS payload entered in the search box.

<img width="1917" height="965" alt="image" src="https://github.com/user-attachments/assets/b5893868-4f1c-4680-8e77-ace3f8eb8c16" />

Screenshot 2 – JavaScript alert successfully executed.

<img width="1917" height="968" alt="image" src="https://github.com/user-attachments/assets/e9412479-5318-485f-936d-0460f8f0575b" />

Screenshot 3 – Reflected payload visible in the page source.

<img width="1918" height="958" alt="image" src="https://github.com/user-attachments/assets/06625b07-c998-4e93-b0a5-1eb594de6526" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
