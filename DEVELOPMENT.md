# Development Setup & Maintenance Guide

## Prerequisites

- Ruby 3.1+ with bundler
- Node.js 18+
- Git
- Python 3.8+ (optional, for content generation scripts)

## Initial Setup

### 1. Install Ruby Dependencies
```bash
bundle install
```

### 2. Install Node Dependencies
```bash
npm install
```

### 3. Set Up Pre-commit Hooks (Optional but Recommended)
```bash
pip install pre-commit
pre-commit install
pre-commit run --all-files  # First run to fix any existing issues
```

## Local Development

### Build and Serve Locally
```bash
bundle exec jekyll serve
# Site will be available at http://localhost:4000
```

### Watch for CSS/JS Changes
In a separate terminal:
```bash
npm run watch
```

### Build Production Assets
```bash
npm run build
bundle exec jekyll build --strict_front_matter
```

## Updating Dependencies

### Update Ruby Gems
```bash
bundle update
```

Check for updates monthly and test locally before pushing to main.

### Update Node Packages
```bash
npm update
npm outdated  # Check for packages that need major version updates
```

## Theme Switching

**Yes, experimenting with different themes is absolutely possible!** Here's how:

### Option 1: Switch Remote Gem Themes (Easiest)

1. Update `_config.yml`:
```yaml
# Before:
theme: minimal-mistakes-jekyll

# After (example with different theme):
remote_theme: pages-themes/architect@v0.2.0
# Or: remote_theme: pages-themes/cayman@v0.2.0
# Or: pages-themes/dinky, minimal, merlot, midnight, slate, tactile, time-machine
```

2. Update Gemfile:
```ruby
# Before:
# gem "minimal-mistakes-jekyll"

# After (if using remote_theme):
# No need for the theme gem when using remote_theme
```

3. Remove/backup custom `_sass/` and `_layouts/` files if they conflict

4. Test locally:
```bash
bundle exec jekyll serve
```

**Popular Jekyll Themes Compatible with GitHub Pages:**
- `pages-themes/architect` - Modern, academic-friendly
- `pages-themes/minimal` - Clean and simple
- `jekyll-theme-primer` - GitHub's official theme
- `jekyll-theme-hydejack` - Modern, feature-rich
- `just-the-docs` - Documentation-focused, great for academic content

### Option 2: Fork a Theme Repository (Advanced)

1. Clone a different Jekyll theme:
```bash
git clone https://github.com/user/jekyll-theme.git temp-theme
cd temp-theme
```

2. Copy theme files to your repo:
```bash
cp -r temp-theme/_layouts .
cp -r temp-theme/_sass .
cp -r temp-theme/assets .
# Merge/customize as needed
```

3. Update `_config.yml` to use local theme:
```yaml
# Don't specify a theme, Jekyll will use local _layouts and _sass
```

4. Test and commit:
```bash
bundle exec jekyll serve
git add .
git commit -m "Switch to new theme"
```

### Option 3: Build Custom Theme

Create your own by modifying `_layouts/` and `_sass/`:

```
_layouts/
  default.html     # Base template
  page.html        # Static pages
  single.html      # Posts/publications
  home.html        # Homepage

_sass/
  main.scss        # Import all partials
  _variables.scss  # Colors, fonts
  _base.scss       # Base styles
  _header.scss     # Navigation
  _footer.scss     # Footer
```

## Troubleshooting

### Build Fails Locally but Works on GitHub

- Delete `Gemfile.lock` and run `bundle install` again
- Ensure you're using Ruby 3.1+ (check: `ruby --version`)
- Run with strict front matter: `bundle exec jekyll build --strict_front_matter`

### Pre-commit Hooks Won't Run

```bash
# Reinstall hooks
pre-commit install --install-hooks

# Run manually to debug
pre-commit run --all-files --verbose
```

### Content Changes Not Appearing

- Restart the Jekyll server (Ctrl+C, then `bundle exec jekyll serve`)
- Clear cache: `rm -rf .jekyll-cache _site`
- Check `_config.yml` is saved (Jekyll doesn't auto-reload config changes)

## Maintenance Checklist

- **Monthly**: Run `bundle update` and `npm update`, test locally
- **Quarterly**: Review GitHub security alerts for dependencies
- **Annually**: Audit all gems and npm packages, remove unused ones

## Continuous Integration

GitHub Actions automatically:
- ✅ Builds your site on every push
- ✅ Checks for broken links
- ✅ Validates Jekyll frontmatter
- ✅ Stores build artifacts for 5 days

Check `.github/workflows/build.yml` for pipeline details.
