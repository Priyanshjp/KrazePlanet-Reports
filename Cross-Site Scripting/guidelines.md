## Title
Reflected XSS - GIGW: Government Website Guidelines Portal

## Description
**Affected URL:** https://guidelines.pwnedbox.site/index.php?s=

The search functionality is vulnerable to Reflected Cross-Site Scripting (XSS). User-supplied input through the `s` parameter is reflected into the HTML response without proper output encoding. By submitting a specially crafted search query, an attacker can execute arbitrary JavaScript in a visitor's browser when the crafted URL is accessed. The vulnerability was confirmed through successful JavaScript alert execution.

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   https://guidelines.pwnedbox.site/index.php?s=Priyanshxop%27%22%3C%3E%3Cscript%3Ealert(1)%3C%2Fscript%3E

2. Allow the page to load with the crafted value supplied through the `s` parameter.
3. Observe the JavaScript alert displaying `1`.
4. Dismiss the alert.
5. Inspect the page source.
6. Locate the reflected search input in the HTML response.
7. Verify that the injected payload is reflected without proper HTML output encoding.

## POC (Proof of Concept)

Screenshot 1 – Search results page displaying the submitted input.

<img width="1918" height="965" alt="image" src="https://github.com/user-attachments/assets/c334e543-92ae-4166-b8ad-14247932deb0" />

Screenshot 2 – JavaScript alert successfully executed.

<img width="1918" height="965" alt="image" src="https://github.com/user-attachments/assets/ff95e03f-8e4d-4834-8534-4e6bd4eb6a75" />

Screenshot 3 – Injected payload reflected in the page source.

<img width="1918" height="967" alt="image" src="https://github.com/user-attachments/assets/05399d5d-c573-46e5-bea5-972fe976a08c" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
