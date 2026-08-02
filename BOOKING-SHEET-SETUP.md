# Booking Database — Google Sheet Setup (one-time, ~3 minutes)

The booking form on `book.html` saves every request into your Google Sheet:
https://docs.google.com/spreadsheets/d/1GFyaPiBWergJKMKeKlnj3Hh-2yiyuTtaJ3iP65nt0zM/

A static website cannot write to a Google Sheet directly (that would require
exposing write credentials to visitors). The standard, free bridge is a tiny
**Google Apps Script Web App** attached to your sheet. Set it up once:

## Step 1 — Open the script editor
1. Open the Google Sheet linked above (with the Google account that owns it).
2. Menu: **Extensions → Apps Script**.

## Step 2 — Paste this code (replace anything in the editor)

```javascript
const SHEET_ID = '1GFyaPiBWergJKMKeKlnj3Hh-2yiyuTtaJ3iP65nt0zM';

function doPost(e) {
  const sh = SpreadsheetApp.openById(SHEET_ID).getSheets()[0];
  const d = JSON.parse(e.postData.contents);
  if (sh.getLastRow() === 0) {
    sh.appendRow(['Timestamp', 'Service', 'Preferred date', 'Preferred time',
                  'Name', 'Phone', 'Email', 'Notes', 'Status']);
  }
  sh.appendRow([new Date(), d.service || '', d.date || '', d.time || '',
                d.name || '', d.phone || '', d.email || '', d.notes || '', 'New']);
  return ContentService.createTextOutput(JSON.stringify({ ok: true }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

3. Click the 💾 save icon.

## Step 3 — Deploy as a Web App
1. Click **Deploy → New deployment**.
2. Gear icon → select **Web app**.
3. Settings:
   - Description: `Booking form`
   - Execute as: **Me**
   - Who has access: **Anyone**  ← required so the website can post to it
4. Click **Deploy**, approve the permissions prompt (it's your own script
   writing to your own sheet), and **copy the Web app URL** — it ends in `/exec`.

## Step 4 — Paste the URL into the website
Open `book.html`, find this line near the bottom (inside the booking script):

```javascript
  var SHEET_WEBHOOK = "";
```

Paste your URL between the quotes:

```javascript
  var SHEET_WEBHOOK = "https://script.google.com/macros/s/XXXXXXXX/exec";
```

Commit/push (or send the URL to Claude and it will be wired in for you).

## How it works after setup
- Every booking request writes a row: Timestamp, Service, Preferred date,
  Preferred time, Name, Phone, Email, Notes, Status ("New" — edit this column
  as you confirm appointments).
- The email notification to info@ivsforme.com still sends in parallel, so the
  sheet is the database and the email is the alert.
- If the sheet write ever fails, the booking still goes through by email —
  the sheet call is fire-and-forget and never blocks the patient.

## Notes
- Header row is created automatically on the first booking if the sheet is empty.
- To change which tab receives bookings, replace `getSheets()[0]` with
  `getSheetByName('Bookings')`.
- If you later edit the Apps Script, use **Deploy → Manage deployments →
  edit (pencil) → Version: New version** so the same /exec URL keeps working.
