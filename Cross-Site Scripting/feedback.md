## Title
Reflected XSS - Script Tag Filter Evasion

## Description
**Affected URL:** https://feedback.pwnedbox.site/

The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the `fname` and `lname` GET parameters. User-controlled input is reflected directly into HTML input fields without proper output encoding. During testing, a mixed-case script tag (`<ScRipT>`) successfully executed JavaScript, demonstrating that the application's filtering mechanism does not adequately handle variations in HTML tag casing. This allows an attacker to inject executable JavaScript through a crafted URL when a victim visits it.

## Steps to Reproduce

1. Open https://feedback.pwnedbox.site/.
2. Enter a crafted value into the First Name and Last Name fields.
3. Use the following payload: `Priyanshxop'"<><ScRipT>alert(1)</ScRipT>`
4. Submit the form.
5. Observe the JavaScript alert in the browser.
6. Inspect the page source and verify that the injected payload is reflected inside both input fields.

## POC (Proof of Concept)

Screenshot 1 – Payload entered into both name fields.

<img width="1917" height="972" alt="image" src="https://github.com/user-attachments/assets/06f91aa8-2309-43a0-9293-0732626f6717" />

Screenshot 2 – Reflected payload visible in the page.

<img width="1918" height="966" alt="image" src="https://github.com/user-attachments/assets/f50beb80-4a7c-41f5-976c-9198b2460fbd" />

Screenshot 4 – HTML source showing the reflected payload.

<img width="1918" height="966" alt="image" src="https://github.com/user-attachments/assets/623ab475-b910-4833-9f19-555580761553" />


## Impact

- Execute arbitrary JavaScript in a visitor's browser.
- Manipulate page content and form elements.
- Potentially conduct phishing attacks.
- Abuse the vulnerability through crafted links.

## Remediation

1. Apply context-aware HTML attribute encoding.
2. Validate user input on the server side.
3. Avoid relying on case-sensitive blacklist filtering.
4. Implement a restrictive Content Security Policy (CSP).
