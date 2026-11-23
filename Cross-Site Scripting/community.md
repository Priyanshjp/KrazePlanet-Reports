## Title
Stored XSS - Forum Discussions (Reddit Clone)
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://community.pwnedbox.site/

Stored Cross-Site Scripting (XSS) was identified in the Community Hub application. User-controlled input submitted through registration/profile fields and discussion comments is stored and later rendered in the application without proper output encoding. The supplied HTML payload executes JavaScript when another page displays the stored content. This may affect users who view the vulnerable profile or discussion content.


## Steps to Reproduce

1. Open https://community.pwnedbox.site/ and navigate to the Register page.
2. Enter a valid email address and complete the required registration fields.
3. Enter the following payload into a user-controlled profile field, such as First Name, About Me, or Signature:`Priyansh'"><><img src=x onerror=confirm(1)>`
4. Submit the registration form and verify that the account is successfully created.
5. Navigate to the profile page and observe the JavaScript confirmation dialog triggered by the stored input.
6. Log in to the account and navigate to a discussion post.
7. In the comment field, enter the following payload:`Priyansh'"><><img src=x onerror=confirm('Policies')>`
8. Submit the comment.
9. Reload or revisit the discussion page and observe the confirmation dialog when the stored comment is rendered.

## POC (Proof of Concept)

Screenshot 1: Registration form containing HTML injection payloads in multiple user-controlled fields.

<img width="1902" height="968" alt="image" src="https://github.com/user-attachments/assets/7010dff5-e564-4dff-bd8f-41f934b5ec5b" />

Screenshot 2: Registration completed successfully, confirming the submitted profile data was accepted.

<img width="1918" height="971" alt="image" src="https://github.com/user-attachments/assets/ebdc2ede-9060-43f2-8274-01cb8edc3099" />

Screenshot 3: JavaScript confirmation dialog triggered while viewing the stored user profile.

<img width="1918" height="968" alt="image" src="https://github.com/user-attachments/assets/28de2d48-5ce1-48c1-846d-928fc5040592" />

Screenshot 4: Raw HTML Reflection in Profile Source

<img width="1917" height="972" alt="image" src="https://github.com/user-attachments/assets/287d28ae-95fa-45f3-9889-05b451a2c4a6" />

Screenshot 5: Discussion comment containing the XSS payload submitted through the comment form.

<img width="1915" height="973" alt="image" src="https://github.com/user-attachments/assets/3628c0c7-c3ce-4900-9d2b-770870257d6e" />

Screenshot 6: JavaScript confirmation dialog triggered when the stored discussion comment is rendered.

<img width="1918" height="965" alt="image" src="https://github.com/user-attachments/assets/5846a12a-ff2b-4eba-8b88-7a9db23b8711" />

Screenshot 7: Source code showing the stored comment payload in the discussion data.

<img width="1915" height="972" alt="image" src="https://github.com/user-attachments/assets/fd6869ad-eb29-4c73-a4da-da962c0fae32" />



## Impact
- It allows attackers to execute arbitrary JavaScript in the victim's browser within the context of the vulnerable application.
- It may allow attackers to hijack user sessions, steal sensitive information accessible to the application, or potentially lead to account takeover.
- It allows attackers to perform unauthorized actions on behalf of affected users, including modifying application data or abusing available account privileges.


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
