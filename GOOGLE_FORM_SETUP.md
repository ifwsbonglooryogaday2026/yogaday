# Google Form Setup for Yoga Day Registration

This guide shows how to create a Google Form and embed it into the site.

1. Create the Form
   - Go to https://forms.google.com and sign in with your Gmail account.
   - Click **Blank** to create a new form.
   - Add fields matching the registration form (Name, Email, Mobile, Address, Center, Experience, Agreement).

2. Configure Settings
   - Click the gear icon (Settings).
   - Under "General", optionally collect email addresses and limit to 1 response (if desired).
   - Under "Presentation", set a confirmation message.

3. Get the Embed Link
   - Click **Send** (top-right) → click the "<>" embed tab.
   - Copy the HTML iframe snippet or the `src` URL from it. The `src` looks like:
     - `https://docs.google.com/forms/d/e/FORM_ID/viewform?embedded=true`
   - The `FORM_ID` is the long identifier inside that URL.

4. Integrate into the site
   - Open `index.html` and find the Google Form section near the registration form.
   - Replace both occurrences of `FORM_ID` with your form's ID (or replace the full `src`/`href` URL).
   - Example replacement:
     - `https://docs.google.com/forms/d/e/1FAIpQLSeX...abc/viewform?embedded=true`

5. Test
   - Start a local server:
     ```powershell
     cd C:\Users\navee\yodaday
     python -m http.server 8000
     ```
   - Open `http://localhost:8000` and click "Open Google Form" or submit via the embedded form.
   - Check responses in the Google Form's Responses tab (and linked Google Sheet if you enabled it).

Notes
- Using a Google Form means responses are stored in Google Forms/Sheets; your existing EmailJS confirmation flow will not run for submissions made via Google Forms.
- If you want confirmation emails for Google Form submissions, set up an Apps Script trigger on form submission to send emails, or use an add-on like "Form Notifications".

If you want, I can:
- Create the Google Form for you (tell me the form fields and the Gmail account to use), or
- Add an Apps Script that sends confirmation emails on form submit.
