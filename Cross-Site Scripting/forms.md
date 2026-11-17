## Title
Self XSS - via POST Parameter

## Description
**Affected URL:** https://forms.pwnedbox.site/

The KrazePlanet Labs search functionality accepts user-controlled input through the `search` POST parameter.

The application reflects the submitted search value inside the HTML response, specifically in the search results heading and the empty-state message, without proper output encoding.

By submitting a specially crafted HTML and JavaScript payload, JavaScript execution can be triggered in the browser. This is demonstrated by an alert displaying `1`.

## Steps to Reproduce

1. Open https://forms.pwnedbox.site/ in a browser.
2. Locate the search input field on the page.
3. Enter the following crafted payload into the search field: `Priyanshxop'"><ScRipT>\u006Cert(1);</ScRipT>`
4. Observe the JavaScript alert displaying 1.
5. Dismiss the alert.
6. Inspect the page source.
7. Locate the search results heading and empty-state message.
8. Verify that the submitted payload is reflected in both locations without proper HTML output encoding.

## POC (Proof of Concept)

Screenshot 1: Crafted XSS payload entered into the search field.

<img width="1918" height="966" alt="image" src="https://github.com/user-attachments/assets/b8cd921c-2d59-466e-abde-b656920815e1" />

Screenshot 2: JavaScript alert displaying 1 after submitting the search form.

<img width="1917" height="965" alt="image" src="https://github.com/user-attachments/assets/6d696462-decc-4eea-b94d-12050ccb8fe7" />

Screenshot 3: Page source showing the reflected payload in the search results heading and empty-state message.

<img width="1911" height="963" alt="image" src="https://github.com/user-attachments/assets/a7099f45-563e-4d30-957d-f5c346ee4f73" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
