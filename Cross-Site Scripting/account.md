## Title
Reflected XSS - Extended Function Filter

## Description
**Affected URL:** https://account.pwnedbox.site/

The application accepts First Name (`fname`) and Last Name (`lname`) values through GET parameters. During testing, an obfuscated JavaScript event-handler payload was submitted through both parameters.

Source inspection revealed different output-handling behavior. The `fname` value was HTML-encoded, whereas the `lname` value was reflected as raw HTML inside its input element. The payload uses JavaScript string concatenation to construct the `alert` function name, demonstrating a potential bypass of basic function-name filtering.

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   `https://account.pwnedbox.site/?fname=%22%3E%3Cbody%20onpageshow%3D%2F*x*%2Fthis%5B%27a%27%2B%27l%27%2B%27e%27%2B%27r%27%2B%27t%27%5D(1)%2F%2F%3E&lname=%22%3E%3Cbody%20onpageshow%3D%2F*x*%2Fthis%5B%27a%27%2B%27l%27%2B%27e%27%2B%27r%27%2B%27t%27%5D(1)%2F%2F%3E`

2. Allow the page to load with the crafted `fname` and `lname` values.
3. Inspect the page source.
4. Locate the reflected `fname` and `lname` parameters.
5. Observe that `fname` is HTML-encoded, while `lname` is reflected without proper HTML encoding.

## POC (Proof of Concept)

Screenshot 1 – Extended JavaScript function payload entered into both First Name and Last Name fields.

<img width="1918" height="966" alt="image" src="https://github.com/user-attachments/assets/6ffeda1b-ea84-4f6c-ada5-9a9150963355" />

Screenshot 2 – JavaScript alert displaying `1` executes successfully.

<img width="1917" height="957" alt="image" src="https://github.com/user-attachments/assets/31530b9d-bc82-4992-b71c-891058bfb456" />

Screenshot 3 – Page source shows HTML-encoded `fname` and unescaped payload reflection in the `lname` input element.

<img width="1918" height="955" alt="image" src="https://github.com/user-attachments/assets/cefa5110-5252-45c8-96c1-d587d6d7c15d" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
