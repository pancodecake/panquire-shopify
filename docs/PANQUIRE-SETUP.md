# Panquire theme setup

The homepage is implemented in native Shopify Online Store 2.0 Liquid. It uses the existing Horizon theme; no React app, external UI library, app installation, or new base theme is required. Nothing has been uploaded or published.

## Preview

After confirming the intended store, run:

```powershell
shopify theme dev --store YOUR-STORE.myshopify.com --nodelete
```

This creates an unpublished development theme. Do not use `--allow-live`. The CLI currently remembers `byt11k-dx.myshopify.com`, but ownership and upload authorization have not been confirmed. Automatic approval review blocked the attempted development-preview upload before execution.

## Empty pages and navigation

Theme files cannot create Shopify Page records or assign arbitrary root-level routes. Create four pages with empty content, then assign these templates (after the theme is available in the store):

| Navigation label | Page handle | Template | Native destination |
| --- | --- | --- | --- |
| Front Page | — | index | / |
| Products | products | page.products | /pages/products |
| About Us | about | page.about | /pages/about |
| Partner With Us | partners | page.partners | /pages/partners |
| Terms of Service | terms | page.terms | /pages/terms |

The page bodies are empty; the shared header/footer remain. A visually hidden page heading supplies accessible context. No legal copy is invented. Until the four Page records exist, their URLs will return 404. Selecting a page template does not create the page.

The requested `/about`, `/partners`, and `/terms` can be Shopify URL redirects to their native destinations. An optional import file is `docs/panquire-redirects.csv`; import only after the target pages exist and after checking for existing redirects. The navigation settings can then be changed to those aliases if desired. Redirects change the final browser URL to `/pages/...`.

`/products` is a reserved Shopify route and cannot be redirected to a custom empty page. This implementation uses `/pages/products` to satisfy the empty-page requirement. Keeping the exact root-level `/products` address as a custom blank page is not supported by a native Liquid theme. See [Shopify's URL redirect restrictions](https://help.shopify.com/en/manual/online-store/menus-and-links/url-redirect).

Header and footer share the same five navigation entries. Change their destinations in **Theme settings → Panquire storefront**. Hero and editorial call-to-action destinations are separately editable in each section.

## Edit content and images

- Open the homepage in the theme editor. Every major content area is a separate **Panquire** section. Reorder or remove sections as needed.
- Hero slides, service items, featured models, product cards, editorial tiles, reviews, and gallery images use repeatable blocks.
- Select an image in a section/block image picker to replace that placeholder. Clearing the picker restores the neutral placeholder.
- In **Panquire product cards**, choose a collection or select products on individual card blocks. A populated collection takes precedence. An empty collection falls back to the individual blocks.
- Product title, price, discounts, and product links use the selected Shopify product. Placeholder cards do not simulate purchases.
- Real catalog photography remains off until **Theme settings → Panquire storefront → Enable real product images on Panquire homepage** is enabled. An explicitly chosen custom image overrides that default for its block.
- Review blocks stay explicitly labeled as placeholders until genuine reviewer names and text are entered. No ratings or fabricated customer counts are included.
- Journal tiles are editable editorial blocks, not a live blog feed. Set a destination only once that article/page exists.
- The newsletter uses Shopify's native customer form with the `newsletter` tag, email validation, and success/error messages. End-to-end submission still needs a store-backed check.
- Functional labels are in the `panquire` locale namespace. All existing storefront locale files include an English fallback for these new strings; these are not claimed as completed translations. Merchant-editable section text can be translated in Shopify.
- Check text contrast after changing colors or selecting imagery, particularly hero text.

## Files

- `sections/pq-*.liquid`: 10 editable sections, including header/footer and blank-page body.
- `snippets/pq-*.liquid`: shared navigation, media, product card, and icons.
- `assets/panquire.css` and `assets/panquire.js`: scoped styling and progressive interaction behavior.
- `templates/index.json`: reference section ordering and editable sample content.
- `templates/page.{products,about,partners,terms}.json`: empty page templates.
- `sections/header-group.json`, `sections/footer-group.json`: use the new global chrome.
- `layout/theme.liquid`, `config/settings_schema.json`, and storefront locale files: asset loading, settings, and labels.

## Validation and remaining checks

See `docs/panquire-review.md`. The live theme, store pages, redirects, products, checkout, and customer data have not been changed.
