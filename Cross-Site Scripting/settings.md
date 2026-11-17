## Title
Reflected XSS - Mixed Security Parameters

## Description
**Affected URL:** https://settings.pwnedbox.site/

The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the `academy` GET parameter. During testing, multiple GET parameters were identified using x8scan. Further testing of the `academy` parameter revealed that specially crafted input containing a mixed-case script tag and Unicode escape sequence was reflected in the response and successfully executed JavaScript in the browser.

The vulnerability occurs because user-controlled input is not properly encoded before being included in the HTML response. Successful execution was confirmed through a JavaScript alert displaying `1`.

## Steps to Reproduce

1. Open https://settings.pwnedbox.site/?academy=Priyanshxop%27%22%3C%3E%3CScRipT%3Ea\u006cert(1)%3C/ScRipT%3E.
2. Run x8scan to identify the available GET parameters.
3. Identify the `academy` parameter from the discovered parameter list.
4. Inject the crafted XSS payload `Priyanshxop%27"<><ScRipT>a\u006cert(1)</ScRipT>` into the `academy` parameter.
5. Open the modified URL in a browser.
6. Observe the reflected input on the webpage.
7. Confirm that a JavaScript alert displaying `1` appears.
8. Inspect the page source to verify the reflected payload.


## POC (Proof of Concept)

Screenshot 1 – x8scan identifies multiple GET parameters on the application.

<img width="1903" height="340" alt="image" src="https://github.com/user-attachments/assets/0c5450cf-f747-4b86-94ac-9874cce8eb39" />

Screenshot 2 – Crafted payload submitted through the academy parameter and reflected on the webpage.

<img width="1918" height="962" alt="image" src="https://github.com/user-attachments/assets/bda4fe67-0276-450b-8d0d-4516a4b4f516" />

Screenshot 3 – JavaScript alert displaying 1, confirming successful XSS execution.

<img width="1917" height="976" alt="image" src="https://github.com/user-attachments/assets/34b859ba-688a-4597-850b-cdaf48c371b4" />

Screenshot 4 – Page source showing the reflected payload.

<img width="1916" height="961" alt="image" src="https://github.com/user-attachments/assets/ff98806c-fef6-4db9-80a7-9a3ce0d4bd1f" />

## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
