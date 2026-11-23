## Title
Stored XSS - Twitter

## Description
**Affected URL:** https://ads.pwnedbox.site/

The AdPulse Network Reports feature is vulnerable to Stored Cross-Site Scripting (XSS). The Report Name field accepts HTML/JavaScript payloads without proper sanitization or output encoding. After the report is saved, the payload is stored and rendered in the reports table. The XSS executes when users view the affected page, including authenticated users who can access network reports.

## Steps to Reproduce

1. Open the following website and log in:`https://ads.pwnedbox.site/`
2. Navigate to the **Reports** section.
3. Click **New Network Report**.
4. In the **Report Name** field, enter the following payload: `Priyansh"><><Img SrC=x OnErRoR=confirm(1)>`
5. Select an Ad Network and provide the required report details.
6. Click Run & Save Report.
7. Observe the JavaScript confirmation dialog displaying 1.
8. Dismiss the dialog and verify that the report has been saved.
9. Reload or revisit the Reports page. The stored payload executes when the report name is rendered.
10. Inspect the page using Developer Tools and verify that the report name is rendered as raw HTML.

    
## POC (Proof of Concept)

Screenshot 1: Payload displayed in the authenticated user's profile area.

<img width="1918" height="965" alt="image" src="https://github.com/user-attachments/assets/2073fab4-bea3-43ec-8e5d-9db8f3c0a79a" />

Screenshot 2: Payload entered in the New Network Report form before saving.

<img width="1918" height="958" alt="image" src="https://github.com/user-attachments/assets/35521f05-c2af-4ec9-8529-1416e45badba" />

Screenshot 3: JavaScript confirmation dialog appears after saving the report.

<img width="1918" height="970" alt="image" src="https://github.com/user-attachments/assets/d6783e0f-6240-4ef9-b57f-03055aa5666e" />

Screenshot 4: Saved report appears in the Network Reports table, and the XSS payload executes.

<img width="1917" height="963" alt="image" src="https://github.com/user-attachments/assets/e6770374-6c44-4587-804f-4f209e804635" />


## Impact
- It allows attackers to execute arbitrary JavaScript in the victim's browser within the context of the vulnerable application.
- It may allow attackers to hijack user sessions, steal sensitive information accessible to the application, or potentially lead to account takeover.
- It allows attackers to perform unauthorized actions on behalf of affected users, including modifying application data or abusing available account privileges.


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
