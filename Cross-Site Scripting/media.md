## Title
Reflected XSS - Imgur Mobile

## Description
**Affected URL:** https://media.pwnedbox.site/

The application accepts a username through the GET parameter and reflects the supplied value in the page content. During testing, a JavaScript payload was submitted through the `username` parameter and successfully executed in the browser.

Source inspection also revealed that the username is inserted into the profile link's `href` attribute without proper HTML attribute encoding. This allows an attacker to inject HTML elements and execute JavaScript in the victim's browser.

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   https://media.pwnedbox.site/?username=Priyanshxop%27%22%3C%3E%3Cscript%3Ealert(1)%3C/script%3E

2. Allow the page to load with the crafted value supplied through the `username` parameter.
3. Observe the JavaScript alert displaying `1`.
4. Dismiss the alert.
5. Inspect the page source and locate the profile link.
6. Verify that the injected content is reflected inside the link's `href` attribute without proper HTML attribute encoding.
## POC (Proof of Concept)

Screenshot 1 – x8scan output showing the transformed URL and `username` parameter.

<img width="1916" height="265" alt="image" src="https://github.com/user-attachments/assets/f4825ef2-d556-4cec-83c3-4424e597f0e6" />

Screenshot 2 – Application page displaying the reflected username.

<img width="1916" height="967" alt="image" src="https://github.com/user-attachments/assets/49dc4fcf-0200-4477-b089-ba7ee8d731c0" />

Screenshot 3 – JavaScript alert displaying `1`, confirming successful execution.

<img width="1911" height="961" alt="image" src="https://github.com/user-attachments/assets/4a31458e-2660-4221-a98c-94e731940890" />

Screenshot 4 – Page source showing the injected payload reflected inside the profile link's `href` attribute.

<img width="1918" height="952" alt="image" src="https://github.com/user-attachments/assets/78c1e019-cf78-4513-9586-18ac5ebfdcb2" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
