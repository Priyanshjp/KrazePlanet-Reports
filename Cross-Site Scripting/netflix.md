## Title
Stored XSS - Video Streaming Service (Netflix Clone)
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://netflix.pwnedbox.site/

The Netflix clone contains Stored Cross-Site Scripting (XSS) vulnerabilities in user-controlled content. The username entered during registration is stored and later rendered without proper HTML encoding in the navigation bar. The movie reaction comment is also stored and rendered unsafely in the viewer reactions section.

The submitted HTML payloads execute JavaScript when the stored content is rendered, affecting users who view the page or movie details containing the injected content.

## Steps to Reproduce

### A. Stored XSS in Username

1. Open `https://netflix.pwnedbox.site/index.php`.
2. Open the Sign Up form.
3. Enter the following payload in the Username field:`Priyansh"><><Img SrC=x OnErRoR=confirm(12)>`
4. Complete the registration form and submit it.
5. Observe the JavaScript alert displaying `12`.
6. Dismiss the alert.
7. Inspect the page source and locate the username rendered in the navigation bar.
8. Verify that the username is inserted into the HTML without proper encoding.

### B. Stored XSS in Movie Reaction Comment

1. Sign in and open a movie details page, such as `https://netflix.pwnedbox.site/101`.
2. Navigate to the **Share Your Thoughts & Reactions** section.
3. Enter the following payload in the comment field:`Priyansh"><><Img SrC=x OnErRoR=confirm('Comment')>`
4. Submit the reaction.
5. Observe the JavaScript alert displaying `Comment`.
6. Dismiss the alert.
7. Inspect the movie details page source and locate the stored comment inside the viewer reactions section.
8. Verify that the comment is rendered without proper HTML encoding.

## POC (Proof of Concept)

**Screenshot 1:** Registration form containing the injected payloads in the Full Name and Username fields.

<img width="1918" height="965" alt="image" src="https://github.com/user-attachments/assets/63c189d2-257d-4f8e-88de-f62ca67f324f" />

**Screenshot 2:** JavaScript alert displaying `12` after registration, demonstrating execution of the stored username payload.

<img width="1918" height="965" alt="image" src="https://github.com/user-attachments/assets/e905e698-0936-40a5-99c8-39a335eda4f4" />

**Screenshot 3:** Page source showing the username rendered without proper HTML encoding in the navigation bar.

<img width="1916" height="955" alt="image" src="https://github.com/user-attachments/assets/7ed27674-5093-4cae-9117-22dd8a89c808" />

**Screenshot 4:** Movie reaction form containing the injected comment payload.

<img width="1908" height="963" alt="image" src="https://github.com/user-attachments/assets/7ff46672-7610-45d5-bd93-18d7ca9ec204" />

**Screenshot 5:** JavaScript alert displaying `Comment` after submitting the reaction.

<img width="1917" height="965" alt="image" src="https://github.com/user-attachments/assets/f67056aa-bea0-4418-9321-dee554c1c6b9" />

**Screenshot 6:** Page source showing the stored comment rendered without proper HTML encoding in the viewer reactions section.

<img width="1916" height="955" alt="image" src="https://github.com/user-attachments/assets/7e82f552-0fa2-45b6-835e-c0e4f4dd7c5c" />


## Impact
- It allows attackers to execute arbitrary JavaScript in the victim's browser within the context of the vulnerable application.
- It may allow attackers to hijack user sessions, steal sensitive information accessible to the application, or potentially lead to account takeover.
- It allows attackers to perform unauthorized actions on behalf of affected users, including modifying application data or abusing available account privileges.


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
