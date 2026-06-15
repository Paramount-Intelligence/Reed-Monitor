# Reed IT Jobs Monitor

Monitors the [Reed IT Jobs](https://www.reed.co.uk/jobs/it-jobs) listing (sorted by most recent) for new job postings and sends email alerts.

## Features
- **Headless Automation**: Uses Selenium headless Chrome to scrape the public Reed job listing — no login/session required.
- **Structured Extraction**: Parses job cards via Reed's `data-qa` attributes to extract title, company, location, salary, job type, and posting date.
- **Detail Page Enrichment**: Visits each new job's detail page to pull the full job description.
- **De-duplication**: Detects new postings by comparing job IDs (`reed-<id>`) against MongoDB Atlas (shared collection `projects`).
- **Cold Start Reconciliation**: On startup, seeds the DB silently with all currently visible jobs so container restarts never trigger a backlog of emails.
- **Age Filtering**: Skips alerts for jobs older than `MAX_AGE_MINUTES` (parses Reed's relative/absolute posted-date strings).
- **Rich HTML Alerts**: Sends formatted HTML emails via SMTP with job title, company, location, salary, job type, description, and a direct link.
- **Self-Healing**: Automatically catches driver timeouts/crashes and restarts the browser driver to keep the service running 24/7.

## Local Setup

1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Configure `.env` (see existing `.env` for shared MongoDB/SMTP settings).
3. Run the monitor:
   ```bash
   python reed_monitor.py
   ```

## Development and Diagnostic Commands

Run once in debug mode:
```bash
python reed_monitor.py --once --debug
```

Run test mode (skips MongoDB operations, sends 1 test email):
```bash
python reed_monitor.py --once --debug --test
```
