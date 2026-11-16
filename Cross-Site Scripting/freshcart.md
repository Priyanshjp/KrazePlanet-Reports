## Title
Reflected XSS - FreshCart: Online Grocery Store

## Description
**Affected URL:** https://freshcart.pwnedbox.site/

The search functionality is vulnerable to Reflected Cross-Site Scripting (XSS). User-controlled input through the `q` parameter is reflected in the search results page without proper output encoding. A crafted search query can execute JavaScript in a visitor's browser when the resulting URL is accessed. Successful execution was confirmed using a JavaScript alert.

## Steps to Reproduce

1. Open https://freshcart.pwnedbox.site/ in your browser.
2. Navigate to the product search box and identify the `q` parameter.
3. Enter the following test input: `Priyanshxop'"<><script>alert(1)</script>`.
4. Submit the search query and observe the JavaScript alert.
5. Open the page source and locate the reflected input inside the search status message.
6. Confirm that the injected JavaScript executes successfully.

## POC (Proof of Concept)

Screenshot 1 – XSS payload entered in the product search box.

<img width="1918" height="966" alt="image" src="https://github.com/user-attachments/assets/d0b3f8cc-28bd-4416-9463-b1aa0a2493b8" />

Screenshot 2 – JavaScript alert successfully executed.

<img width="1912" height="962" alt="image" src="https://github.com/user-attachments/assets/5e67c25d-e90d-40d4-8f81-638211496f04" />

Screenshot 3 – Reflected payload visible in the page source.

<img width="1918" height="955" alt="image" src="https://github.com/user-attachments/assets/20c8bdfa-245c-4a2e-8f8f-f0d9287d9314" />


## Impact

- Execute JavaScript in a visitor's browser.
- Manipulate displayed product information or page content.
- Conduct phishing attacks using injected content.

## Remediation

1. **Output Encoding:** Apply context-aware encoding before displaying search input.
2. **Input Validation:** Validate and sanitize user-controlled search parameters.
3. **Content Security Policy:** Implement a restrictive CSP to reduce the impact of script injection.
