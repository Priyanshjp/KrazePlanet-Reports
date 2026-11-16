## Title
Reflected XSS - PUBG
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://assets.pwnedbox.site/

The application uses the `p` GET parameter to filter and display PUBG community content. During testing, the supplied parameter value was reflected into the `data-query` HTML attribute without proper output encoding.

The lack of HTML attribute encoding allowed an attacker to inject HTML and JavaScript through a specially crafted URL. The injected payload successfully executed a JavaScript confirmation dialog in the browser, confirming a Reflected Cross-Site Scripting (XSS) vulnerability.

## Steps to Reproduce

1. Open https://assets.pwnedbox.site/.
2. Locate the Filter input field on the Community page.
3. Enter the crafted XSS payload into the filter field.
4. Submit the search request.
5. Observe the JavaScript confirmation dialog.
6. Inspect the page source and locate the `data-query` attribute.
7. Verify that the injected payload is reflected without proper HTML attribute encoding.

## POC (Proof of Concept)

Screenshot 1 – XSS payload entered into the PUBG Community Filter field and reflected in the results.

<img width="1918" height="957" alt="image" src="https://github.com/user-attachments/assets/daf9d8fa-64c1-45b9-87b1-8581d627ab32" />

Screenshot 2 – JavaScript confirmation dialog displaying `XSS` executes successfully.

<img width="1918" height="961" alt="image" src="https://github.com/user-attachments/assets/ddc1bbae-8723-458c-950f-1fedd6d3fb8c" />

Screenshot 3 – Page source shows the injected payload inside the `data-query` attribute without proper output encoding.

<img width="1918" height="970" alt="image" src="https://github.com/user-attachments/assets/d0d6f365-edc7-4913-aae2-a2de7b5efc46" />

## Impact

- **Arbitrary JavaScript Execution:** An attacker may execute JavaScript in a victim's browser through a crafted URL.
- **Phishing and Content Manipulation:** An attacker may modify displayed page content or create deceptive elements.
- **Victim-Targeted Attacks:** Crafted links may be distributed to users to trigger the reflected XSS when opened.
   
## Remediation

1. **Context-Aware Output Encoding:** Apply proper HTML attribute encoding to the `p` parameter before inserting it into the `data-query` attribute.
2. **Secure Output Handling:** Use `htmlspecialchars()` with appropriate encoding flags, such as `ENT_QUOTES | ENT_SUBSTITUTE`, and UTF-8 encoding.
3. **Content Security Policy:** Implement a restrictive Content Security Policy and avoid relying on blacklist-based filtering as the primary XSS defense.
