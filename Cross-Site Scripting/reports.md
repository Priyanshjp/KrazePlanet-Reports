## Title
Self XSS - in Document Title

## Description
**Affected URL:** https://reports.pwnedbox.site/

The Reports page contains a Reflected Cross-Site Scripting (XSS) vulnerability in the HTML document title.

The application reflects user-controlled input directly into the HTML `<title>` element without proper output encoding. A specially crafted HTML payload can break out of the title context and introduce an event handler that executes JavaScript in the browser.

Successful execution is demonstrated by a JavaScript confirmation dialog displaying `1`.

## Steps to Reproduce

1. Open `https://reports.pwnedbox.site/` in a browser.
2. Locate the **Name** input field.
3. Enter the following crafted XSS payload: `'"<!--->)</Title></h1></h2></h3></h4></div></p></a></font></label></button></img></ul></ol></li></option></span></Style></Script></textArea></iFrame></noScript><Img Src=OnXSS OnError=(confirm)(1)>`
4. Click the Enter button to submit the form.
5. Observe the JavaScript confirmation dialog displaying 1.
6. Dismiss the confirmation dialog.
7. Inspect the page source.
8. Locate the HTML <title> element.
9. Verify that the supplied payload is reflected inside the document title without proper context-aware output encoding.

## POC (Proof of Concept)

Screenshot 1: Crafted XSS payload entered into the Name field.

<img width="1918" height="950" alt="image" src="https://github.com/user-attachments/assets/387bd775-bd2e-413a-b3ff-5cd3584975e2" />

Screenshot 2: JavaScript confirmation dialog displaying 1.

<img width="1915" height="950" alt="image" src="https://github.com/user-attachments/assets/eee79a4f-34a4-4845-b055-9a7702bad33b" />

Screenshot 3: Page source showing the injected payload inside the HTML <title> element.

<img width="1918" height="960" alt="image" src="https://github.com/user-attachments/assets/952aa7e2-435b-4886-8c44-fa6c734f1674" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
