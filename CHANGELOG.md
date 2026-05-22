# Changelog

All notable changes to `axitrace/module-tracking-hyva` are documented here.

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
