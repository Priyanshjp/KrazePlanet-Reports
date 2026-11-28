## Title
Open Redirect - Semrush

## Description
**Affected URL:** https://out.pwnedbox.site/

The application contains an open redirect vulnerability in its redirect functionality. By supplying an external destination through a user-controlled URL parameter, an attacker can cause the application to redirect users to an arbitrary external website.

## Steps to Reproduce

1. Open the following crafted URL directly in your browser: https://out.pwnedbox.site/index.php?url=//bing.com
2. Observe that the application responds with an HTTP 302 redirect.
3. Verify that the response contains the following redirect header: `Location: //bing.com`
4. The browser resolves the protocol-relative destination and redirects to `https://bing.com`.


## POC (Proof of Concept)

Screenshot 1: Semrush page displaying the external source link and the intercepted request in Burp Suite.

<img width="1910" height="1018" alt="image" src="https://github.com/user-attachments/assets/ca266ba1-d04f-4701-9a98-b78f08f05949" />

Screenshot 2: Burp Intruder results showing HTTP 302 responses for redirect payload variations.

<img width="1918" height="1018" alt="image" src="https://github.com/user-attachments/assets/7530e66e-a033-46bc-8eb5-e45faa3b8cfe" />

Screenshot 3: The crafted URL containing the external redirect destination.

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/278cc149-c55b-4d8b-b743-c5a35f98297c" />

Screenshot 4: The browser successfully redirected to Bing.

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/375d8b56-7bd5-4463-80bd-d0e96e1c6834" />


## Impact
- It allows attackers to redirect users to malicious or phishing websites through a trusted application URL.
- It may lead to credential theft, sensitive information disclosure, or social engineering attacks.
- It can damage the application's reputation and be abused to distribute malicious links that appear to originate from a trusted domain.

## Remediation
- Validate redirect destinations against a strict allowlist of trusted domains and reject unauthorized external destinations.
- Use predefined internal paths or relative URLs instead of accepting arbitrary user-controlled redirect destinations.
- Parse and normalize the destination URL, allow only approved schemes such as `https`, and redirect to a safe default page if validation fails.
