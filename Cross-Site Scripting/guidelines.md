## Title
Reflected XSS - GIGW: Government Website Guidelines Portal

## Description
**Affected URL:** https://guidelines.pwnedbox.site/index.php?s=

## Steps to Reproduce

1. Open https://guidelines.pwnedbox.site/index.php?s= in the browser.
2. Navigate to the search functionality and identify the `s` parameter.
3. Submit the following payload through the search parameter: `Priyanshxop'"<><script>alert(1)</script>`.
4. Observe the JavaScript alert displayed by the application.
5. Inspect the page source and verify that the injected payload is reflected in the HTML response.

## POC (Proof of Concept)

Screenshot 1 – Search results page displaying the submitted input.

<img width="1918" height="965" alt="image" src="https://github.com/user-attachments/assets/c334e543-92ae-4166-b8ad-14247932deb0" />

Screenshot 2 – JavaScript alert successfully executed.

<img width="1918" height="965" alt="image" src="https://github.com/user-attachments/assets/ff95e03f-8e4d-4834-8534-4e6bd4eb6a75" />

Screenshot 3 – Injected payload reflected in the page source.

<img width="1918" height="967" alt="image" src="https://github.com/user-attachments/assets/05399d5d-c573-46e5-bea5-972fe976a08c" />


## Impact

- Execute JavaScript in a visitor's browser.
- Manipulate page content or display malicious content.
- Conduct phishing attacks through injected scripts.

## Remediation

1. **Output Encoding:** Apply context-aware encoding before rendering user-controlled input.
2. **Input Validation:** Validate and sanitize search parameters on the server.
3. **Content Security Policy:** Implement a restrictive CSP to reduce the impact of script injection.
