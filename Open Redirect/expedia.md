## Title
Open Redirect in Logout & Login

## Description
**Affected URL:** https://expedia.pwnedbox.site/

The logout functionality accepts a user-controlled redirect destination through the `logout` parameter. By modifying this parameter to an external URL, an attacker can redirect users from the trusted application to an arbitrary external website.

## Steps to Reproduce

1. Open https://expedia.pwnedbox.site/ and log in to the application.
2. Click the account menu and select **Sign out**.
3. Observe that the logout functionality uses the `logout` parameter.
4. Modify the parameter to an external URL using the following crafted URL: https://expedia.pwnedbox.site/?logout=https://www.bing.com
5. Open the crafted URL directly in the browser.
6. Observe that the user is redirected to `https://www.bing.com` instead of remaining on the application.


## POC (Proof of Concept)


Screenshot 1: Account menu showing the Sign out option.

<img width="1918" height="1016" alt="image" src="https://github.com/user-attachments/assets/284a7f39-5741-46d7-b7aa-1443d4d34dda" />

Screenshot 2: Logout functionality using the `?logout=1` parameter.

<img width="1918" height="1016" alt="image" src="https://github.com/user-attachments/assets/464cd81a-8ac9-4e92-bbc4-e1462f7268db" />

Screenshot 3: Modified logout parameter containing an external destination.

<img width="1918" height="1017" alt="image" src="https://github.com/user-attachments/assets/0a3256f1-20eb-497a-9a84-e29ec9e00eb1" />

Screenshot 4: Browser successfully redirected to `bing.com`.

<img width="1918" height="1015" alt="image" src="https://github.com/user-attachments/assets/9710f0fb-110f-45f6-99be-3bfa76e7beb7" />

## Impact
- It allows attackers to redirect users to malicious or phishing websites through a trusted application URL.
- It may lead to credential theft, sensitive information disclosure, or social engineering attacks.
- It can damage the application's reputation and be abused to distribute malicious links that appear to originate from a trusted domain.

## Remediation
- Validate redirect destinations against a strict allowlist of trusted domains and reject unauthorized external destinations.
- Use predefined internal paths or relative URLs instead of accepting arbitrary user-controlled redirect destinations.
- Parse and normalize the destination URL, allow only approved schemes such as `https`, and redirect to a safe default page if validation fails.
