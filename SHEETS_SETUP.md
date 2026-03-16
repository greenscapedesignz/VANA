# 📊 VANA — Google Sheets Integration Setup

Connect VANA to your Google Sheet so every user registration is captured automatically.
**Estimated time: 10 minutes. No coding knowledge required.**

---

## What gets collected

Every time a new user registers on VANA, the following is written as a new row in your sheet:

| Column | Data |
|--------|------|
| A | Timestamp |
| B | Full name |
| C | Email address |
| D | Phone number |
| E | Studio / company |
| F | City |
| G | Role (landscape architect, developer, etc.) |
| H | How they heard about VANA |
| I | Their current location / scan city |

---

## Step 1 — Create your Google Sheet

1. Go to [sheets.google.com](https://sheets.google.com)
2. Click **Blank** to create a new spreadsheet
3. Rename it: click "Untitled spreadsheet" → type **VANA Users**
4. In **Row 1**, add these column headers exactly:

```
Timestamp | Name | Email | Phone | Studio | City | Role | Source | Scan Location
```

5. **Copy the Sheet ID** from the URL — it's the long string between `/d/` and `/edit`:
```
https://docs.google.com/spreadsheets/d/  THIS_IS_YOUR_SHEET_ID  /edit
```

---

## Step 2 — Create the Apps Script

1. In your Google Sheet, click **Extensions** → **Apps Script**
2. A new tab opens with a code editor
3. **Delete all existing code** in the editor
4. **Paste this exact code:**

```javascript
function doPost(e) {
  try {
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    const data = JSON.parse(e.postData.contents);
    
    sheet.appendRow([
      new Date().toLocaleString('en-IN', {timeZone: 'Asia/Kolkata'}),
      data.name        || '',
      data.email       || '',
      data.phone       || '',
      data.studio      || '',
      data.city        || '',
      data.role        || '',
      data.source      || '',
      data.location    || ''
    ]);
    
    return ContentService
      .createTextOutput(JSON.stringify({status: 'success'}))
      .setMimeType(ContentService.MimeType.JSON);
      
  } catch(err) {
    return ContentService
      .createTextOutput(JSON.stringify({status: 'error', message: err.toString()}))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

// Test function — run this manually to verify setup works
function testSheet() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  sheet.appendRow([
    new Date().toLocaleString('en-IN', {timeZone: 'Asia/Kolkata'}),
    'Test User',
    'test@example.com',
    '+91 98765 43210',
    'Test Studio',
    'Bengaluru',
    'Landscape architect',
    'Direct',
    'Bengaluru, Karnataka'
  ]);
  Logger.log('Test row added successfully');
}
```

5. Click the **Save** icon (floppy disk) or press `Ctrl+S`
6. Name the project: type **VANA Sheets** → click **OK**

---

## Step 3 — Test the script

1. In the function dropdown (shows "doPost"), select **testSheet**
2. Click **Run** (▶ play button)
3. Click **Review permissions** → select your Google account → click **Allow**
4. Go back to your Google Sheet — you should see a test row added
5. If you see the test row, the script is working correctly

---

## Step 4 — Deploy as a web app

1. Click **Deploy** → **New deployment**
2. Click the **gear icon** next to "Select type" → choose **Web app**
3. Fill in the deployment settings:

| Setting | Value |
|---------|-------|
| Description | VANA user registration |
| Execute as | **Me** (your Google account) |
| Who has access | **Anyone** |

4. Click **Deploy**
5. Click **Authorize access** → select your Google account → click **Allow**
6. You will see a screen with your **Web app URL** — it looks like:

```
https://script.google.com/macros/s/AKfycbw.../exec
```

7. **Copy this URL** — you will need it in the next step

---

## Step 5 — Add the URL to VANA

1. Open `index.html` in your code editor
2. Find this line near the top of the `<script>` block (around line 550):

```javascript
const SHEETS_URL = 'YOUR_APPS_SCRIPT_URL_HERE';
```

3. Replace `YOUR_APPS_SCRIPT_URL_HERE` with your actual URL:

```javascript
const SHEETS_URL = 'https://script.google.com/macros/s/AKfycbw.../exec';
```

4. Save the file
5. Push to GitHub:
```bash
git add .
git commit -m "Add Google Sheets integration"
git push
```

---

## Step 6 — Verify it works

1. Open your live VANA site
2. Clear your browser's localStorage to trigger the registration modal again:
   - Open browser DevTools (`F12`)
   - Go to **Application** → **Local Storage** → your site URL
   - Delete the `vana_registered` and `vana_user` entries
   - Refresh the page
3. The registration modal will appear
4. Fill in the form and click **Get started with VANA**
5. Check your Google Sheet — a new row should appear within a few seconds

---

## Troubleshooting

### Registration submits but no row appears in sheet

**Most likely cause:** The Apps Script deployment needs to be re-authorised after any code changes.

Solution:
1. Go back to Apps Script
2. Click **Deploy** → **Manage deployments**
3. Click the pencil (edit) icon on your deployment
4. Change the version to **New version**
5. Click **Deploy**

### "Script function not found: doPost" error in Apps Script

Make sure the function is named exactly `doPost` (lowercase d, uppercase P) and is not inside any other function.

### CORS error in browser console

This is expected with the `mode: 'no-cors'` setting — it means the data was sent successfully even though the browser cannot read the response. Check your sheet for the new row.

### Sheet not updating after new deployments

Every time you change the Apps Script code and redeploy, **you must create a New version** (not update the existing one) for changes to take effect.

---

## Making sense of your data

Once you have collected registrations, you can:

- **Filter by role** — see how many landscape architects vs developers vs homeowners
- **Filter by city** — understand geographic reach
- **Sort by timestamp** — track growth over time
- **Pivot by source** — see which referral channel (LinkedIn, Instagram, referral) brings most users
- **Create a chart** — insert → chart → line chart on timestamp column to see growth

---

## Privacy note

The data you collect should be used in accordance with applicable data protection laws (India's DPDP Act 2023 for Indian users). Consider adding a brief privacy note to the registration form if you plan to contact users for marketing purposes.

The registration form in VANA already includes this note:
> "Your details are collected securely to improve VANA. We do not share your information with third parties."

---

*For support: greenscapedesignz.com*
