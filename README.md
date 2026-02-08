# Quant Research Dashboard

Interactive dashboard for quantitative research, factor analysis, and performance monitoring.

## 🚀 Quick Start

### Local Viewing

1. **Generate the dashboard:**
   ```bash
   conda run -n qt python scripts/generate_dashboard.py
   ```

2. **Open in browser:**
   - Main Dashboard: `file:///<path>/reports/dashboard.html`
   - Report Portal: `file:///<path>/reports/index.html`

### Auto-Update

Update dashboard when adding new reports:

```bash
# Update both dashboard and report index
conda run -n qt python scripts/generate_dashboard.py
conda run -n qt python scripts/generate_report_index.py
```

## 📊 Dashboard Modules

### 1. Overview
- Quick statistics: total stocks, factors, reports, data size
- System health snapshot

### 2. Data Status
- Data coverage (date range, stock count)
- Dataset information
- Last update time

### 3. Factor Library
- All registered factors
- Categories and parameters
- Searchable list

### 4. Reports
- All generated analysis reports
- Organized by factor and type (raw/neutralized)
- Click to view full report

### 5. Performance
- Performance metrics comparison (coming soon)
- IC, IR, Sharpe ratios

## 🌐 Deploy to GitHub Pages

### Method 1: Manual Deployment

1. **Enable GitHub Pages:**
   - Go to your repo → Settings → Pages
   - Source: Deploy from a branch
   - Branch: `main` (or `gh-pages`)
   - Folder: `/reports` or `/docs`

2. **Push reports to GitHub:**
   ```bash
   git add reports/dashboard.html reports/index.html reports/*.html
   git commit -m "Update dashboard and reports"
   git push
   ```

3. **Access your dashboard:**
   - URL: `https://<username>.github.io/<repo-name>/dashboard.html`

### Method 2: Automated with GitHub Actions

Create `.github/workflows/deploy-dashboard.yml`:

```yaml
name: Deploy Dashboard

on:
  push:
    branches: [ main ]
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: |
          pip install polars

      - name: Generate dashboard
        run: |
          python scripts/generate_dashboard.py
          python scripts/generate_report_index.py

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: $\{{ secrets.GITHUB_TOKEN }}
          publish_dir: ./reports
          publish_branch: gh-pages
```

### Method 3: Custom Domain (Optional)

1. Add a `CNAME` file to `reports/`:
   ```
   quant.yourdomain.com
   ```

2. Configure DNS with your domain provider:
   - Add CNAME record pointing to `<username>.github.io`

## 📁 File Structure

```
reports/
├── dashboard.html          # Main dashboard (landing page)
├── index.html             # Report browser portal
├── README.md              # This file
├── .nojekyll              # Prevent Jekyll processing
├── f2s_raw_vs_neutralized/   # Factor analysis reports
│   ├── f2s_*_raw_*.html
│   └── f2s_*_neutralized_*.html
└── *.html                 # Other reports
```

## ⚙️ Configuration

### Regenerate on Factor Analysis

Add to your factor analysis workflow:

```python
# At the end of your analysis script
import os
os.system("conda run -n qt python scripts/generate_dashboard.py")
os.system("conda run -n qt python scripts/generate_report_index.py")
```

### Exclude Large Files

If reports are too large, create `.gitignore`:

```
# Ignore large report files
reports/**/*.html
!reports/dashboard.html
!reports/index.html
```

## 🔒 Privacy Considerations

**Important:** Even for private repositories, GitHub Pages sites are **publicly accessible**.

If your reports contain sensitive data:
- ✅ Host on internal server
- ✅ Use password protection
- ✅ Generate static HTML without sensitive metrics
- ❌ Do NOT use GitHub Pages

## 📝 Tips

1. **Bookmark the dashboard URL** for quick access
2. **Set up auto-generation** after factor analysis
3. **Use search features** in factor and report panels
4. **Mobile friendly** - dashboard works on tablets/phones
5. **Share the link** with your team (if not sensitive)

## 🛠️ Troubleshooting

### Dashboard shows 0 factors

The factor registry might not be loading. Ensure:
- Factors are properly registered with `@factor` decorator
- Factor modules are imported somewhere

### Reports not showing

Run the generation script:
```bash
conda run -n qt python scripts/generate_report_index.py
```

### GitHub Pages shows 404

- Check that Pages is enabled in repo settings
- Verify the correct branch and folder are selected
- Wait a few minutes for deployment to complete
- Ensure `dashboard.html` exists in the published directory

## 📚 Further Reading

- [GitHub Pages Documentation](https://docs.github.com/pages)
- [GitHub Actions for Pages](https://github.com/peaceiris/actions-gh-pages)
- [Custom Domains](https://docs.github.com/pages/configuring-a-custom-domain-for-your-github-pages-site)

---

Generated by `scripts/generate_dashboard.py`
# quant-reports
