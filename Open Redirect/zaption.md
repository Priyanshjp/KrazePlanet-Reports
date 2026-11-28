## Title
Open redirect filter bypass

## Description
**Affected URL:** https://zaption.pwnedbox.site/

The logout functionality accepts a user-controlled redirect destination through the `returnTo` parameter. By using a crafted URL with encoded characters and backslashes, the redirect validation can be bypassed, causing the application to redirect users to an external website.

## Steps to Reproduce

1. Open the following crafted URL directly in the browser: https://zaption.pwnedbox.site/logout?returnTo=/%2f%5c%5c%2fbing%2ecom%2f
2. Observe that the application processes the crafted redirect destination.
3. The server responds with an HTTP `302 Found` status and a `Location` header pointing to `///bing.com/`.
4. The browser follows the redirect and navigates to Bing.

## POC (Proof of Concept)

**Screenshot 1:** Shows the authenticated Zaption account and the logout functionality.

<img width="1918" height="1016" alt="image" src="https://github.com/user-attachments/assets/d71a9870-4923-4658-b4cc-1aabe0010324" />

**Screenshot 2:** Burp Suite Intruder shows the crafted redirect payload and the HTTP `302 Found` response with the external `Location` header.

<img width="1918" height="1015" alt="image" src="https://github.com/user-attachments/assets/59797a9f-ce31-4c79-b98a-80507de45302" />

**Screenshot 3:** Shows the crafted URL containing encoded characters in the redirect destination.

<img width="1918" height="1020" alt="image" src="https://github.com/user-attachments/assets/a1a5d648-6c83-4953-b6a1-6a6cea3c5efb" />

**Screenshot 4:** Shows the browser navigating to the external Bing website after the redirect.

<img width="1918" height="1012" alt="image" src="https://github.com/user-attachments/assets/c74c8278-0840-4c60-884e-d4f26aa22597" />


## Impact
- It allows attackers to redirect users to malicious or phishing websites through a trusted application URL.
- It may lead to credential theft, sensitive information disclosure, or social engineering attacks.
- It can damage the application's reputation and be abused to distribute malicious links that appear to originate from a trusted domain.

## Remediation
- Validate redirect destinations against a strict allowlist of trusted domains and reject unauthorized external destinations.
- Use predefined internal paths or relative URLs instead of accepting arbitrary user-controlled redirect destinations.
- Parse and normalize the destination URL, allow only approved schemes such as `https`, and redirect to a safe default page if validation fails.
