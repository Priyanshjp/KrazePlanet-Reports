## Title
Stored XSS - Support Center (DigitalOcean Clone)

## Description
**Affected URL:** https://digitalocean.pwnedbox.site/

The Support Center ticket creation form is vulnerable to Stored Cross-Site Scripting (XSS). User-supplied content in the ticket Subject and Description fields is stored and later displayed in the Support Tickets listing without proper output encoding. The supplied HTML payload is interpreted by the browser, allowing JavaScript execution when the stored ticket content is rendered. The issue was demonstrated by submitting a ticket and triggering a JavaScript confirmation dialog from the stored description.

## Steps to Reproduce

1. Open the affected website:`https://digitalocean.pwnedbox.site/`
2. Navigate to **Support Center** and click **Create Ticket**.
3. In the **Subject** field, enter:`Priyansh"'><><Img SrC=x OnErRoR=confirm(1)>`
4. In the Description field, enter:`Priyansh"'><><Img SrC=x OnErRoR=confirm(2)>`
5. Select the required priority and submit the support ticket.
6. After the ticket is created, observe the JavaScript confirmation dialog displaying 2.
7. Dismiss the dialog and observe the stored ticket in the Support Tickets listing.
8. Inspect the page source and verify that the submitted subject and description are reflected in the stored ticket content.

## POC (Proof of Concept)

Screenshot 1: Malicious HTML payloads entered into the Subject and Description fields.

<img width="1918" height="962" alt="image" src="https://github.com/user-attachments/assets/75de54cc-32cc-41df-ab8f-006cc9b129db" />

Screenshot 2: JavaScript confirmation dialog displaying 2 after ticket submission; the stored ticket is visible in the listing.

<img width="1917" height="952" alt="image" src="https://github.com/user-attachments/assets/6eef94af-736f-4261-8398-1c6f7dbdd5ed" />

Screenshot 3: Page source showing the submitted ticket subject and description reflected in the ticket listing.

<img width="1918" height="967" alt="image" src="https://github.com/user-attachments/assets/faab4d5c-ed80-490f-b3e1-3f90bf594a2c" />


## Impact
- It allows attackers to execute arbitrary JavaScript in the victim's browser within the context of the vulnerable application.
- It may allow attackers to hijack user sessions, steal sensitive information accessible to the application, or potentially lead to account takeover.
- It allows attackers to perform unauthorized actions on behalf of affected users, including modifying application data or abusing available account privileges.


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
