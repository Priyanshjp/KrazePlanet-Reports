## Title
Basic URL Parameter Redirect

## Description
**Affected URL:** https://redirect.pwnedbox.site/

A basic open redirect vulnerability was identified in the `url` query parameter of the affected endpoint. The application accepts a user-controlled destination and redirects the browser to the specified external URL without sufficient validation. This behavior was observed by intercepting the request in Burp Suite and modifying the destination to an external website.

## Steps to Reproduce

1. Open the following crafted URL in a browser: https://redirect.pwnedbox.site/?url=https%3A%2F%2Fwww.postgresql.org%2Fabout%2Fnews%2F
2. Allow the request to load.
3. Observe that the browser redirects to `https://www.postgresql.org/about/news/`.
4. Intercept the request using Burp Suite Proxy.
5. Inspect the `url` query parameter and verify that it contains the external destination.
6. Modify the destination to another external URL and forward the request https://redirect.pwnedbox.site/?url=https://bing.com .
7. Confirm that the application redirects the browser to the supplied destination.


## POC (Proof of Concept)

Screenshot 1: DevPulse page and Burp Suite request showing the url parameter pointing to PostgreSQL News.

<img width="1917" height="1025" alt="image" src="https://github.com/user-attachments/assets/16515f15-f1e0-4034-b089-392749ab510a" />

Screenshot 2: Modified redirect destination observed in Burp Suite, showing the browser navigating to the supplied external URL.

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/5ea0e9b0-6895-4c0e-9515-bf55545e59cf" />

Screenshot 3: Browser redirected to Bing after the redirect request was forwarded.

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/00c2239b-b0d5-464c-b609-5d518f275112" />



## Impact
- It allows attackers to redirect users to malicious or phishing websites through a trusted application URL.
- It may lead to credential theft, sensitive information disclosure, or social engineering attacks.
- It can damage the application's reputation and be abused to distribute malicious links that appear to originate from a trusted domain.

## Remediation
- Validate redirect destinations against a strict allowlist of trusted domains and reject unauthorized external destinations.
- Use predefined internal paths or relative URLs instead of accepting arbitrary user-controlled redirect destinations.
- Parse and normalize the destination URL, allow only approved schemes such as `https`, and redirect to a safe default page if validation fails.
