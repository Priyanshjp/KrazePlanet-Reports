## Title
Reflected XSS - Case-Insensitive Filter Bypass

## Description
**Affected URL:** https://directory.pwnedbox.site/

The application accepts First Name (`fname`) and Last Name (`lname`) through GET parameters. During testing, a mixed-case script tag payload was submitted through both parameters.

The application reflected the supplied values inside the HTML input elements without proper output encoding. The mixed-case `<ScRipT>` tag was interpreted by the browser, resulting in successful JavaScript execution. This demonstrates a Reflected Cross-Site Scripting (XSS) vulnerability affecting both parameters.

## Steps to Reproduce

1. Open https://directory.pwnedbox.site/.
2. Locate the First Name and Last Name input fields.
3. Enter the crafted payload `Priyanshxop'"<><ScRipT>alert(1)</ScRipT>` into both fields.
4. Submit the form.
5. Observe the JavaScript alert displaying `1`.
6. Inspect the page source.
7. Verify that the payload is reflected inside both input elements without proper HTML encoding.
   
## POC (Proof of Concept)

Screenshot 1 – Mixed-case XSS payload entered into both First Name and Last Name fields.

<img width="1918" height="963" alt="image" src="https://github.com/user-attachments/assets/56e6ab31-c320-4869-8ba4-f21173d1cb78" />

Screenshot 2 – JavaScript alert displaying `1` executes successfully.

<img width="1918" height="970" alt="image" src="https://github.com/user-attachments/assets/82da2268-55af-4948-83e8-ba44ace5806e" />

Screenshot 3 – Page source confirms unescaped payload reflection in both `fname` and `lname` input elements.

<img width="1918" height="957" alt="image" src="https://github.com/user-attachments/assets/3139a0be-1880-467f-aac5-e4c7c7ce0f4d" />


## Impact

- **Arbitrary JavaScript Execution:** An attacker may execute JavaScript in a victim's browser through a crafted URL.
- **Phishing and Content Manipulation:** An attacker may manipulate page content or display deceptive elements.
- **Victim-Targeted Attacks:** Crafted links may be distributed to users to trigger the reflected XSS when opened.

## Remediation

1. **Context-Aware Output Encoding:** Properly HTML-attribute-encode user-controlled values before inserting them into input elements.
2. **Secure Input Handling:** Apply consistent validation and output encoding to both `fname` and `lname` parameters.
3. **Content Security Policy:** Implement a restrictive Content Security Policy and avoid relying on case-sensitive or blacklist-based script filtering as the primary XSS defense.
