## Title
Reflected XSS - Multi-Parameter Script Filter Evasion

## Description
**Affected URL:** https://cookbook.pwnedbox.site/

The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the `lname` GET parameter. During testing, the injected payload was reflected in the Last Name input field, while the `fname` parameter did not demonstrate the same behavior. A mixed-case script tag (`<ScRipT>`) successfully bypassed the filtering mechanism and executed JavaScript in the browser, displaying an alert popup. This indicates insufficient input handling and missing context-aware output encoding in the vulnerable parameter.

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   [`https://cookbook.pwnedbox.site/?lname=Priyanshxop%27%22%3C%3EScRipT%3Ealert%281%29%3C%2FScRipT%3E`](https://cookbook.pwnedbox.site/?fname=aa&lname=Priyanshxop%2527%22%3C%3E%3CScRipT%3Ea%5Cu006cert%281%29%3C%2FScRipT%3E)

2. Allow the page to load with the crafted `lname` parameter.
3. Observe the JavaScript alert displaying `1`.
4. Click **OK** to dismiss the alert.
5. Inspect the page source.
6. Verify that the supplied payload is reflected through the `lname` parameter.
## POC (Proof of Concept)

Screenshot 1 – Payload entered in the First Name and Last Name fields.

<img width="1918" height="962" alt="image" src="https://github.com/user-attachments/assets/fdd6c5dc-954d-46c9-9b37-f4c39aa7cb48" />

Screenshot 2 – JavaScript alert successfully executed.

<img width="1918" height="965" alt="image" src="https://github.com/user-attachments/assets/13650628-39e6-4734-b4b4-417e26ac7e82" />

Screenshot 3 – Reflected payload visible in the HTML page source.

<img width="1917" height="962" alt="image" src="https://github.com/user-attachments/assets/bd721903-d285-44a6-87e9-aeadc6045468" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
