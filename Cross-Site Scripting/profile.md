## Title
Reflected XSS - Function Name Filter

## Description
**Affected URL:** https://profile.pwnedbox.site/

The application accepts First Name (`fname`) and Last Name (`lname`) values through GET parameters. During testing, a crafted HTML event-handler payload was submitted through the form. The page source showed that the `fname` value was HTML-encoded, whereas the `lname` value was reflected without proper encoding.

The payload used JavaScript string concatenation to construct the `alert` function name. This bypassed the filtering behavior and triggered JavaScript execution through the `onpageshow` event.

## Steps to Reproduce

1. Open https://profile.pwnedbox.site/.
2. Locate the First Name and Last Name input fields.
3. Enter the crafted payload `"><body onpageshow=/*x*/this['a'+'l'+'e'+'r'+'t'](1)//>` into the input fields.
4. Submit the form.
5. Observe the JavaScript alert displaying `1`.
6. Inspect the page source and compare how the `fname` and `lname` values are reflected.
7. Verify that the `lname` value is reflected without proper HTML attribute encoding.

## POC (Proof of Concept)

Screenshot 1 – Crafted payload entered into the First Name and Last Name fields.

<img width="1918" height="957" alt="image" src="https://github.com/user-attachments/assets/9b9f0be3-c059-41ee-8f4c-bd6a574a9559" />

Screenshot 2 – JavaScript alert displaying `1` executes after form submission.

<img width="1918" height="968" alt="image" src="https://github.com/user-attachments/assets/ffb2bce3-30e4-4f9a-b0d6-38ce3455a582" />

Screenshot 3 – Page source shows HTML-encoded `fname` and unencoded payload reflection in `lname`.

<img width="1917" height="973" alt="image" src="https://github.com/user-attachments/assets/39c51679-8ef9-4cc2-a436-902b89537859" />

## Impact

- **Arbitrary JavaScript Execution:** An attacker may execute JavaScript in a victim's browser through a crafted URL.
- **Phishing and Content Manipulation:** An attacker may manipulate page content or display deceptive elements to visitors.
- **Victim-Targeted Attacks:** Crafted links may be distributed to users to trigger the reflected XSS when opened.
  
## Remediation

1. **Context-Aware Output Encoding:** Apply proper HTML attribute encoding to all user-controlled values before reflecting them in HTML.
2. **Consistent Parameter Handling:** Ensure both `fname` and `lname` receive the same secure validation and output-encoding treatment.
3. **Content Security Policy:** Implement a restrictive Content Security Policy and avoid relying on blacklist-based JavaScript filtering as the primary XSS defense.
