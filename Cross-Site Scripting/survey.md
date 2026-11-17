## Title
Self XSS - POST XSS in Input Tag Value


## Description
**Affected URL:** https://survey.pwnedbox.site/

The Survey page contains a Reflected Cross-Site Scripting (XSS) vulnerability in the Name input field.

The application reflects user-controlled input directly into the HTML `value` attribute without proper HTML attribute encoding. A specially crafted payload can break out of the input attribute context and introduce a script element, resulting in JavaScript execution in the browser.

The vulnerability is demonstrated by a JavaScript alert displaying `1`.


## Steps to Reproduce

1. Open `https://survey.pwnedbox.site/` in a browser.
2. Locate the **Name** input field.
3. Enter the following crafted XSS payload: `Priyanshxop'"><ScRipT>\u0061lert(1);</ScRipT>`
4. Click the Enter button to submit the form.
5. Observe the JavaScript alert displaying 1.
6. Dismiss the alert.
7. Inspect the page source.
8. Locate the HTML input element and examine its value attribute.
9. Verify that the submitted payload is reflected inside the value attribute without proper HTML attribute encoding.

## POC (Proof of Concept)

Screenshot 1: Crafted XSS payload entered into the Name field.

<img width="1918" height="966" alt="image" src="https://github.com/user-attachments/assets/d84f19d4-97da-4964-8ebe-6674114ddfe8" />

Screenshot 2: JavaScript alert displaying 1.

<img width="1918" height="953" alt="image" src="https://github.com/user-attachments/assets/7e6e978e-0b77-4f69-9273-3b3bf3d56064" />

Screenshot 3: Page source showing the reflected payload inside the input value attribute.

<img width="1916" height="958" alt="image" src="https://github.com/user-attachments/assets/9b7769f0-97b1-494e-baf1-ba65cccbc9a0" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
