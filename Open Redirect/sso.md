## Title
Open Redirect - Tumblr
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://sso.pwnedbox.site/

The application contains an open redirect vulnerability in its logout functionality. The redirect destination can be manipulated to point to an external website, allowing an attacker to redirect users from the trusted application domain to an attacker-controlled or phishing destination.

## Steps to Reproduce

1. Open the following crafted URL directly in your browser: https://sso.pwnedbox.site/?action=logout&redirect_to=https://bing.com%5C@www.tumblr.com/dashboard
2. Observe that the application responds with an HTTP 302 redirect.
3. In Burp Suite, verify the response contains a `Location` header pointing to: `https://bing.com\@www.tumblr.com/dashboard`
4. Observe that the browser navigates to Bing, demonstrating that the external redirect destination can be controlled.

## POC (Proof of Concept)

Screenshot 1: Tumblr dashboard and the intercepted logout request in Burp Suite.

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/01c2b5e6-378c-442e-8d51-d1d315dadef5" />

Screenshot 2: Burp Intruder results showing HTTP 302 responses for redirect payload variations.
 
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/abe18098-593b-4d12-a82c-898de29d720d" />

Screenshot 3: The crafted URL containing the manipulated redirect destination.

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/c1e784bc-668e-43f6-9191-48d4733389c9" />

Screenshot 4: The browser redirected to Bing, confirming the open redirect behavior.

<img width="1888" height="1002" alt="image" src="https://github.com/user-attachments/assets/d70b17ea-959a-45df-a907-58425e057eb7" />


## Impact
- It allows attackers to redirect users to malicious or phishing websites through a trusted application URL.
- It may lead to credential theft, sensitive information disclosure, or social engineering attacks.
- It can damage the application's reputation and be abused to distribute malicious links that appear to originate from a trusted domain.

## Remediation
- Validate redirect destinations against a strict allowlist of trusted domains and reject unauthorized external destinations.
- Use predefined internal paths or relative URLs instead of accepting arbitrary user-controlled redirect destinations.
- Parse and normalize the destination URL, allow only approved schemes such as `https`, and redirect to a safe default page if validation fails.
