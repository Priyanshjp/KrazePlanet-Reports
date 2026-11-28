## Title
Open redirect using checkout_url
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://myshopify.pwnedbox.site/

The logout functionality accepts a user-controlled return destination through the `return_url` parameter. By supplying an external URL, an attacker can cause users to be redirected away from the trusted application to an external website.

The demonstrated redirect chain uses the store's URL redirect configuration to redirect the destination path to an external domain.

## Steps to Reproduce


1. Open the following crafted URL directly in the browser: https://myshopify.pwnedbox.site/account/logout?return_url=https://checkout.shopify.com/priyansh
2. Observe that the logout flow redirects the browser to the supplied destination.
3. The destination path `/priyansh` is configured in the store's URL redirects to point to `https://example.com`.
4. Observe that the browser is ultimately redirected to `example.com`, demonstrating the external redirect chain.

## POC (Proof of Concept)

Screenshot 1: The Aurora storefront and Admin navigation.

<img width="1913" height="1020" alt="image" src="https://github.com/user-attachments/assets/69485e66-070b-433d-87d3-4b391262c842" />

Screenshot 2: URL redirects configuration showing `/priyansh` redirecting to `https://example.com`.

<img width="1918" height="1017" alt="image" src="https://github.com/user-attachments/assets/46c6b786-3f68-4dce-8aa0-8c9d1e99ac7a" />

Screenshot 3: Crafted logout URL containing the external `return_url` destination.

<img width="1918" height="1072" alt="image" src="https://github.com/user-attachments/assets/140200f7-608f-4359-a8ab-8eda8acac8ed" />

Screenshot 4: Browser successfully redirected to `example.com`.

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/98faa8e5-a1eb-4bae-bd29-ffdacc30b52e" />


## Impact
- It allows attackers to redirect users to malicious or phishing websites through a trusted application URL.
- It may lead to credential theft, sensitive information disclosure, or social engineering attacks.
- It can damage the application's reputation and be abused to distribute malicious links that appear to originate from a trusted domain.

## Remediation
- Validate redirect destinations against a strict allowlist of trusted domains and reject unauthorized external destinations.
- Use predefined internal paths or relative URLs instead of accepting arbitrary user-controlled redirect destinations.
- Parse and normalize the destination URL, allow only approved schemes such as `https`, and redirect to a safe default page if validation fails.
