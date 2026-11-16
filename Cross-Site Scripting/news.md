## Title
Reflected XSS - HTML Tag Filter Bypass

## Description
**Affected URL:** https://news.pwnedbox.site/

The application accepts user-controlled input through the `title` GET parameter and reflects it inside the HTML document's title element. During testing, a specially crafted HTML payload bypassed the filtering mechanism and successfully executed JavaScript in the browser.

The vulnerability occurs because user input is not safely encoded before being inserted into the HTML response. This allows an attacker to manipulate the document structure and execute arbitrary JavaScript in a visitor's browser.

## Steps to Reproduce

1. Open https://news.pwnedbox.site/.
2. Identify the `title` GET parameter.
3. Submit a crafted HTML payload `<!--></Title/</Textarea/</Script/</Iframe/><Details/Open/OnToggle=(confirm)(1)-->%5C` through the parameter.
4. Load the modified URL in the browser.
5. Observe the JavaScript confirmation dialog displaying `1`.
6. Inspect the rendered page and its source code.
7. Verify that the supplied input is reflected inside the HTML title element.
   
## POC (Proof of Concept)

Screenshot 1 – x8scan output showing the discovered GET parameters.

<img width="1918" height="280" alt="image" src="https://github.com/user-attachments/assets/d4e31984-adad-4593-a2db-9de92e38658e" />

Screenshot 2 – Crafted payload submitted through the `title` parameter.

<img width="1918" height="961" alt="image" src="https://github.com/user-attachments/assets/0e8b696e-e2ee-41b9-bea7-6579b60bcca0" />

Screenshot 3 – JavaScript confirmation dialog displaying `1`.

<img width="1917" height="967" alt="image" src="https://github.com/user-attachments/assets/333d98e9-fffa-4ae2-843f-f9ebaaec0f0e" />

Screenshot 4 – Rendered page showing the injected HTML content.

<img width="1918" height="968" alt="image" src="https://github.com/user-attachments/assets/95fc3b74-e234-46eb-a2a4-d121c66ef2f5" />

Screenshot 5 – Page source showing the reflected payload inside the HTML title element.

<img width="1918" height="971" alt="image" src="https://github.com/user-attachments/assets/94b5e6a9-d338-4530-9381-440ca4bd64c0" />


## Impact

- **Arbitrary JavaScript Execution:** An attacker may execute JavaScript in a victim's browser through a specially crafted URL.
- **Phishing and UI Manipulation:** Malicious scripts may modify page content or display deceptive interfaces.
- **Targeted Attacks:** Attackers may distribute crafted links to users and exploit the vulnerability when they visit the affected page.
  
## Remediation

1. **Context-Aware Output Encoding:** Encode user-controlled input before inserting it into HTML elements, attributes, or other document contexts.
2. **Input Validation:** Validate the `title` parameter and reject unexpected markup where HTML input is not required.
3. **Content Security Policy:** Implement a restrictive CSP and avoid relying on blacklist-based HTML filtering as the primary XSS defense.
