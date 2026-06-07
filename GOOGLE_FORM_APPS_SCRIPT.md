# Apps Script to email form submissions (paste into Extensions → Apps Script)

Use these default form field labels:
- Full Name
- Email Address
- Mobile Number
- Complete Address
- Study Center / Institute Name (Optional)
- Yoga Experience
- I agree to the terms and conditions

If your form uses different labels, update the `emailField` and `nameField` variables in the script below.

---

```javascript
function onFormSubmit(e) {
  var named = e.namedValues || {};
  var emailField = 'Email Address'; // change if your form uses a different label
  var nameField = 'Full Name'; // change if different

  // Generate a Registration ID
  var regId = 'IYD-' + Utilities.formatDate(new Date(), Session.getScriptTimeZone(), 'yyyyMMddHHmmss');

  // Build admin message
  var msg = 'New Yoga Day Registration\n\n';
  msg += 'Registration ID: ' + regId + '\n\n';
  for (var k in named) {
    msg += k + ': ' + named[k].join(', ') + '\n';
  }

  var subject = 'Yoga Day Registration — ' + regId;

  // Send to admin (script runs as the Google account owner)
  MailApp.sendEmail('ifwsbonglooryogaday@gmail.com', subject, msg);

  // Optional: send confirmation to respondent if email provided
  var recipient = named[emailField] ? named[emailField][0] : null;
  if (recipient) {
    var person = named[nameField] ? named[nameField][0] : '';
    var body = 'Dear ' + person + ',\n\n' +
               'Thank you for registering for International Yoga Day 2026.\n' +
               'Your Registration ID: ' + regId + '\n\n' +
               'Details submitted:\n';
    for (var k2 in named) {
      body += k2 + ': ' + named[k2].join(', ') + '\n';
    }
    body += '\nRegards,\nInternational Yoga Day 2026';
    MailApp.sendEmail(recipient, 'Registration Confirmation — ' + regId, body);
  }
}
```

Steps to install:
1. Open the linked Google Sheet for responses (Form → Responses → Open in Sheets).
2. Extensions → Apps Script.
3. Replace any existing code with the script above and Save.
4. Run the function `onFormSubmit` once to trigger the authorization prompt; grant permissions.
5. In Apps Script, open Triggers (left menu) → Add Trigger:
   - Function: `onFormSubmit`
   - Event source: `From spreadsheet`
   - Event type: `On form submit`
6. Test by submitting the Google Form. Admin notifications will be sent to `ifwsbonglooryogaday@gmail.com`.

Notes:
- Emails are sent from the Google account that owns the form/sheet. Sign in as `ifwsbonglooryogaday@gmail.com` if you want emails to originate from that address.
- Apps Script daily quotas apply.
