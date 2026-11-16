## Title
Reflected XSS - Event Handler Filter

## Description
**Affected URL:** https://tickets.pwnedbox.site/

The application accepts First Name (`fname`) and Last Name (`lname`) values through GET parameters. During testing, an event-handler payload using JavaScript string concatenation was submitted to bypass basic function-name filtering.

Source inspection showed that the `fname` value was HTML-encoded, whereas the `lname` value was reflected as raw HTML inside its input element. This allowed the injected HTML event handler to execute JavaScript in the browser, confirming a Reflected Cross-Site Scripting (XSS) vulnerability.

## Steps to Reproduce

1. Open https://tickets.pwnedbox.site/.
2. Locate the First Name and Last Name input fields.
3. Submit the crafted event-handler payload `"><body onpageshow=/*x*/this['a'+'l'+'e'+'r'+'t'](1)//>` through the GET parameters.
4. Observe the JavaScript alert displaying `1`.
5. Inspect the page source.
6. Compare the output handling of `fname` and `lname`.
7. Verify that the `lname` value is reflected without proper HTML attribute encoding.

## POC (Proof of Concept)

Screenshot 1 – Rendered page showing the submitted payload and its reflection.

<img width="1918" height="945" alt="image" src="https://github.com/user-attachments/assets/cddb6055-7da4-4ef4-82aa-0c59cbcd66c1" />

Screenshot 2 – JavaScript alert displaying `1`, confirming successful execution.

<img width="1917" height="953" alt="image" src="https://github.com/user-attachments/assets/5614d7c9-527b-4307-a4eb-10fec204dc60" />

Screenshot 3 – Page source showing encoded `fname` and unescaped event-handler payload reflection in `lname`.

<img width="1918" height="971" alt="image" src="https://github.com/user-attachments/assets/49259973-a9ab-46f1-80ff-bc58c60367eb" />

## Impact

- **Arbitrary JavaScript Execution:** An attacker may execute JavaScript in a victim's browser through a crafted URL.
- **Phishing and Content Manipulation:** An attacker may modify displayed page content or create deceptive elements.
- **Victim-Targeted Attacks:** Crafted links may be distributed to users to trigger the reflected XSS when opened.

## Remediation

1. **Context-Aware Output Encoding:** Apply proper HTML attribute encoding to all user-controlled values before inserting them into input elements.
2. **Consistent Parameter Handling:** Ensure both `fname` and `lname` receive the same secure validation and output-encoding treatment.
3. **Content Security Policy:** Implement a restrictive Content Security Policy and avoid relying on blacklist-based event-handler filtering as the primary XSS defense.
