# Shopify Theme Local Development – Quick Start

This guide explains how to set up local development for a Shopify theme, pull the theme from a store, and preview changes locally.

---

# 1. Install Prerequisites

## Install Homebrew (macOS)

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## Install Node Version Manager (recommended)

```bash
brew install nvm
```

Add to your shell config (`~/.zshrc` or `~/.bashrc`):

```bash
export NVM_DIR="$HOME/.nvm"
source "$(brew --prefix nvm)/nvm.sh"
```

Reload your shell:

```bash
source ~/.zshrc
```

## Install Node (LTS)

```bash
nvm install 18
nvm use 18
nvm alias default 18
```

Verify installation:

```bash
node -v
npm -v
```

---

# 2. Install Shopify CLI

Install Shopify CLI and theme tooling globally:

```bash
npm install -g @shopify/cli @shopify/theme
```

Verify installation:

```bash
shopify version
```

---

# 3. Authenticate with Shopify

Run any Shopify CLI command targeting the store. This will trigger browser authentication.

Example:

```bash
shopify theme list --store STORE-NAME.myshopify.com
```

Example for this project:

```bash
shopify theme list --store lubrisynhaproducts.myshopify.com
```

This will open a browser window asking you to authenticate.

---

# 4. List Themes in the Store

```bash
shopify theme list --store STORE.myshopify.com
```

Example output:

```
name                          role          id
Recharge                      [unpublished] #156679307510
Ella-5.0.0-sections-ready     [live]        #125456875720
```

The **theme ID** is needed to pull or push a specific theme.

---

# 5. Pull Theme Locally

Create a project directory:

```bash
mkdir lubrisyn-recharge-theme
cd lubrisyn-recharge-theme
```

Pull the theme from Shopify:

```bash
shopify theme pull \
  --store lubrisynhaproducts.myshopify.com \
  --theme 156679307510
```

This downloads all theme files locally.

---

# 6. Start Local Development

Run the Shopify dev server:

```bash
shopify theme dev \
  --store lubrisynhaproducts.myshopify.com
```

This will:

• Create a temporary development theme  
• Start a file watcher  
• Provide a preview URL  
• Auto-refresh when files change  

Example preview URL:

```
http://127.0.0.1:9292
```

---

# 7. Preview Useful Routes

Sometimes the preview initially loads a gift card page. Navigate to another route manually.

Examples:

```
/
 /collections/all
 /products/product-handle
 /cart
```

---

# 8. Push Local Changes Back to Shopify

When ready to update the theme:

```bash
shopify theme push \
  --store lubrisynhaproducts.myshopify.com \
  --theme 156679307510
```

---

# 9. Avoid Overwriting Theme Editor Changes

If anyone edits the theme through Shopify's Theme Editor, pull before pushing.

```bash
shopify theme pull \
  --store STORE.myshopify.com \
  --theme THEME_ID
```

Important files affected:

```
config/settings_data.json
templates/*.json
```

---

# 10. Creating Custom Sections

Add a new file:

```
sections/custom-section.liquid
```

Example:

```liquid
<div class="custom-callout">
  <h2>{{ section.settings.heading }}</h2>
</div>

{% schema %}
{
  "name": "Custom callout",
  "settings": [
    {
      "type": "text",
      "id": "heading",
      "label": "Heading",
      "default": "Hello world"
    }
  ],
  "presets": [
    {
      "name": "Custom callout"
    }
  ]
}
{% endschema %}
```

The **presets** entry allows the section to appear in **Theme Editor → Add Section**.

---

# 11. Common Troubleshooting

## Authentication Issues

```bash
shopify auth logout
```

Then run another Shopify CLI command to reauthenticate.

---

## Theme Not Appearing in CLI

Possible causes:

• Wrong `.myshopify.com` domain  
• Theme not purchased  
• Missing collaborator permissions  

---

# 12. Git Setup (Recommended)

Initialize a repository:

```bash
git init
git add .
git commit -m "Initial theme pull"
```

Add `.gitignore`:

```
.DS_Store
node_modules
.env
```

---

# Typical Development Workflow

```
shopify theme pull
shopify theme dev
# edit code
shopify theme push
```

---

# Theme Structure Overview

```
assets/       CSS, JS, images
config/       theme settings
layout/       base theme layout
sections/     reusable page sections
snippets/     reusable Liquid fragments
templates/    page templates
locales/      translations
```

---