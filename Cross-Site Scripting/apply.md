## Title
Self XSS - POST-Based Reflected XSS

## Description
**Affected URL:** https://apply.pwnedbox.site/


The application is vulnerable to POST-Based Reflected Cross-Site Scripting (XSS) through the Name input field. User-supplied input is reflected directly into the HTML response without proper output encoding.

During testing, a crafted JavaScript payload was submitted through the form. The browser successfully executed the injected JavaScript and displayed an alert containing `1`. The page source also confirmed that the submitted value was reflected in the response.


## Steps to Reproduce

1. Open https://apply.pwnedbox.site/ in a browser.
2. Locate the **Name** input field.
3. Enter the following payload:

   `Priyanshxop%27"<><ScRipT>a\u006cert(1)</ScRipT>`

4. Click the **Enter** button to submit the form.
5. Observe the JavaScript alert displaying `1`.
6. Click **OK** to dismiss the alert.
7. Open the page source and locate the reflected greeting.
8. Verify that the submitted payload appears inside the HTML response without proper output encoding


## POC (Proof of Concept)

Screenshot 1 – Crafted XSS payload entered into the Name field before submission.

<img width="1916" height="972" alt="image" src="https://github.com/user-attachments/assets/6231e1d9-b185-4025-bfdb-a8c0afa56e18" />

Screenshot 2 – JavaScript alert successfully executing and displaying 1.

<img width="1918" height="962" alt="image" src="https://github.com/user-attachments/assets/29554b5c-11e6-479b-bd31-21e3240feca4" />

Screenshot 3 – Page source showing the user-controlled input reflected inside the greeting paragraph.

<img width="1916" height="966" alt="image" src="https://github.com/user-attachments/assets/53a7a467-6cad-40d9-8e86-f0ad225a564a" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
