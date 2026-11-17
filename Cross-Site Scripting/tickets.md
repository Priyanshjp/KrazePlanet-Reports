## Title
Reflected XSS - Event Handler Filter

## Description
**Affected URL:** https://tickets.pwnedbox.site/

The application accepts First Name (`fname`) and Last Name (`lname`) values through GET parameters. During testing, an event-handler payload using JavaScript string concatenation was submitted to bypass basic function-name filtering.

Source inspection showed that the `fname` value was HTML-encoded, whereas the `lname` value was reflected as raw HTML inside its input element. This allowed the injected HTML event handler to execute JavaScript in the browser, confirming a Reflected Cross-Site Scripting (XSS) vulnerability.

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   https://tickets.pwnedbox.site/?lname=%22%3E%3Cbody%20onpageshow%3D%2F*x*%2Fthis%5B%27a%27%2B%27l%27%2B%27e%27%2B%27r%27%2B%27t%27%5D(1)%2F%2F%3E

2. Allow the Tickets page to load with the crafted payload supplied through the `lname` parameter.
3. Observe the JavaScript alert displaying `1`.
4. Dismiss the alert.
5. Inspect the page source.
6. Compare how the `fname` and `lname` values are reflected in the HTML response.
7. Verify that the `lname` value is reflected without proper HTML attribute encoding, while the `fname` value is encoded.

## POC (Proof of Concept)

Screenshot 1 – Rendered page showing the submitted payload and its reflection.

<img width="1918" height="945" alt="image" src="https://github.com/user-attachments/assets/cddb6055-7da4-4ef4-82aa-0c59cbcd66c1" />

Screenshot 2 – JavaScript alert displaying `1`, confirming successful execution.

<img width="1917" height="953" alt="image" src="https://github.com/user-attachments/assets/5614d7c9-527b-4307-a4eb-10fec204dc60" />

Screenshot 3 – Page source showing encoded `fname` and unescaped event-handler payload reflection in `lname`.

<img width="1918" height="971" alt="image" src="https://github.com/user-attachments/assets/49259973-a9ab-46f1-80ff-bc58c60367eb" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
