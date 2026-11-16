## Title
Reflected XSS - RefSeek: Academic & Scientific Search Engine

## Description
**Affected URL:** [https://refseek.pwnedbox.site/](https://refseek.pwnedbox.site/index.php?q=)

The search functionality is vulnerable to Reflected Cross-Site Scripting (XSS). User-controlled input through the `q` parameter is reflected into the search results page without proper output encoding, allowing injected JavaScript to execute in the visitor's browser.

## Steps to Reproduce


1. Open https://refseek.pwnedbox.site/ in your browser.
2. Navigate to the search functionality and identify the `q` parameter.
3. Submit the following payload through the search parameter: `Priyanshxop'"<><script>alert(1)</script>`.
4. Observe the JavaScript alert displayed in the browser.
5. Inspect the page source and locate the reflected payload in the search results heading and result entries.

## POC (Proof of Concept)

Screenshot 1 – XSS payload entered in the search box.

<img width="1917" height="965" alt="image" src="https://github.com/user-attachments/assets/b5893868-4f1c-4680-8e77-ace3f8eb8c16" />

Screenshot 2 – JavaScript alert successfully executed.

<img width="1917" height="968" alt="image" src="https://github.com/user-attachments/assets/e9412479-5318-485f-936d-0460f8f0575b" />

Screenshot 3 – Reflected payload visible in the page source.

<img width="1918" height="958" alt="image" src="https://github.com/user-attachments/assets/06625b07-c998-4e93-b0a5-1eb594de6526" />

## Impact

- Execute JavaScript in a visitor's browser.
- Manipulate search results and displayed page content.
- Conduct phishing attacks using injected content.
 
## Remediation

1. **Output Encoding:** Apply context-aware encoding before rendering search input and results.
2. **Input Validation:** Validate and sanitize user-controlled search parameters.
3. **Content Security Policy:** Implement a restrictive CSP to reduce the impact of script injection.
