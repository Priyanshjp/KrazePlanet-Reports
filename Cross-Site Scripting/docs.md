## Title
Reflected XSS - Page Heading

## Description
**Affected URL:** https://docs.pwnedbox.site/

The application contains a GET-based search/reflection functionality. The visible form displays First Name (`fname`) and Last Name (`lname`) fields, but parameter discovery using `x8scan` revealed an additional GET parameter named `q`.

The `q` parameter was not directly visible in the form. After identifying it, a crafted XSS payload was supplied through the parameter. The application reflected the input inside the page heading without proper output encoding, resulting in JavaScript execution in the browser.

## Steps to Reproduce

## Steps to Reproduce

1. Run parameter discovery against `https://docs.pwnedbox.site/` using x8scan.
2. Identify the additional GET parameter `q`.
3. Open the following crafted URL in a browser:

   https://docs.pwnedbox.site/?q=Priyanshxop%27%22%3C%3E%3CScRipT%3Ealert(%22XSS%22);%3C/ScRipT%3E

4. Allow the page to load with the crafted `q` parameter.
5. Observe the JavaScript alert displaying `XSS`.
6. Click **OK** to dismiss the alert.
7. Inspect the page source.
8. Verify that the supplied payload is reflected inside the page heading.

## POC (Proof of Concept)

Screenshot 1 – `x8scan` identifies the hidden `q` GET parameter alongside `fname` and `lname`.

<img width="1892" height="318" alt="image" src="https://github.com/user-attachments/assets/f74317c7-6604-45cd-93df-b70812373f45" />

Screenshot 2 – The crafted `q` parameter is supplied in the browser URL.

<img width="1918" height="953" alt="image" src="https://github.com/user-attachments/assets/3c3d292e-03fa-42a0-981f-3e05866b1bde" />

Screenshot 3 – JavaScript alert displaying `XSS` executes successfully.

<img width="1918" height="961" alt="image" src="https://github.com/user-attachments/assets/e2ffaa52-4f15-4035-aa24-a335d82911a9" />

Screenshot 4 – Page source confirms that the injected payload is reflected inside the HTML heading.

<img width="1918" height="962" alt="image" src="https://github.com/user-attachments/assets/d0d79d51-9128-44b5-841a-c027721e1e9e" />

## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
