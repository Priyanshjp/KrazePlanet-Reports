## Title
Unauthorized Access to Administrative Interface via HTTP 302 Redirect


## Description
**Affected URL:** https://japan.pwnedbox.site/

The application exposes an administrative login interface at `/login.php`. The login page accepts the default credentials `admin:admin`, allowing access to the administrative dashboard without using a secure, unique password.

After authentication, the application redirects the user to `/dashboard.php`, where administrative information and functionality are accessible.

The initial request to the root URL returns an HTTP 302 redirect to the login page. The security issue is the use of weak default credentials that allow unauthorized administrative access.


## Steps to Reproduce

1. Open https://japan.pwnedbox.site/ in a browser.
2. Observe that the application redirects to the login interface.
3. Navigate to the login page at: https://japan.pwnedbox.site/login.php
4. Enter the following credentials:

   **Username:**`admin`
   Password:`admin`

6. Click Sign in.
7. Observe that authentication succeeds.
8. Verify that the browser redirects to:`https://japan.pwnedbox.site/dashboard.php`
9. Confirm that the Administration Dashboard is accessible and displays administrative sections such as Students, Faculty, Courses, Examinations, and Admissions.


## POC (Proof of Concept)

Screenshot 1: HTTP request to the application root.

<img width="1498" height="728" alt="image" src="https://github.com/user-attachments/assets/c41ab280-bb59-4444-803b-a510a405848d" />

Screenshot 2: Directory discovery identifies /login.php.

<img width="1906" height="772" alt="image" src="https://github.com/user-attachments/assets/b1dc3536-ef00-4be3-957f-503ab644f6f4" />

Screenshot 3: Administrative login page.

<img width="1918" height="970" alt="image" src="https://github.com/user-attachments/assets/911fd753-dcfb-476e-8364-680849a52b3a" />

Screenshot 4: Successful access to the Administration Dashboard using admin:admin.

<img width="1918" height="971" alt="image" src="https://github.com/user-attachments/assets/2c7f6895-d738-4a37-a7f1-ee80096ac620" />


## Impact

Impact
- An unauthorized user may gain administrative access using publicly known default credentials.
- Administrative functionality and potentially sensitive institutional information may be exposed.
- Depending on the permissions available in the dashboard, an attacker could modify or delete administrative records.


## Remediation

1. Remove default credentials and require a unique, strong password during initial setup.
2. Enforce strong password policies and multi-factor authentication for administrative accounts.
3. Implement login rate limiting, account lockout protections, and monitoring for suspicious authentication attempts.
