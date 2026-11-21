## Title
404 Response-Based Discovery Leads to Open Redirect
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://research.pwnedbox.site/

The `/researchers.php` endpoint was discovered during directory enumeration after the root page returned a 404 response. The endpoint accepts a `next` parameter that controls the redirect destination. By supplying an external URL, the application responds with HTTP 302 and redirects the browser to the attacker-controlled destination without validating it. This behavior can be abused to create a trusted-domain URL that redirects users to an external website.

## Steps to Reproduce

1. Open the following crafted URL in a browser:https://research.pwnedbox.site/researchers.php?next=https://example.com

2. Observe that the request is processed by `researchers.php`.
3. The server responds with an HTTP `302 Found` status.
4. Inspect the response headers and verify that the `Location` header is set to `https://example.com`.
5. Allow the browser to follow the redirect.
6. Confirm that the browser navigates to `example.com`.

   
## POC (Proof of Concept)

**Screenshot 1:** The root page displays a 404 response.

<img width="1918" height="970" alt="image" src="https://github.com/user-attachments/assets/548256e7-2b7a-46a2-ae95-6817023799a1" />

**Screenshot 2:** Directory enumeration identifies `publications.php` and `researchers.php` as accessible endpoints.

<img width="1897" height="955" alt="image" src="https://github.com/user-attachments/assets/dcf92a47-a601-4974-922c-0037a0b93c1d" />

**Screenshot 3:** The `researchers.php?next=/` endpoint displays the researchers page.

<img width="1918" height="961" alt="image" src="https://github.com/user-attachments/assets/23234e7b-c414-405a-bacf-0715f502c902" />

**Screenshot 4:** The crafted URL with `next=https://example.com` is entered in the browser.

<img width="1911" height="961" alt="image" src="https://github.com/user-attachments/assets/b0c36932-f29f-4747-85bd-87e5e226a14b" />

**Screenshot 5:** Burp Suite shows the request containing the `next` parameter and the server's HTTP 302 response with `Location: https://example.com`.

<img width="1918" height="890" alt="image" src="https://github.com/user-attachments/assets/787985d1-ce62-4809-a499-8106699aff03" />

**Screenshot 6:** The browser follows the redirect and loads `example.com`.

<img width="1918" height="965" alt="image" src="https://github.com/user-attachments/assets/d74d43c8-5b8c-49b8-9c10-23b2e7b7e6cd" />


## Impact
- It allows attackers to redirect users from a trusted website to malicious or phishing websites.
- It can be used to steal user credentials through convincing phishing pages and increase the risk of account compromise.
- It may be abused in phishing campaigns, social engineering attacks, and to bypass security mechanisms that trust links from the vulnerable domain.


## Remediation
- Validate the `next` parameter against a strict allowlist of approved redirect destinations. Prefer relative paths within the application.
- Reject untrusted absolute URLs, protocol-relative URLs (`//attacker.com`), and URLs using unexpected schemes such as `javascript:`. Do not rely only on string matching or blacklist-based filtering.
- If external redirects are necessary, show a warning or confirmation page before redirecting users. Ensure redirect destinations are validated server-side before issuing HTTP 301, 302, 303, 307, or 308 responses.
