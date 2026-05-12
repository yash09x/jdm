# Google Sheets Integration (JDM Form)

1. Create a Google Sheet and rename first tab to `Leads`.
2. In Google Sheet: `Extensions -> Apps Script`.
3. Replace code with this:

```javascript
function doGet() {
  return ContentService
    .createTextOutput("JDM Sheets webhook is live")
    .setMimeType(ContentService.MimeType.TEXT);
}

function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Leads");
  if (!sheet) {
    sheet = SpreadsheetApp.getActiveSpreadsheet().insertSheet("Leads");
  }

  if (sheet.getLastRow() === 0) {
    sheet.appendRow([
      "submittedAt",
      "name",
      "phone",
      "car",
      "pickupDate",
      "tripType",
      "message"
    ]);
  }

  var data = JSON.parse(e.postData.contents || "{}");
  sheet.appendRow([
    data.submittedAt || new Date().toISOString(),
    data.name || "",
    data.phone || "",
    data.car || "",
    data.pickupDate || "",
    data.tripType || "",
    data.message || ""
  ]);

  return ContentService
    .createTextOutput(JSON.stringify({ ok: true }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

4. Click `Deploy -> New deployment`.
5. Type: `Web app`.
6. Execute as: `Me`.
7. Who has access: `Anyone`.
8. Deploy and copy Web app URL.
9. In `JDM.html`, update:

`const SHEETS_WEB_APP_URL = "PASTE_YOUR_GOOGLE_APPS_SCRIPT_WEBAPP_URL_HERE";`

Paste your URL there.

10. Submit form once and verify row is added in `Leads`.

If you redeploy a new version, update the web app URL in `JDM.html`.
