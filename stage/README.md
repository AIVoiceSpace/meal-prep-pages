# Meal Prep Pages

Automated meal prep blog powered by [reddit-recommend](https://github.com/AIVoiceSpace/reddit-recommend).

## 📖 Overview

This Jekyll site publishes curated meal prep content from Reddit's r/MealPrepSunday community. Posts are automatically generated, staged for preview, and published on a M/W/F/Sat schedule.

## 🏗️ Architecture

```
reddit-recommend          meal-prep-pages
    ↓                            ↓
[detect topics]        [generate blog posts]
    ↓                            ↓
[topic plan JSON] ──────────────→ [_posts/*.md]
                                   ↓
                           [GitHub Actions]
                                   ↓
                         [Preview on staging]
                                   ↓
                           [Merge to main]
                                   ↓
                      [Publish to live site]
```

## 🚀 Quick Start

### Prerequisites

- Ruby 3.0+
- Bundler
- Git

### Installation

```bash
# Clone this repo
git clone https://github.com/AIVoiceSpace/meal-prep-pages.git
cd meal-prep-pages

# Install dependencies
bundle install

# Serve locally
bundle exec jekyll serve
```

Visit `http://localhost:4000` to see the site.

## 📝 Content Generation

### Step 1: Generate Blog Posts

Blog posts are generated in reddit-recommend using detected topics:

```bash
cd ../reddit-recommend
python3 scripts/generate_blog_posts.py --topic-file exports/topic_plans/topics_2025-12-08.json
```

This outputs markdown files to a local directory.

### Step 2: Stage Content

Copy generated posts to this repo's `_posts/` directory:

```bash
# Files should be named: YYYY-MM-DD-slug.md
cp /path/to/generated/*.md _posts/
git add _posts/
git commit -m "chore(posts): stage new content for YYYY-MM-DD"
git push origin staging
```

### Step 3: Preview on Staging

GitHub Actions automatically builds the staging branch and provides a preview URL. Review the rendered output before approving the merge.

### Step 4: Merge to Main

Once approved, merge the PR to `main` to publish live.

## 📅 Publishing Schedule

Posts are scheduled via frontmatter `date` field:

- **Monday**: Publication window
- **Wednesday**: Publication window
- **Friday**: Publication window
- **Saturday**: Publication window

Generate posts one week in advance using these dates.

## 🏷️ Frontmatter Format

All posts must include valid YAML frontmatter:

```yaml
---
layout: single
title: "Your Post Title"
slug: your-post-slug
date: 2025-12-08 08:00:00 -0700
categories:
  - Guides
tags:
  - meal-prep
  - instant-pot
  - batch-cooking
description: "Brief description for SEO and social sharing"
excerpt: "Short excerpt for post lists"
---
```

## 🔗 Placeholder Links

Generated posts use placeholder tokens for affiliate links:

```markdown
Check out [this Instant Pot][INSTANT_POT_LINK] for fast cooking.

[INSTANT_POT_LINK]: /redirects/instant-pot
```

Placeholder mapping is maintained in reddit-recommend and resolved to actual affiliate URLs before publication.

## 📂 Directory Structure

```
meal-prep-pages/
├── _config.yml              # Jekyll configuration
├── _posts/                  # Blog posts (auto-generated)
├── _pages/                  # Static pages
├── _layouts/                # Post/page templates
├── assets/                  # Images, CSS, JS
├── .github/
│   └── workflows/
│       └── jekyll-deploy.yml  # CI/CD workflow
├── Gemfile                  # Ruby dependencies
├── Gemfile.lock             # Locked versions
├── index.md                 # Homepage
└── README.md                # This file
```

## 🔧 Configuration

### GitHub Pages

This site is deployed via GitHub Pages. Settings:

- **Source**: `main` branch
- **Theme**: Minimal Mistakes (via Gemfile)
- **Staging**: Preview via alternate URL (configured in Actions)

### Secrets

GitHub Actions requires these secrets:

- `GITHUB_TOKEN`: Auto-provided by GitHub
- Custom: Add any additional secrets in Repo Settings → Secrets

## 📋 Workflow Files

### `jekyll-deploy.yml`

Validates and builds Jekyll on pull requests:

1. Checkout code
2. Set up Ruby + gems
3. Run `bundle exec jekyll build`
4. Post preview URL as comment on PR

See `.github/workflows/jekyll-deploy.yml` for details.

## 🧪 Local Testing

Before staging content, test locally:

```bash
# Build the site
bundle exec jekyll build

# Serve with live reload
bundle exec jekyll serve --livereload

# Build in production mode
JEKYLL_ENV=production bundle exec jekyll build
```

Check `_site/` folder for rendered output.

## 🐛 Troubleshooting

### Build fails with "Gem not found"

```bash
bundle install
bundle update
```

### Port 4000 already in use

```bash
bundle exec jekyll serve --port 4001
```

### Theme files not loading

Ensure Minimal Mistakes is in your Gemfile and `minimal-mistakes-jekyll` is set in `_config.yml`.

## 📚 Resources

- [Jekyll Documentation](https://jekyllrb.com/)
- [Minimal Mistakes Theme](https://mmistakes.github.io/minimal-mistakes/)
- [GitHub Pages Docs](https://pages.github.com/)
- [reddit-recommend](https://github.com/AIVoiceSpace/reddit-recommend)

## 📄 License

See LICENSE file or contact maintainers.

## 🤝 Contributing

This is an automated pipeline. To modify:

1. Update prompt templates in reddit-recommend
2. Modify blog generator logic in reddit-recommend
3. Submit PR with changes and test data
4. Once approved, next auto-run will use new logic

---

**Powered by [reddit-recommend](https://github.com/AIVoiceSpace/reddit-recommend)**
