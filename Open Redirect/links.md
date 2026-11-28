## Title
Open Redirect - Omise
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://links.pwnedbox.site/

The application is vulnerable to an open redirect through URL path manipulation. By modifying the path with multiple forward slashes and an external hostname, an attacker can cause the application to issue an HTTP 302 redirect to an external website.

## Steps to Reproduce

1. Open the following crafted URL directly in your browser: https://links.pwnedbox.site////bing.com/index.php?category=developers&page=1
2. Observe that the application responds with an HTTP 302 redirect.
3. Verify that the browser is redirected to `https://www.bing.com/index.php?category=developers&page=1`.


## POC (Proof of Concept)

Screenshot 1: The original Omise Developer Blog page and the intercepted request in Burp Suite.

<img width="1918" height="1017" alt="image" src="https://github.com/user-attachments/assets/2ff715fb-e765-4ec4-bdc7-f5dbdcb10117" />

Screenshot 2: Burp Intruder results showing HTTP 302 responses for redirect payload variations.

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/07193ba6-cbb6-4f23-abca-dc4f61dd3597" />

Screenshot 3: The crafted URL in the browser and the resulting redirect to Bing.

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/9ff0dc61-b539-43e9-b826-57fbc36a7206" />

Screenshot 4: The external destination displaying the redirected request.

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/5281d327-267c-4f16-97fd-81fb9630112f" />


## Impact
- It allows attackers to redirect users to malicious or phishing websites through a trusted application URL.
- It may lead to credential theft, sensitive information disclosure, or social engineering attacks.
- It can damage the application's reputation and be abused to distribute malicious links that appear to originate from a trusted domain.

## Remediation
- Validate redirect destinations against a strict allowlist of trusted domains and reject unauthorized external destinations.
- Use predefined internal paths or relative URLs instead of accepting arbitrary user-controlled redirect destinations.
- Parse and normalize the destination URL, allow only approved schemes such as `https`, and redirect to a safe default page if validation fails.
