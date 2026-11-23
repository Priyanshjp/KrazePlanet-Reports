## Title
Stored XSS - Acronis

## Description
**Affected URL:** https://forum.pwnedbox.site/

A Stored Cross-Site Scripting (XSS) vulnerability was identified in the Profile Signature field of the Acronis Forum. User-supplied signature content is stored in the database and rendered as HTML on forum posts without proper sanitization. An attacker can inject malicious HTML containing JavaScript event handlers into the signature field. When other users view a post containing the attacker's signature, the injected JavaScript executes in their browser within the context of the forum.

## Steps to Reproduce

1. Open https://forum.pwnedbox.site/ and log in to your account.
2. Navigate to **My Profile** and select **Edit Profile**.
3. Locate the **Signature** field under the Profile Signature section.
4. Enter the following payload:`Priyansh"><><Img SrC=x OnErRoR=confirm(3)>`
5. Click Save Profile to store the signature.
6. Open a forum post where your signature is displayed.
7. Observe that the JavaScript confirmation dialog executes.
8. Inspect the page source or DOM and verify that the stored signature is rendered as HTML.

## POC (Proof of Concept)

Screenshot 1: Payloads entered into Display Name, About Me, and Profile Signature fields.

<img width="1891" height="967" alt="image" src="https://github.com/user-attachments/assets/048354ad-0822-43f2-81c4-23deb7138c46" />

Screenshot 2: Profile saved successfully and the confirmation dialog executes.

<img width="1918" height="962" alt="image" src="https://github.com/user-attachments/assets/b1940a35-cb20-4f8b-9a94-20294ba6ca3b" />

Screenshot 3: DOM inspection showing the Profile Signature field and the stored signature rendered on the page.

<img width="1918" height="960" alt="image" src="https://github.com/user-attachments/assets/fce872bb-eef6-44ea-81be-0d60a142805c" />


## Impact
- It allows attackers to execute arbitrary JavaScript in the victim's browser within the context of the vulnerable application.
- It may allow attackers to hijack user sessions, steal sensitive information accessible to the application, or potentially lead to account takeover.
- It allows attackers to perform unauthorized actions on behalf of affected users, including modifying application data or abusing available account privileges.

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
