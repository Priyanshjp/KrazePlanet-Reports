## Title
Reflected XSS - Script & Img Tag Filter

## Description
**Affected URL:** https://support.pwnedbox.site/

The application accepts First Name (`fname`) and Last Name (`lname`) values through GET parameters. During testing, HTML event-handler payloads were submitted through both parameters.

The application reflected the supplied values inside the HTML input elements without proper context-aware output encoding. By injecting a crafted `<body>` element with an `onpageshow` event handler, JavaScript execution was achieved in the browser. This confirms a Reflected Cross-Site Scripting (XSS) vulnerability affecting both parameters.

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   https://support.pwnedbox.site/?fname=%27%22%3C%3E%3Cbody%20onpageshow%3Dalert(1)%3E&lname=%27%22%3C%3E%3Cbody%20onpageshow%3Dalert(1)%3E

2. Allow the page to load with the crafted payload supplied through both parameters.
3. Observe the JavaScript alert displaying `1`.
4. Dismiss the alert.
5. Inspect the page source.
6. Locate the First Name and Last Name input elements.
7. Verify that the supplied payload is reflected inside both input elements without proper HTML attribute encoding.


## POC (Proof of Concept)

Screenshot 1 – Crafted payload entered into both First Name and Last Name fields.

<img width="1918" height="958" alt="image" src="https://github.com/user-attachments/assets/567363c4-ecc3-4b2e-9710-68768880cfe2" />

Screenshot 2 – JavaScript alert displaying `1` executes successfully after submission.

<img width="1918" height="956" alt="image" src="https://github.com/user-attachments/assets/b1f85ff7-d880-4534-ae91-4c34ab3be7fb" />

Screenshot 3 – Page source confirms unescaped payload reflection in both `fname` and `lname` input elements.

<img width="1916" height="963" alt="image" src="https://github.com/user-attachments/assets/e8ddb56e-c7cb-4fec-8af0-864dca0e9da5" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
