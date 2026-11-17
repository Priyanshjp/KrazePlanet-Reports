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
3. Enter the crafted XSS payload `Priyanshxop'"<><ScRipT>confirm("XSS");</ScRipT>` into the filter field.
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
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
