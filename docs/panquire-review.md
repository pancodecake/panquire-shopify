# Panquire interface review

Scope: the new Panquire homepage, shared navigation/footer, and four empty-page templates. Stack: existing Horizon 4.1.4, native Liquid/JSON sections, scoped CSS, vanilla JavaScript. No existing AGENTS.md, CONTRIBUTING.md, CODING_STANDARDS.md, or CLAUDE.md was found in the workspace inventory. Project design and setup notes were added in this change. Existing merchandise/search/cart templates were not redesigned or visually reviewed.

Used `web-design-engineer` for reference reconstruction, `frontend-design` for design calibration, and `better-interface` with its accessibility, layout, writing, typography, color, and UI owners for source review. Shopify Liquid guidance and the installed Shopify CLI supplied theme checks.

## Coverage

| Domain | Evidence inspected | Result |
| --- | --- | --- |
| Accessibility | Semantic links/buttons, native details menu, Escape behavior, focus rules, image alt handling, decorative placeholders, newsletter labels/errors, reduced-motion branches | Clear in source; keyboard and screen-reader runtime not verified |
| Layout | 1050px container, 1000/700/380px media queries, grid min-width rules, horizontal rails, empty-page height | Clear in source; rendered 320px/200% zoom/RTL not verified |
| Writing | Navigation labels, upcoming-product states, explicit review placeholders, real-product links, newsletter error/success states | Clear; no fabricated pricing, guarantees, testimonials, or review counts |
| Typography | Defined body/display families, heading sizing, body line heights, wrapping, 16px newsletter input, price tabular numerals | Clear in source; actual font metrics and wrapping not verified |
| Colors | Six default declared foreground/background pairs calculated using WCAG relative luminance | All tested pairs pass 4.5:1; uploaded imagery and merchant color changes require a fresh check |
| UI | Manual slider controls, disabled rail ends, active model underline and pressed state, reduced-motion opt-out, empty states | Clear in source; browser interactions not verified |

No actionable interface findings remain in the inspected source scope. The following are explicit verification and deployment gaps, not claims of a successful browser review.

## Verification

- `shopify theme check --output json`: zero errors, six pre-existing warnings in untouched Horizon files. One `ExcessiveSettingsCount` in `sections/header.liquid`, five `UnusedDocParam` warnings in `snippets/divider.liquid`. The first pass identified missing locale entries and a missing discount label; these were corrected, and the second pass reported only those six existing warnings.
- `node --check assets/panquire.js`: passed.
- `git -c core.safecrlf=false diff --check`: passed.
- Inline Node structural check parsed `templates/index.json`, both section groups, and four page templates; resolved every referenced section schema; checked section settings, block types, and block settings: 16 section instances and 38 blocks passed.
- WCAG ratio calculation used linear sRGB (`v / 12.92` below 0.04045, otherwise `((v + 0.055) / 1.055) ** 2.4`), weighted luminance, and `(lighter + 0.05) / (darker + 0.05)`:

| Foreground / background | Ratio |
| --- | --- |
| #ffffff / #211f1e | 16.41:1 |
| #c4c1be / #211f1e | 9.16:1 |
| #211f1e / #f7c343 | 10.04:1 |
| #625e58 / #dedbd6 | 4.66:1 |
| #625e58 / #f4f3f1 | 5.81:1 |
| #c4c1be / #302e2c | 7.55:1 |

## Not verified

- The Shopify skill's `scripts/validate.mjs --theme-path ... --files ... --model GPT-6 --client-name codex --client-version desktop --artifact-id panquire-homepage --revision 1` could not start because its bundled `@shopify/theme-check-common` dependency is absent. The installed Shopify CLI Theme Check was used successfully instead.
- Live reference browser inspection timed out. Four crops of the supplied full-page screenshot were inspected, alongside the existing same-day extracted section inventory. Mobile reference interactions and exact animation timing remain unobserved.
- Automatic approval review rejected `shopify theme dev --store byt11k-dx.myshopify.com --nodelete --host 127.0.0.1 --port 9292` before execution because the store destination was discovered from local CLI configuration, not established by the user. No files were uploaded.
- No rendered theme preview, screenshot comparison, JavaScript console check, overflow measurement, keyboard walkthrough, screen-reader test, native newsletter submission, or real product/cart journey was run.
- Four Page records, template assignments, and optional short URL redirects have not been created or verified on a store. Navigation uses native `/pages/...` destinations pending that setup. `/products` cannot be mapped to the custom blank page by a native Shopify theme.

Verdict: Approve for the reported source review only. Store setup and browser acceptance remain pending; this is not a publishing approval or a pixel-fidelity claim.
