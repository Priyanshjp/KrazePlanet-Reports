## Title
Reflected XSS - Multi-Parameter Filter Evasion

## Description
**Affected URL:** https://checkout.pwnedbox.site/

The application accepts user-controlled input through multiple GET parameters. During testing, the `cat` parameter was found to reflect supplied input in the HTTP response.

A crafted input containing HTML and JavaScript markup was submitted to evaluate the application's filtering and output handling. The response displayed parts of the supplied input, indicating that user-controlled data is reflected without adequate context-aware handling. JavaScript execution still requires separate confirmation.

## Steps to Reproduce

1. Open https://checkout.pwnedbox.site/.
2. Identify the GET parameters using x8scan.
3. Select the `cat` parameter for testing.
4. Submit a crafted HTML and JavaScript payload `"Priyanshxop'"<><script>alert(1)</script>` through `cat`.
5. Observe the response and reflected input on the page.
6. Inspect the page source to verify how the supplied value is handled.

## POC (Proof of Concept)

Screenshot 1 – x8scan identifies the available GET parameters.

<img width="1918" height="282" alt="image" src="https://github.com/user-attachments/assets/7c33bd81-0bf2-4fa4-a254-aa62db1a53ba" />

Screenshot 2 – Crafted input submitted through the `cat` parameter.

<img width="1918" height="966" alt="image" src="https://github.com/user-attachments/assets/4e870a48-9ef0-4d95-b385-5f411c0a5f95" />

Screenshot 3 – Reflected input visible in the browser response.

<img width="1917" height="968" alt="image" src="https://github.com/user-attachments/assets/dde873d8-d04e-4362-bd17-bcc3d3a4a17d" />

Screenshot 4 – Page source showing the reflected input.

<img width="1918" height="962" alt="image" src="https://github.com/user-attachments/assets/931dadeb-2f23-4bff-a8ae-5c84700e5dc7" />

## Impact

- **Potential Script Injection:** If the reflected input reaches an executable HTML context, an attacker may be able to execute JavaScript.
- **Page Manipulation:** Unsafe handling of reflected input may allow injected markup to alter page content.
- **Targeted Attacks:** If script execution is confirmed, attackers may craft malicious URLs targeting visitors.

## Remediation

1. **Context-Aware Output Encoding:** Encode all user-controlled values before inserting them into HTML responses.
2. **Input Validation:** Validate the `cat` parameter and reject unexpected HTML or script markup where it is not required.
3. **Content Security Policy:** Apply a restrictive CSP and avoid relying solely on blacklist-based filtering.
