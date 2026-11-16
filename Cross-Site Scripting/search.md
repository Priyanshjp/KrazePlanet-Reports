## Title
Reflected XSS - Global Site Search Portal

## Description
**Affected URL:** https://search.pwnedbox.site/

The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the `fname` and `lname` GET parameters. User-supplied input is reflected into HTML input fields without proper output encoding, allowing injected JavaScript to execute in the browser.

## Steps to Reproduce


1. Open https://search.pwnedbox.site/ in your browser.
2. Enter a test payload `Priyanshxop'"<><script>alert(1)</script>` in the First Name and Last Name fields.
3. Submit the form and observe the JavaScript alert.
4. Inspect the URL and identify the `fname` and `lname` parameters.
5. Open the page source and verify that the payload is reflected inside both input fields.

## POC (Proof of Concept)

Screenshot 1 – Payload entered in the First Name and Last Name fields.

<img width="1918" height="962" alt="image" src="https://github.com/user-attachments/assets/26f884f6-4aed-4dab-bad0-a8c3589afd94" />

Screenshot 2 – JavaScript alert successfully executed.

<img width="1915" height="970" alt="image" src="https://github.com/user-attachments/assets/d6c863d0-29af-4caa-9d09-0ed38365fb68" />

Screenshot 3 – Reflected payload visible in both HTML input fields in page source.

<img width="1918" height="963" alt="image" src="https://github.com/user-attachments/assets/37aed823-652a-4c27-981c-20fc0d3f4795" />


## Impact

- Execute JavaScript in a visitor's browser.
- Manipulate page content or input fields.
- Conduct phishing attacks using injected content.

## Remediation

1. **Output Encoding:** Properly HTML-attribute-encode user input before rendering it.
2. **Input Validation:** Validate and sanitize both GET parameters.
3. **Content Security Policy:** Implement a restrictive CSP to reduce the impact of script injection.
