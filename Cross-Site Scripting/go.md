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
4. Open the following crafted URL in a browser:

    https://go.pwnedbox.site/?returnTo=Priyanshxop%27%22%3C%3EScRipT%3Econfirm(%22XSS%22);%3C/ScRipT%3E
   
6. Allow the Shopify-style account confirmation page to load.
7. Observe the JavaScript confirmation dialog displaying XSS.
8. Dismiss the confirmation dialog.
9. Inspect the page source and locate the Cancel & return link.
10. Verify that the injected payload is reflected inside the link's href attribute.


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
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
