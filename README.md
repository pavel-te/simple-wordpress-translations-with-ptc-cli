# Simple WordPress Translations with PTC CLI

This repository demonstrates how to use [PTC CLI](https://github.com/OnTheGoSystems/ptc-cli/) to automate the translation process for WordPress plugins and themes.

## 📋 Overview

The project shows a complete workflow for translation processing:
- 📤 Upload `.pot` files to PTC (Private Translation Cloud)
- 🔄 Automatic translation generation
- 📥 Download ready `.po` and `.mo` files
- 🤖 Automation via GitHub Actions

## 🏗️ Project Structure

```
.
├── .github/workflows/ptc.yml    # GitHub Actions workflow
├── .ptc/config.yml              # PTC CLI configuration
├── languages/                   # Translations folder
│   ├── examplewp.pot           # Source translation file
│   ├── examplewp-*.po          # Translation files
│   └── examplewp-*.mo          # Compiled translations
└── README.md                   # This documentation
```

## ⚙️ Configuration

### 1. PTC Configuration (`.ptc/config.yml`)

```yaml
source_locale: en
files:
  - file: languages/examplewp.pot
    output: languages/examplewp-{{lang}}.po
    additional_translation_files:
      - type: mo
        path: languages/examplewp-{{lang}}.mo
      - type: wp
        path: languages/examplewp-{{lang}}.json
      - type: php
        path: languages/examplewp-{{lang}}.l10n.php
```

**Parameter descriptions:**
- `source_locale: en` - source language (English)
- `file` - path to source `.pot` file
- `output` - template for output files ({{lang}} is replaced with language code)
- `additional_translation_files` - additional file formats:
  - `mo` - compiled files for WordPress
  - `wp` - JSON files for JavaScript translations
  - `php` - PHP files for modern WordPress translations

### 2. GitHub Actions (`.github/workflows/ptc.yml`)

The workflow automatically triggers on:
- Push to `main` branch with changes in `config/locales/**/*.yml`
- Manual trigger through GitHub UI

## 🚀 Quick Start

### Step 1: API Token Setup

1. Get your API token from [PTC Dashboard](https://app.ptc.wpml.org/)
2. Add token to repository secrets:
   - Go to `Settings` → `Secrets and variables` → `Actions`
   - Create new secret `PTC_API_TOKEN`
   - Paste your token

### Step 2: Local Usage

```bash
# Clone the repository
git clone <your-repo-url>
cd gh-simple-wordpress-translations-with-ptc-cli

# Download PTC CLI
curl -fsSL https://raw.githubusercontent.com/OnTheGoSystems/ptc-cli/refs/heads/main/ptc-cli.sh -o ptc-cli.sh
chmod +x ptc-cli.sh

# Run translation processing
./ptc-cli.sh --config-file .ptc/config.yml --api-token your-secret-token --verbose
```

### Step 3: GitHub Actions Automation

1. Ensure `PTC_API_TOKEN` is configured in secrets
2. Enable Actions permissions:
   - `Settings` → `Actions` → `General` 
   - `Workflow permissions` → ✅ "Allow GitHub Actions to create and approve pull requests"
3. Run the workflow:
   - Go to `Actions` tab
   - Select "Process Translation Files"
   - Click "Run workflow"

## 📊 Processing Flow

PTC CLI performs the following steps:

1. **📤 Upload** - `examplewp.pot` file is uploaded to PTC
2. **⚙️ Processing** - automatic translation is triggered
3. **👀 Monitoring** - translation status is tracked:
   ```
   🔵 QQ 1/30  → Queued
   🔵 PP 15/30 → Processing
   🟢 CC 28/30 → Completed
   ```
4. **📥 Download** - ready translations are saved to `languages/`

### Changing File Paths

```yaml
files:
  - file: path/to/your.pot
    output: path/to/translations/your-{{lang}}.po
    additional_translation_files:
      - type: mo
        path: path/to/translations/your-{{lang}}.mo
```

### Monitoring Configuration

```bash
# Increase timeout
./ptc-cli.sh --config-file .ptc/config.yml --monitor-max-attempts 60

# Change check interval
./ptc-cli.sh --config-file .ptc/config.yml --monitor-interval 10
```

## 🐛 Troubleshooting

### Common Errors

**HTTP 401 Unauthorized**
```
[ERROR] Failed to upload file: examplewp.pot (HTTP 401)
```
✅ **Solution:** Check your API token is correct

**Files Not Found**
```
[ERROR] No files found for pattern: examplewp.pot
```
✅ **Solution:** Ensure the file exists and path is correct

**Translation Timeout**
```
[WARNING] Timed out files: 1
```
✅ **Solution:** Increase `--monitor-max-attempts` or check status manually

### Debug Mode

For detailed diagnostics use:

```bash
./ptc-cli.sh --config-file .ptc/config.yml --verbose --dry-run
```

## 📝 Logs and Monitoring

PTC CLI provides colored logs:
- 🔵 **INFO** - general information
- 🟢 **SUCCESS** - successful operations  
- 🟡 **WARNING** - warnings
- 🔴 **ERROR** - errors
- 🔵 **DEBUG** - detailed information (only with --verbose)

## 🎯 Use Cases

This setup is perfect for:
- **WordPress Plugin Development** - automate translations for international distribution
- **Theme Development** - provide multilingual support out of the box
- **CI/CD Integration** - automatic translation updates on code changes
- **Team Collaboration** - consistent translation workflow across team members

## 🔄 Workflow Integration

### Manual Trigger
1. Update your `.pot` file with new strings
2. Run the GitHub Action manually
3. Review the generated Pull Request
4. Merge translations

### Automatic Trigger
1. Commit changes to translation source files
2. Push to main branch
3. GitHub Actions automatically processes translations
4. Pull Request is created with updates

## 🔗 Useful Links

- [PTC CLI Repository](https://github.com/OnTheGoSystems/ptc-cli/)
- [PTC Dashboard](https://app.ptc.wpml.org/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)

## 🏆 Best Practices

### Security
- ✅ Store API tokens in GitHub Secrets, never in code
- ✅ Use environment variables for local development
- ✅ Review translation PRs before merging

---

💡 **Tip:** This repository serves as a template. Fork it and adapt it to your needs!