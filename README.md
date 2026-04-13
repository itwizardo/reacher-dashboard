# Reacher Dashboard

A single-file HTML dashboard for email verification using the [Reacher](https://reacher.email/) API.

## What it does

- **Single email verification** -- type an email and verify it instantly
- **Bulk verification** -- drag-and-drop a CSV file or paste a list of emails (one per line)
- **Results table** with color-coded status badges: safe (green), risky (yellow), invalid (red), unknown (gray)
- **Stats summary** showing counts for total checked, safe, risky, and invalid
- **CSV export** of all results
- **Configurable API URL** -- point it at any Reacher backend instance

## How to use

1. **Start your Reacher backend** (default: `http://localhost:8080`). See [Reacher's docs](https://help.reacher.email/) for setup.

2. **Open the dashboard** in a browser:
   ```
   open index.html
   ```

3. **Set the API URL** in the top-right field if your Reacher instance is not on the default `http://localhost:8080`.

4. **Verify emails:**
   - Type a single email into the input field and click **Verify**
   - For bulk verification, either drag a CSV file onto the drop zone or paste emails into the text area, then click **Verify All**

5. **Export results** by clicking the **Export CSV** button that appears once you have results.

## API details

The dashboard calls `POST {apiUrl}/v0/check_email` with:
```json
{ "to_email": "user@example.com" }
```

It extracts these fields from the response:

| Field               | Description                          |
|---------------------|--------------------------------------|
| `is_reachable`      | Overall status: safe/risky/invalid/unknown |
| `smtp.is_deliverable` | Whether the mailbox exists         |
| `smtp.is_catch_all`   | Whether the domain is catch-all    |
| `misc.is_disposable`  | Whether the email is disposable    |
| `mx.accepts_mail`     | Whether MX records are valid       |
| `mx.records`          | List of MX record hostnames        |

## Requirements

- A running Reacher backend instance
- A modern web browser (no external dependencies, no build step)

## Notes

- Bulk verification adds a 500ms delay between requests to avoid overwhelming the API.
- All processing happens client-side. No data is sent anywhere except to the Reacher API URL you configure.
- If running Reacher locally and opening the HTML file directly (via `file://`), you may need to configure CORS on the backend. Running through a local HTTP server avoids this.
