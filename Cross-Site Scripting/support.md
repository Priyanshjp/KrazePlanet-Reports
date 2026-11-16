## Title
Reflected XSS - Script & Img Tag Filter

## Description
**Affected URL:** https://support.pwnedbox.site/

The application accepts First Name (`fname`) and Last Name (`lname`) values through GET parameters. During testing, HTML event-handler payloads were submitted through both parameters.

The application reflected the supplied values inside the HTML input elements without proper context-aware output encoding. By injecting a crafted `<body>` element with an `onpageshow` event handler, JavaScript execution was achieved in the browser. This confirms a Reflected Cross-Site Scripting (XSS) vulnerability affecting both parameters.

## Steps to Reproduce

1. Open https://support.pwnedbox.site/.
2. Locate the First Name and Last Name input fields.
3. Enter the crafted payload `'"<><body onpageshow=alert(1)>` into both fields.
4. Submit the form.
5. Observe the JavaScript alert displaying `1`.
6. Inspect the page source.
7. Verify that the payload is reflected inside both input elements without proper HTML encoding.


## POC (Proof of Concept)

Screenshot 1 – Crafted payload entered into both First Name and Last Name fields.

<img width="1918" height="958" alt="image" src="https://github.com/user-attachments/assets/567363c4-ecc3-4b2e-9710-68768880cfe2" />

Screenshot 2 – JavaScript alert displaying `1` executes successfully after submission.

<img width="1918" height="956" alt="image" src="https://github.com/user-attachments/assets/b1f85ff7-d880-4534-ae91-4c34ab3be7fb" />

Screenshot 3 – Page source confirms unescaped payload reflection in both `fname` and `lname` input elements.

<img width="1916" height="963" alt="image" src="https://github.com/user-attachments/assets/e8ddb56e-c7cb-4fec-8af0-864dca0e9da5" />

## Impact

- **Arbitrary JavaScript Execution:** An attacker may execute JavaScript in a victim's browser through a crafted URL.
- **Phishing and Content Manipulation:** An attacker may manipulate page content or display deceptive elements.
- **Victim-Targeted Attacks:** Crafted links may be distributed to users to trigger the reflected XSS when opened.

## Remediation

1. **Context-Aware Output Encoding:** Properly HTML-attribute-encode user-controlled values before inserting them into input elements.
2. **Consistent Parameter Validation:** Apply secure validation and output encoding to both `fname` and `lname` parameters.
3. **Content Security Policy:** Implement a restrictive Content Security Policy and avoid relying on blacklist-based HTML tag filtering as the primary XSS defense.
