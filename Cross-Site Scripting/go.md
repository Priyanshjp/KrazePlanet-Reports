## Title
Reflected XSS - Shopify

## Description
**Affected URL:** https://go.pwnedbox.site/

The application uses the `returnTo` GET parameter to determine the destination of the Cancel & return link on the account confirmation page. During parameter discovery using `x8scan`, an additional GET parameter named `returnTo` was identified.

Testing revealed that the parameter value was reflected directly into the `href` attribute of the Cancel & return link without proper HTML attribute encoding. By supplying a crafted payload, it was possible to inject HTML and JavaScript into the page, resulting in successful JavaScript execution in the browser.

## Steps to Reproduce

1. Run `x8scan` against https://go.pwnedbox.site/ to discover GET parameters.
2. Identify the `returnTo` parameter in the transformed URL.
3. Supply the crafted XSS payload `Priyanshxop'"<><ScRipT>confirm("XSS");</ScRipT>` through the `returnTo` parameter.
4. Open the crafted URL in a browser.
5. Observe the Shopify-style account confirmation page.
6. Observe the JavaScript confirmation dialog displaying `XSS`.
7. Inspect the page source and locate the Cancel & return link.
8. Verify that the injected payload is reflected inside the link's `href` attribute.

## POC (Proof of Concept)

Screenshot 1 – `x8scan` identifies the hidden `returnTo` GET parameter.

<img width="1918" height="280" alt="image" src="https://github.com/user-attachments/assets/56fef6bf-1e78-4c80-80c2-1f03293e6563" />

Screenshot 2 – Crafted payload supplied through the `returnTo` parameter, loading the account confirmation page.

<img width="1918" height="970" alt="image" src="https://github.com/user-attachments/assets/5fed1a55-6912-40e3-a4f6-90a5d56fff12" />

Screenshot 3 – JavaScript confirmation dialog displaying `XSS` executes successfully.

<img width="1918" height="961" alt="image" src="https://github.com/user-attachments/assets/0fda5f6b-ce5b-43e7-9b59-590ee7f6254c" />

Screenshot 4 – Page source confirms the injected payload is reflected inside the Cancel & return link's `href` attribute.

<img width="1918" height="958" alt="image" src="https://github.com/user-attachments/assets/0478f2d1-05a2-4164-acc3-e6016d47aee3" />


## Impact

- **Arbitrary JavaScript Execution:** An attacker may execute JavaScript in a victim's browser through a specially crafted URL.
- **Phishing and UI Manipulation:** An attacker may create deceptive page elements or manipulate displayed content.
- **Victim-Targeted Attacks:** Malicious links may be distributed to users to trigger the reflected XSS when opened.

## Remediation

1. **Context-Aware Output Encoding:** Properly HTML-attribute-encode the `returnTo` parameter before inserting it into the `href` attribute.
2. **URL Validation:** Validate redirect destinations against an allowlist of trusted URLs and reject unexpected or malformed values.
3. **Content Security Policy:** Implement a restrictive Content Security Policy and avoid relying on blacklist-based filtering as the primary XSS defense.
