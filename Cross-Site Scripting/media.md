## Title
Reflected XSS - Imgur Mobile

## Description
**Affected URL:** https://media.pwnedbox.site/

The application accepts a username through the GET parameter and reflects the supplied value in the page content. During testing, a JavaScript payload was submitted through the `username` parameter and successfully executed in the browser.

Source inspection also revealed that the username is inserted into the profile link's `href` attribute without proper HTML attribute encoding. This allows an attacker to inject HTML elements and execute JavaScript in the victim's browser.

## Steps to Reproduce

1. Open https://media.pwnedbox.site/.
2. Identify the `username` GET parameter.
3. Submit the following payload through the parameter: `Priyanshxop'"<><script>alert(1)</script>`
4. Load the crafted URL in the browser.
5. Observe the JavaScript alert displaying `1`.
6. Inspect the page source and locate the reflected username in the profile link.
7. Verify that the injected content appears inside the `href` attribute.

## POC (Proof of Concept)

Screenshot 1 – x8scan output showing the transformed URL and `username` parameter.

<img width="1916" height="265" alt="image" src="https://github.com/user-attachments/assets/f4825ef2-d556-4cec-83c3-4424e597f0e6" />

Screenshot 2 – Application page displaying the reflected username.

<img width="1916" height="967" alt="image" src="https://github.com/user-attachments/assets/49dc4fcf-0200-4477-b089-ba7ee8d731c0" />

Screenshot 3 – JavaScript alert displaying `1`, confirming successful execution.

<img width="1911" height="961" alt="image" src="https://github.com/user-attachments/assets/4a31458e-2660-4221-a98c-94e731940890" />

Screenshot 4 – Page source showing the injected payload reflected inside the profile link's `href` attribute.

<img width="1918" height="952" alt="image" src="https://github.com/user-attachments/assets/78c1e019-cf78-4513-9586-18ac5ebfdcb2" />


## Impact

- **Arbitrary JavaScript Execution:** An attacker may execute JavaScript in a victim's browser by sending a crafted URL.
- **Phishing and Content Manipulation:** Malicious scripts may modify page content or display deceptive elements.
- **Targeted User Attacks:** Attackers may distribute crafted links to target users and exploit the vulnerability when they visit the URL.
  
## Remediation

1. **Context-Aware Output Encoding:** Apply proper HTML attribute encoding before inserting user-controlled values into `href` attributes.
2. **Input Validation:** Validate and safely handle the `username` GET parameter, rejecting unexpected HTML and script content where appropriate.
3. **Content Security Policy:** Implement a restrictive CSP and avoid relying on input filtering as the primary XSS defense.
