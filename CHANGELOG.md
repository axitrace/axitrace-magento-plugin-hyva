# Changelog

All notable changes to `axitrace/module-tracking-hyva` are documented here.

## [0.1.1] - 2026-05-24

### Fixed
- Storefront pixel (`view/frontend/templates/pixel.phtml`) now explicitly calls `window.Axitrace.init(...)` after the SDK loads and dispatches the appropriate event based on the page type. Previously the SDK was loaded into the DOM but never initialized, so no client-side events (page view, product view, add-to-cart) were ever sent. Mirrors the fix shipped in `axitrace/module-tracking` v0.1.2 for the Luma theme, adapted to Hyva's Alpine.js + CSP-strict environment.
- Page-type → SDK event mapping: `home`/`cart`/`cms` → `page.view`; `product` → `product.view` (with `params.sku` at top level as required by ingestion); `category` → `page.view` with category metadata; add-to-cart click on Hyva tocart buttons → `product.addToCart`.
- Sets `window.axitrace` to the initialized tracker and dispatches an `axitrace:ready` window event so the existing order-confirmation template can hook in without race conditions.

## [0.1.0] - 2026-05-22

### Added
- Initial release. Provides Hyva theme (Tailwind + Alpine.js + Magewire) templates that override the Luma pixel and order-confirmation phtml files from `axitrace/module-tracking`.
- Strict CSP compatibility via `$hyvaCsp->registerInlineScript()` after every `<script>` block.
- Alpine CSP-build compatible `x-data` usage (no inline expression calls).
- Separate Hyva Checkout context registration (`checkout_index_index.xml`).

### Compatibility
- Magento Open Source 2.4.6 / 2.4.7 / 2.4.8 + Adobe Commerce.
- PHP 8.1 / 8.2 / 8.3 / 8.4 (per Magento version matrix).
- Hyva Theme 1.x (with free + open source 2025-11-10 build) and Hyva Checkout.
- Requires `axitrace/module-tracking` ^0.1.0.
