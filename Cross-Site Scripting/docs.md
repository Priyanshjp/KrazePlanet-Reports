## Title
Reflected XSS - Page Heading

## Description
**Affected URL:** https://docs.pwnedbox.site/

The application contains a GET-based search/reflection functionality. The visible form displays First Name (`fname`) and Last Name (`lname`) fields, but parameter discovery using `x8scan` revealed an additional GET parameter named `q`.

The `q` parameter was not directly visible in the form. After identifying it, a crafted XSS payload was supplied through the parameter. The application reflected the input inside the page heading without proper output encoding, resulting in JavaScript execution in the browser.

## Steps to Reproduce

1. Open `https://docs.pwnedbox.site/`.
2. Observe the visible First Name and Last Name input fields.
3. Run parameter discovery using `x8scan` against the target URL.
4. Identify the additional GET parameter `q` in the transformed URL.
5. Inject the XSS payload `Priyanshxop'"<><ScRipT>alert("XSS");</ScRipT>` into the `q` parameter.
6. Open the crafted URL in a browser.
7. Observe the JavaScript alert execution.
8. Inspect the page source and verify that the supplied payload is reflected inside the page heading.

## POC (Proof of Concept)

Screenshot 1 – `x8scan` identifies the hidden `q` GET parameter alongside `fname` and `lname`.

<img width="1892" height="318" alt="image" src="https://github.com/user-attachments/assets/f74317c7-6604-45cd-93df-b70812373f45" />

Screenshot 2 – The crafted `q` parameter is supplied in the browser URL.

<img width="1918" height="953" alt="image" src="https://github.com/user-attachments/assets/3c3d292e-03fa-42a0-981f-3e05866b1bde" />

Screenshot 3 – JavaScript alert displaying `XSS` executes successfully.

<img width="1918" height="961" alt="image" src="https://github.com/user-attachments/assets/e2ffaa52-4f15-4035-aa24-a335d82911a9" />

Screenshot 4 – Page source confirms that the injected payload is reflected inside the HTML heading.

<img width="1918" height="962" alt="image" src="https://github.com/user-attachments/assets/d0d79d51-9128-44b5-841a-c027721e1e9e" />

## Impact

- **Arbitrary JavaScript Execution:** An attacker may execute JavaScript in a victim's browser through a crafted URL.
- **Phishing and Content Manipulation:** An attacker may modify displayed page content or create deceptive elements.
- **Victim-Targeted Attacks:** Specially crafted links may be distributed to users to trigger the reflected XSS payload when opened.

## Remediation

1. **Context-Aware Output Encoding:** Properly HTML-encode all user-controlled values before inserting them into page headings or other HTML elements.
2. **Input Validation:** Validate and safely handle all GET parameters, including hidden or undocumented parameters such as `q`.
3. **Content Security Policy:** Implement a restrictive CSP and avoid relying on blacklist-based filtering as the primary XSS defense.
