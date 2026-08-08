# UAE Tax & Reporting Update Monitor

Checks the Federal Tax Authority, the UAE Ministry of Finance and the IFRS
Foundation every weekday for new publications, tags them by topic, and
publishes them to a static dashboard.

**Full setup instructions are in `Tax_Monitor_Setup_Guide.docx`.** This file is
the quick reference.

## Commands

```bash
pip install -r requirements.txt

python monitor.py            # run a check and rebuild the dashboard
python monitor.py --check    # test every source URL, report which are alive
python monitor.py --rebuild  # rebuild the dashboard from stored data only
python monitor.py --digest   # print the digest of new items
python monitor.py --export   # write docs/tax_updates_export.xlsx
```

## Files

| File | Purpose |
|---|---|
| `config.yaml` | Sources, topics and settings. **The only file you need to edit.** |
| `monitor.py` | The program. |
| `data/items.json` | Everything seen so far. Do not delete unless you want to start over. |
| `docs/index.html` | The dashboard. |
| `docs/data.json` | Machine-readable export. |
| `.github/workflows/monitor.yml` | The schedule. Runs 06:00 UTC weekdays. |

## Email digests

Set these as GitHub repository secrets (Settings → Secrets and variables →
Actions). Omit them and the email step is skipped silently.

`SMTP_HOST` · `SMTP_PORT` · `SMTP_USER` · `SMTP_PASS` · `EMAIL_TO`

Use an app password, not your account password.

## Maintenance

Run `python monitor.py --check` monthly, or read the Actions log. Any source
returning zero items has had its page redesigned — update the `url` or
`selector` in `config.yaml`, or set `enabled: false` and rely on the Google
News source for that topic.

## Limitations

- Collects and files; does not read or interpret. Judgment stays with you.
- Cannot reach anything behind the EmaraTax login. Check client notifications manually.
- HTML scrapers break when sites are redesigned; Google News sources do not.
- News items are third-party reports. Verify against the primary text before advising.
