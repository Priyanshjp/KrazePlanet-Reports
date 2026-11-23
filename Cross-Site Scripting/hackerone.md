## Title
Stored XSS - Bug Bounty Platform (HackerOne Clone)

## Description
**Affected URL:** https://hackerone.pwnedbox.site/

The vulnerability exists in the vulnerability report submission feature. User-supplied input in the Title, Description, and Impact fields is stored and later rendered without proper HTML encoding. Submitting HTML with an event handler causes JavaScript to execute when the stored report is viewed, resulting in Stored Cross-Site Scripting (XSS).


## Steps to Reproduce

1. Open the following URL: https://hackerone.pwnedbox.site/index.php?view=submit
2. Fill in the Title field with the following payload:`Priyansh"><><ImG SrC=x OnErRoR=confirm(1)>`
3. Fill in the Description field with:`Priyansh"><><ImG SrC=x OnErRoR=confirm(2)>`
4. Fill in the Impact field with:`Priyansh"><><ImG SrC=x OnErRoR=confirm(3)>`
5. Select the target program and severity, then click Submit Report.
6. After the report is submitted, observe the JavaScript confirmation dialog.
7. Dismiss the dialog and open the submitted report from the dashboard.
8. Observe that the stored payload is rendered in the report details and JavaScript executes.
9. Inspect the page source and verify that the injected HTML is reflected in the report title, description, and impact sections without proper encoding.

## POC (Proof of Concept)

Screenshot 1: The report submission form containing the injected payloads in the Title, Description, and Impact fields.

<img width="1918" height="963" alt="image" src="https://github.com/user-attachments/assets/5a0ab7c0-e5d2-48f5-afec-749039d07321" />

Screenshot 2: JavaScript confirmation dialog triggered by the stored payload when the submitted report is viewed.

<img width="1917" height="961" alt="image" src="https://github.com/user-attachments/assets/2bb06c06-9687-4b56-974a-5ee7a8abd9bc" />

Screenshot 3: Page source showing the injected HTML reflected in the report title, description, and impact content.

<img width="1918" height="966" alt="image" src="https://github.com/user-attachments/assets/d6626427-f8d8-42f5-8294-5a4a1a8097ea" />


## Impact
- It allows attackers to execute arbitrary JavaScript in the victim's browser within the context of the vulnerable application.
- It may allow attackers to hijack user sessions, steal sensitive information accessible to the application, or potentially lead to account takeover.
- It allows attackers to perform unauthorized actions on behalf of affected users, including modifying application data or abusing available account privileges.


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
