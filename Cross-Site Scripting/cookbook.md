## Title
Reflected XSS - Multi-Parameter Script Filter Evasion

## Description
**Affected URL:** https://cookbook.pwnedbox.site/

The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the `lname` GET parameter. During testing, the injected payload was reflected in the Last Name input field, while the `fname` parameter did not demonstrate the same behavior. A mixed-case script tag (`<ScRipT>`) successfully bypassed the filtering mechanism and executed JavaScript in the browser, displaying an alert popup. This indicates insufficient input handling and missing context-aware output encoding in the vulnerable parameter.

## Steps to Reproduce

1. Open https://cookbook.pwnedbox.site/.
2. Enter the following payload in the Last Name field: `Priyanshxop'"<><ScRipT>alert(1)</ScRipT>`
3. Submit the form.
4. Observe the JavaScript alert popup.
5. Inspect the page source and verify that the payload is reflected in the `lname` parameter.

## POC (Proof of Concept)

Screenshot 1 – Payload entered in the First Name and Last Name fields.

<img width="1918" height="962" alt="image" src="https://github.com/user-attachments/assets/fdd6c5dc-954d-46c9-9b37-f4c39aa7cb48" />

Screenshot 2 – JavaScript alert successfully executed.

<img width="1918" height="965" alt="image" src="https://github.com/user-attachments/assets/13650628-39e6-4734-b4b4-417e26ac7e82" />

Screenshot 3 – Reflected payload visible in the HTML page source.

<img width="1917" height="962" alt="image" src="https://github.com/user-attachments/assets/bd721903-d285-44a6-87e9-aeadc6045468" />

## Impact

- Execute JavaScript in a visitor's browser.
- Manipulate webpage content and display malicious elements.
- Conduct phishing attacks through specially crafted URLs.
  
## Remediation

1. **Output Encoding:** Apply context-aware HTML attribute encoding to user-controlled input.
2. **Input Validation:** Validate and safely handle values received through GET parameters.
3. **Secure Filtering:** Avoid blacklist-based filtering and implement a restrictive Content Security Policy (CSP).
