## Title
Reflected XSS - Case-Insensitive Filter Bypass

## Description
**Affected URL:** https://directory.pwnedbox.site/

The application accepts First Name (`fname`) and Last Name (`lname`) through GET parameters. During testing, a mixed-case script tag payload was submitted through both parameters.

The application reflected the supplied values inside the HTML input elements without proper output encoding. The mixed-case `<ScRipT>` tag was interpreted by the browser, resulting in successful JavaScript execution. This demonstrates a Reflected Cross-Site Scripting (XSS) vulnerability affecting both parameters.

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   https://directory.pwnedbox.site/?fname=Priyanshxop&lname=Priyanshxop%27%22%3C%3E%3CScRipT%3Ealert%281%29%3C%2FScRipT%3E

2. Allow the page to load with the crafted `fname` and `lname` parameters.
3. Observe the JavaScript alert displaying `1`.
4. Click **OK** to dismiss the alert.
5. Inspect the page source.
6. Locate the reflected `fname` and `lname` values.
7. Verify that the payload is reflected inside both input elements without proper HTML encoding.

   
## POC (Proof of Concept)

Screenshot 1 – Mixed-case XSS payload entered into both First Name and Last Name fields.

<img width="1918" height="963" alt="image" src="https://github.com/user-attachments/assets/56e6ab31-c320-4869-8ba4-f21173d1cb78" />

Screenshot 2 – JavaScript alert displaying `1` executes successfully.

<img width="1918" height="970" alt="image" src="https://github.com/user-attachments/assets/82da2268-55af-4948-83e8-ba44ace5806e" />

Screenshot 3 – Page source confirms unescaped payload reflection in both `fname` and `lname` input elements.

<img width="1918" height="957" alt="image" src="https://github.com/user-attachments/assets/3139a0be-1880-467f-aac5-e4c7c7ce0f4d" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
