## Title
Reflected XSS - Function Name Filter

## Description
**Affected URL:** https://profile.pwnedbox.site/

The application accepts First Name (`fname`) and Last Name (`lname`) values through GET parameters. During testing, a crafted HTML event-handler payload was submitted through the form. The page source showed that the `fname` value was HTML-encoded, whereas the `lname` value was reflected without proper encoding.

The payload used JavaScript string concatenation to construct the `alert` function name. This bypassed the filtering behavior and triggered JavaScript execution through the `onpageshow` event.

## Steps to Reproduce


1. Open the following crafted URL in a browser:

   https://profile.pwnedbox.site/?lname=%22%3E%3Cbody%20onpageshow%3D%2F*x*%2Fthis%5B%27a%27%2B%27l%27%2B%27e%27%2B%27r%27%2B%27t%27%5D(1)%2F%2F%3E

2. Allow the profile page to load with the crafted payload supplied through the `lname` parameter.
3. Observe the JavaScript alert displaying `1`.
4. Dismiss the alert.
5. Inspect the page source and compare how the `fname` and `lname` values are reflected.
6. Verify that the `lname` value is reflected without proper HTML attribute encoding, while the `fname` value is encoded.

## POC (Proof of Concept)

Screenshot 1 – Crafted payload entered into the First Name and Last Name fields.

<img width="1918" height="957" alt="image" src="https://github.com/user-attachments/assets/9b9f0be3-c059-41ee-8f4c-bd6a574a9559" />

Screenshot 2 – JavaScript alert displaying `1` executes after form submission.

<img width="1918" height="968" alt="image" src="https://github.com/user-attachments/assets/ffb2bce3-30e4-4f9a-b0d6-38ce3455a582" />

Screenshot 3 – Page source shows HTML-encoded `fname` and unencoded payload reflection in `lname`.

<img width="1917" height="973" alt="image" src="https://github.com/user-attachments/assets/39c51679-8ef9-4cc2-a436-902b89537859" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
