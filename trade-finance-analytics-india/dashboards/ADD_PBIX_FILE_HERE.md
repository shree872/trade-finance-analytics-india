# PLACEHOLDER — Add Your Files Here

## What belongs in this folder:

### `trade_finance_analytics.pbix`
Your main Power BI Desktop file. To add it:
1. Save your Power BI file as `trade_finance_analytics.pbix`
2. Copy it into this `dashboards/` folder
3. Run: `git add dashboards/trade_finance_analytics.pbix`

### Note on .pbix file size
Power BI files can be large (10–50MB+). If GitHub rejects the upload (limit: 100MB),
use Git Large File Storage (LFS):
```
git lfs install
git lfs track "*.pbix"
git add .gitattributes
git add dashboards/trade_finance_analytics.pbix
```

### Alternative: Publish to Power BI Service
1. Open your .pbix in Power BI Desktop
2. Click Home → Publish → Select "My Workspace"
3. Go to app.powerbi.com → find your report → click Share → Get a link
4. Paste the public link in README.md under Dashboard Walkthrough
