## Title
Stored XSS - Shopify

## Description
**Affected URL:** https://cms.pwnedbox.site/

The Quill CMS allows users to create and publish articles. The article editor accepts HTML content and indicates that content is saved as-is and rendered using `innerHTML` on the articles page. A JavaScript payload supplied through article content is stored and executes when the articles page loads. This results in Stored Cross-Site Scripting (XSS), which may affect users who view the published article.


## Steps to Reproduce

1. Log in to the CMS at `https://cms.pwnedbox.site/`.
2. Navigate to the **Write** section and create a new article.
3. Enter the following payload in the article title or HTML content field:

   ```html
   Priyansh"><><Img SrC=x OnErRoR=confirm(1)>

4. Publish the article.
5. Navigate to the My Articles page.
6. Observe that the JavaScript confirmation dialog appears with the value 1.
7. Dismiss the dialog and inspect the published article using browser developer tools.
8. Verify that the supplied payload is stored and rendered as part of the article.

## POC (Proof of Concept)

Screenshot 1: The article editor exposes an HTML source editing option and states that content is rendered using innerHTML.

<img width="1916" height="977" alt="image" src="https://github.com/user-attachments/assets/36dd35f3-7841-470a-9598-4253a5a911ad" />

Screenshot 2: The XSS payload is entered into the article title, description, and tags fields.

<img width="1917" height="966" alt="image" src="https://github.com/user-attachments/assets/1485c4ea-6732-4fa0-a2ac-9aaf5b837109" />

Screenshot 3: After publishing the article, a JavaScript confirmation dialog displaying 1 appears.

<img width="1918" height="963" alt="image" src="https://github.com/user-attachments/assets/1d385237-1061-44ce-808e-357fd9f7ae72" />

Screenshot 4: Developer tools show the published article and the stored payload in the article's DOM.

<img width="1918" height="962" alt="image" src="https://github.com/user-attachments/assets/235a1c1f-dec4-4915-8972-75d3a52079a4" />


## Impact
- It allows attackers to execute arbitrary JavaScript in the victim's browser within the context of the vulnerable application.
- It may allow attackers to hijack user sessions, steal sensitive information accessible to the application, or potentially lead to account takeover.
- It allows attackers to perform unauthorized actions on behalf of affected users, including modifying application data or abusing available account privileges.

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
