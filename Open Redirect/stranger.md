## Title
Open Redirect on HackerOne
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://stranger.pwnedbox.site/

The application accepts a crafted URL containing backslashes and redirects the browser to an external website. During testing, specially crafted URL paths were processed as external redirect destinations, allowing navigation away from the trusted application domain.

## Steps to Reproduce

1. Open https://stranger.pwnedbox.site/ in a browser.
2. Observe the application's homepage.
3. Send the following request using Burp Suite Repeater or open the URL directly in a browser: https://stranger.pwnedbox.site/\\//bing.com/
4. Observe that the application responds with **HTTP 302 Found**.
5. Inspect the response's `Location` header, which points to an external destination.
6. Open the crafted URL in a browser and observe that it navigates to Bing.

   
## POC (Proof of Concept)


Screenshot 1: HackerOne-style homepage of `stranger.pwnedbox.site`, showing the target application.

<img width="1918" height="1013" alt="image" src="https://github.com/user-attachments/assets/0b76a7f8-6937-49e8-9925-9a5e4d9b6604" />

Screenshot 2: Burp Suite Intruder results showing HTTP 302 responses for crafted redirect payloads.

<img width="1918" height="1006" alt="image" src="https://github.com/user-attachments/assets/bf107727-a867-43c0-8c5d-838a86afa14a" />

Screenshot 3: Crafted URL containing backslashes and an external destination.

<img width="1918" height="1020" alt="image" src="https://github.com/user-attachments/assets/82fa4a69-57aa-46ce-abfe-7d3185738a1a" />

Screenshot 4: Browser redirected to Bing after opening the crafted URL.

<img width="1918" height="1020" alt="image" src="https://github.com/user-attachments/assets/f474b621-ac18-4ce9-9be8-167b73011922" />


## Impact
- It allows attackers to redirect users to malicious or phishing websites through a trusted application URL.
- It may lead to credential theft, sensitive information disclosure, or social engineering attacks.
- It can damage the application's reputation and be abused to distribute malicious links that appear to originate from a trusted domain.

## Remediation
- Validate redirect destinations against a strict allowlist of trusted domains and reject unauthorized external destinations.
- Use predefined internal paths or relative URLs instead of accepting arbitrary user-controlled redirect destinations.
- Parse and normalize the destination URL, allow only approved schemes such as `https`, and redirect to a safe default page if validation fails.
