## Title
Reflected XSS - TutorialRepublic: Web Development Reference Search

## Description
**Affected URL:** https://tutorialrepublic.pwnedbox.site/

The search functionality is vulnerable to Reflected Cross-Site Scripting (XSS). The application reflects user-supplied input from the `q` parameter into the search result page without proper output encoding. As a result, an attacker can inject JavaScript that executes in a visitor's browser when the crafted URL is opened.

## Steps to Reproduce
1. Open https://tutorialrepublic.pwnedbox.site/ and navigate to the search functionality.
2. Inject the XSS payload into the `q` parameter: `Priyanshxop'"<><script>alert(1)</script>`.
3. Submit the request and observe the JavaScript alert.
4. Inspect the page source and observe the payload reflected inside the search result message.
5. Confirm that the injected JavaScript executes in the browser.

## POC (Proof of Concept)
Screenshot 1 – Payload entered in the search box.

<img width="1918" height="966" alt="image" src="https://github.com/user-attachments/assets/3ce077e3-8ea5-48df-ab38-63c384b3432b" />

Screenshot 2 – JavaScript alert execution.

<img width="1918" height="967" alt="image" src="https://github.com/user-attachments/assets/9770d9de-6cc9-4e4a-b2be-31af721c5f91" />

Screenshot 3 – Reflected payload visible in page source.

<img width="1917" height="965" alt="image" src="https://github.com/user-attachments/assets/0671fd3d-d206-41eb-8616-9eeffca1db34" />


## Impact
- Execute arbitrary JavaScript in a victim's browser.
- Manipulate page content or perform actions in the victim's session, depending on application controls.
- Conduct phishing attacks by displaying malicious content.

## Remediation
1. **Output Encoding:** Apply context-aware HTML encoding to user-controlled input before rendering.
2. **Input Validation:** Validate and sanitize search parameters on the server.
3. **Content Security Policy:** Implement a restrictive CSP to reduce the impact of script injection.
