## Title
Reflected XSS - PubMed: National Library of Medicine Search Builder

## Description
**Affected URL:** https://pubmed.pwnedbox.site/

The search functionality is vulnerable to Reflected Cross-Site Scripting (XSS). User-controlled input through the `term` parameter is reflected into the search results page without proper output encoding, allowing injected JavaScript to execute in the visitor's browser.

## Steps to Reproduce

1. Open https://pubmed.pwnedbox.site/ in your browser.
2. Navigate to the search functionality and identify the `term` parameter.
3. Submit the following payload through the search parameter: `Priyanshxop'"<><script>alert(1)</script>`.
4. Observe the JavaScript alert displayed in the browser.
5. Inspect the page source and locate the reflected payload in the search results warning message.
   
## POC (Proof of Concept)

Screenshot 1 – XSS payload entered in the search box.

<img width="1916" height="960" alt="image" src="https://github.com/user-attachments/assets/3376c568-ebb5-4ba9-8657-fd1ce30a4d45" />

Screenshot 2 – JavaScript alert successfully executed.

<img width="1912" height="970" alt="image" src="https://github.com/user-attachments/assets/236f321e-dd20-442c-b830-195ad6ccaf98" />

Screenshot 3 – Reflected payload visible in the page source.

<img width="1911" height="960" alt="image" src="https://github.com/user-attachments/assets/460b13e5-27d9-4ee5-9934-8f9081a28735" />



## Impact

- Execute JavaScript in a visitor's browser.
- Manipulate displayed search results and page content.
- Conduct phishing attacks using injected content.

## Remediation

1. **Output Encoding:** Apply context-aware encoding to search terms before rendering them.
2. **Input Validation:** Validate and sanitize user-controlled search parameters.
3. **Content Security Policy:** Implement a restrictive CSP to reduce the impact of script injection.
