## Title
Reflected XSS - Equifax


## Description
**Affected URL:** https://tracking.pwnedbox.site/

The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the `search` GET parameter. The supplied search value is reflected into the page and also passed into the JavaScript analytics tracking function without proper JavaScript string escaping.

During testing, a crafted payload successfully broke out of the JavaScript string context and triggered a browser confirmation dialog. This demonstrates that user-controlled search input can execute JavaScript when a specially crafted URL is opened.


## Steps to Reproduce

1. Open the following URL in a browser:

   `https://tracking.pwnedbox.site/?search='"<!-->)</Title/</h1/</h2/</h3/</h4/</div/</p/</a/</font/</label/</button/</img/</ul/</ol/</li/</option/</span/</Style/</Script/</textArea/</iFrame/</noScript><Img Src=OnXSS OnError=(confirm)(1)>
`

2. Open the modified URL in the browser.
3. Observe the JavaScript confirmation dialog displaying `1`.
4. Inspect the page source and locate the analytics tracking function.
5. Verify that the supplied search value is reflected inside the JavaScript `searchTerm` value without proper escaping.

## POC (Proof of Concept)

Screenshot 1 – Original Equifax search page with the search parameter.

<img width="1918" height="973" alt="image" src="https://github.com/user-attachments/assets/20669a48-d2a1-4409-8c81-ee60d4f695be" />

Screenshot 2 – Crafted payload triggering the JavaScript confirmation dialog.

<img width="1918" height="953" alt="image" src="https://github.com/user-attachments/assets/812dbcd8-f457-49e0-aef4-1fcbe547ea44" />

Screenshot 3 – Page source showing the reflected payload inside the analytics tracking function.

<img width="1918" height="975" alt="image" src="https://github.com/user-attachments/assets/3e84053b-4da0-4f79-95fd-51f2407efdf6" />


## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
