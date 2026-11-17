## Title
Reflected XSS - Multi-Parameter Filter Evasion

## Description
**Affected URL:** https://checkout.pwnedbox.site/

The application accepts user-controlled input through multiple GET parameters. During testing, the `cat` parameter was found to reflect supplied input in the HTTP response.

A crafted input containing HTML and JavaScript markup was submitted to evaluate the application's filtering and output handling. The response displayed parts of the supplied input, indicating that user-controlled data is reflected without adequate context-aware handling. JavaScript execution still requires separate confirmation.

## Steps to Reproduce

1. Identify the GET parameters of `https://checkout.pwnedbox.site/` using x8scan.
2. Select the `cat` parameter for testing.
3. Open the following crafted URL in a browser:

   [https://checkout.pwnedbox.site/?cat=Priyanshxop%27%22%3C%3EScRipT%3Ea%5Cu006cert(1)%3C/ScRipT%3E](https://checkout.pwnedbox.site/?cat=Priyanshxop%27%22%3C%3E%3CScRipT%3Ea\u006cert(1)%3C/ScRipT%3E)

4. Allow the page to load with the crafted `cat` parameter.
5. Observe how the supplied value is reflected in the page response.
6. Inspect the page source and verify how the `cat` parameter is handled.

## POC (Proof of Concept)

Screenshot 1 – x8scan identifies the available GET parameters.

<img width="1918" height="282" alt="image" src="https://github.com/user-attachments/assets/7c33bd81-0bf2-4fa4-a254-aa62db1a53ba" />

Screenshot 2 – Crafted input submitted through the `cat` parameter.

<img width="1918" height="966" alt="image" src="https://github.com/user-attachments/assets/6fd22784-3db7-4e2c-ab34-bb9b92ee1db0" />

Screenshot 3 – Reflected input visible in the browser response.

<img width="1918" height="963" alt="image" src="https://github.com/user-attachments/assets/d9f26f94-78e2-4dad-a142-af4b707d1fbb" />

Screenshot 4 – Page source showing the reflected input.

<img width="1917" height="961" alt="image" src="https://github.com/user-attachments/assets/a5484127-770e-4410-a735-8923093c6f63" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
