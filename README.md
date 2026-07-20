# n8n Google traffic report

Import `google-traffic-report.n8n.json` into your local n8n. It creates a new Google Sheets spreadsheet in Google Drive on every run with five tabs:

- Executive Overview (headline KPIs, acquisition mix, content performance, organic search, prioritized SEO opportunities, and analyst takeaways)
- Traffic Sources (channel and source/medium)
- Landing Pages
- Search Queries (clicks, impressions, CTR, and average position)
- Search Pages

After creating the report, the workflow sets the spreadsheet to **Anyone with the link — Viewer** and emails the link to the configured recipients.

## Setup

1. In Google Cloud, enable **Google Analytics Data API**, **Google Search Console API**, and **Google Sheets API**.
2. Configure a Google OAuth2 API credential in n8n. Include these scopes:
   - `https://www.googleapis.com/auth/analytics.readonly`
   - `https://www.googleapis.com/auth/webmasters.readonly`
   - `https://www.googleapis.com/auth/spreadsheets`
   - `https://www.googleapis.com/auth/drive.file`
3. Enable **Google Drive API** in the same Google Cloud project. The Drive File scope only permits the workflow to manage files created through the connected app.
4. Import the workflow JSON.
5. Open **CONFIG - Edit These Values** and replace:
   - `REPLACE_WITH_GA4_PROPERTY_ID` with the numeric GA4 Property ID (not the `G-...` Measurement ID).
   - `sc-domain:example.com` with the exact property shown in Search Console. URL-prefix properties look like `https://example.com/`.
   - Optionally change the report name and 30-day reporting window.
   - Add comma-separated addresses under `report_recipients`.
   - Set `report_from_email` to the sender address used by the SMTP account.
6. Open each HTTP Request node and select the same Google OAuth2 API credential. Reconnect it after adding the Drive File scope.
7. Create or select an SMTP credential in **Email Report Link**. For Gmail SMTP, use an app password when two-step verification is enabled.
8. Save and run **Run Manually** once. The workflow shares the spreadsheet, returns its URL, and emails it.
9. When the test succeeds, activate the workflow. It runs Mondays at 8:00 AM in the workflow timezone.

The workflow is inactive on import and contains no credential IDs or secrets. Anyone who receives the generated link can view that report, so do not use public-link sharing for sensitive analytics.
