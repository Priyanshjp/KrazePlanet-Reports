## Title
Stored XSS - Product Reviews (Flipkart Clone)

## Description
**Affected URL:** https://members.pwnedbox.site/

A Stored Cross-Site Scripting (XSS) vulnerability was identified in the product review functionality. User-supplied review comments are stored and later rendered without proper output encoding. An attacker can submit a crafted review containing HTML event-handler JavaScript, which executes when the review is displayed to users.

## Steps to Reproduce

1. Log in to https://members.pwnedbox.site/
2. Open the product page:
   https://members.pwnedbox.site/index.php?product_id=1
3. Navigate to the **Write a Customer Review** section.
4. Select a star rating.
5. Enter the following payload in the **Headline / Short Title** field: `Priyansh"><><Img SrC=x OnErRoR=confirm('Title')>`
6. Enter the following payload in the Detailed Review Comments field:`Priyansh"><><Img SrC=x OnErRoR=confirm('Des')>`
7. Click Submit Product Review.
8. Observe the JavaScript confirmation dialog.
9. Inspect the page source and verify that the submitted review content is stored and rendered in the customer reviews section.

## POC (Proof of Concept)

Screenshot 1: XSS payloads entered in the review title and detailed comments fields.

<img width="1918" height="972" alt="image" src="https://github.com/user-attachments/assets/22b6aef1-d420-4d47-9114-83744344ef94" />

Screenshot 2: JavaScript confirmation dialog displaying Des after the review is submitted.

<img width="1918" height="963" alt="image" src="https://github.com/user-attachments/assets/8de80980-f3ad-4556-b967-74e6bfb53477" />

Screenshot 3: Page source showing the stored review payload in the customer review title and body.

<img width="1912" height="956" alt="image" src="https://github.com/user-attachments/assets/75a9f669-ad93-466e-8039-7814f59572ae" />


## Impact
- It allows attackers to execute arbitrary JavaScript in the victim's browser within the context of the vulnerable application.
- It may allow attackers to hijack user sessions, steal sensitive information accessible to the application, or potentially lead to account takeover.
- It allows attackers to perform unauthorized actions on behalf of affected users, including modifying application data or abusing available account privileges.


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
