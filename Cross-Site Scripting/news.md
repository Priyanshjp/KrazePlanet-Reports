## Title
Reflected XSS - HTML Tag Filter Bypass

## Description
**Affected URL:** https://news.pwnedbox.site/

The application accepts user-controlled input through the `title` GET parameter and reflects it inside the HTML document's title element. During testing, a specially crafted HTML payload bypassed the filtering mechanism and successfully executed JavaScript in the browser.

The vulnerability occurs because user input is not safely encoded before being inserted into the HTML response. This allows an attacker to manipulate the document structure and execute arbitrary JavaScript in a visitor's browser.

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   https://news.pwnedbox.site/?title=%27%22%3C!--%3E)%3C/Title/%3C/h1/%3C/h2/%3C/h3/%3C/h4/%3C/div/%3C/p/%3C/a/%3C/font/%3C/label/%3C/button/%3C/img/%3C/ul/%3C/ol/%3C/li/%3C/option/%3C/span/%3C/Style/%3C/Script/%3C/textArea/%3C/iFrame/%3C/noScript%3E%3CImg%20Src=OnXSS%20OnError=(confirm)(1)%3E)

2. Allow the page to load with the crafted payload supplied through the `title` parameter.
3. Observe the JavaScript confirmation dialog displaying `1`.
4. Dismiss the confirmation dialog.
5. Inspect the rendered page and its source code.
6. Locate the HTML `<title>` element.
7. Verify that the supplied input is reflected inside the title context without proper output encoding.
   
## POC (Proof of Concept)

Screenshot 1 – x8scan output showing the discovered GET parameters.

<img width="1918" height="280" alt="image" src="https://github.com/user-attachments/assets/d4e31984-adad-4593-a2db-9de92e38658e" />

Screenshot 2 – Crafted payload submitted through the `title` parameter.

<img width="1918" height="961" alt="image" src="https://github.com/user-attachments/assets/0e8b696e-e2ee-41b9-bea7-6579b60bcca0" />

Screenshot 3 – JavaScript confirmation dialog displaying `1`.

<img width="1917" height="967" alt="image" src="https://github.com/user-attachments/assets/333d98e9-fffa-4ae2-843f-f9ebaaec0f0e" />

Screenshot 4 – Rendered page showing the injected HTML content.

<img width="1918" height="968" alt="image" src="https://github.com/user-attachments/assets/95fc3b74-e234-46eb-a2a4-d121c66ef2f5" />

Screenshot 5 – Page source showing the reflected payload inside the HTML title element.

<img width="1918" height="971" alt="image" src="https://github.com/user-attachments/assets/94b5e6a9-d338-4530-9381-440ca4bd64c0" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
