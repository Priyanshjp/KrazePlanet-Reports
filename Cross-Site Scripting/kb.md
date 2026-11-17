## Title
Reflected XSS - Search Filter Bypass


## Description
**Affected URL:** https://kb.pwnedbox.site/

The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the `search` GET parameter. User-supplied search input is reflected into the results heading and the empty-state message without proper output encoding.

During testing, a crafted payload using a mixed-case script tag and a Unicode escape sequence successfully executed JavaScript in the browser. This confirms that the search parameter can be used to inject executable content into the response.


## Steps to Reproduce

1. Open https://kb.pwnedbox.site/?search=Priyanshxop%27%22%3C%3E%3CScRipT%3Ea\u006cert(1)%3C/ScRipT%3E.
2. Enter a test value in the search field and submit it.
3. Identify the `search` parameter in the resulting URL.
4. Replace the search value with the crafted XSS payload `Priyanshxop%27"<><ScRipT>a\u006cert(1)</ScRipT>`.
5. Open the modified URL in the browser.
6. Observe the JavaScript alert displaying `1`.
7. Inspect the page source and verify that the payload is reflected in the results heading and empty-state message.

## POC (Proof of Concept)

Screenshot 1 – Search parameter populated with the normal test value.

<img width="1918" height="961" alt="image" src="https://github.com/user-attachments/assets/9069aaa3-9798-4dcc-a55e-a7c306081509" />

Screenshot 2 – Crafted payload in the URL and JavaScript alert displaying 1.

<img width="1917" height="962" alt="image" src="https://github.com/user-attachments/assets/979caba4-1dab-4d39-8c20-4431a5f34440" />

Screenshot 3 – Page source showing the reflected payload in the results heading and empty-state message.

<img width="1918" height="955" alt="image" src="https://github.com/user-attachments/assets/7c12a24e-c57a-4d3f-8c1f-bb6a2c303158" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
