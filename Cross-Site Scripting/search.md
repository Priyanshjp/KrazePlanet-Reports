## Title
Reflected XSS - Global Site Search Portal

## Description
**Affected URL:** https://search.pwnedbox.site/

The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the `fname` and `lname` GET parameters. User-supplied input is reflected into HTML input fields without proper output encoding, allowing injected JavaScript to execute in the browser.

## Steps to Reproduce


1. Open the following crafted URL in a browser:

   https://search.pwnedbox.site/?fname=Priyanshxop%27%22%3C%3E%3Cscript%3Ealert(1)%3C%2Fscript%3E&lname=Priyanshxop%27%22%3C%3E%3Cscript%3Ealert(1)%3C%2Fscript%3E

2. Allow the page to load with the crafted payloads supplied through the `fname` and `lname` parameters.
3. Observe the JavaScript alert displaying `1`.
4. Dismiss the alert.
5. Inspect the URL and verify that both `fname` and `lname` parameters contain the supplied payload.
6. Inspect the page source and locate the First Name and Last Name input fields.
7. Verify that the injected values are reflected inside both input fields without proper HTML attribute encoding.

## POC (Proof of Concept)

Screenshot 1 – Payload entered in the First Name and Last Name fields.

<img width="1918" height="962" alt="image" src="https://github.com/user-attachments/assets/26f884f6-4aed-4dab-bad0-a8c3589afd94" />

Screenshot 2 – JavaScript alert successfully executed.

<img width="1915" height="970" alt="image" src="https://github.com/user-attachments/assets/d6c863d0-29af-4caa-9d09-0ed38365fb68" />

Screenshot 3 – Reflected payload visible in both HTML input fields in page source.

<img width="1918" height="963" alt="image" src="https://github.com/user-attachments/assets/37aed823-652a-4c27-981c-20fc0d3f4795" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
