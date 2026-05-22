# AxiTrace for Magento — Hyva theme compatibility module

CSP-compatible Hyva (Tailwind + Alpine.js + Magewire) override layer for the
parent `axitrace/module-tracking` module. Required on stores running the Hyva
theme or Hyva Checkout; not needed for Luma-only stores.

- **Parent module**: [`axitrace/module-tracking`](https://packagist.org/packages/axitrace/module-tracking)
- **Hyva**: 1.x — including the free + open source 2025-11-10 build
- **PHP**: 8.1 / 8.2 / 8.3 / 8.4 (matches parent module support matrix)

## Install

```bash
composer require axitrace/module-tracking axitrace/module-tracking-hyva
bin/magento module:enable AxiTrace_Tracking Hyva_AxiTraceTracking
bin/magento setup:upgrade
bin/magento cache:clean
```

That's it. There is **no separate configuration screen** — the parent module's
admin config (Stores → Configuration → AxiTrace) controls both Luma and Hyva
behaviour. This package only swaps templates.

## What this package does

1. **Registers itself as a Hyva compatibility module** via
   `hyva-themes/magento2-compat-module-fallback` so its
   `view/frontend/templates/pixel.phtml` overrides the parent module's
   `pixel.phtml` whenever Hyva is the active theme.
2. **Uses Alpine.js CSP-build conventions** —
   `x-data="axitracePixel"` (no parens), no inline expression evaluation.
3. **Pairs every inline `<script>` with `$hyvaCsp->registerInlineScript()`** so
   strict CSP environments allow the SDK to load.
4. **Registers a separate block for Hyva Checkout** (`checkout_index_index.xml`)
   because Hyva Checkout has its own CSP pipeline distinct from regular
   storefront pages.

## Hyva Checkout

If you use [Hyva Checkout](https://hyva.io/hyva-checkout/), the purchase event
still fires on the standard Magento success page (`/checkout/onepage/success`)
because Hyva Checkout returns the merchant to that route after order placement.
You do not need to install any additional packages.

## CI: CSP audit

`.github/workflows/ci.yml` includes a custom step that fails the build if any
inline `<script>` in `view/frontend/templates/*.phtml` is not followed by a
`$hyvaCsp->registerInlineScript()` call. Useful preventive guardrail when
extending the module.

## Issues

[github.com/axitrace/axitrace-magento-plugin-hyva/issues](https://github.com/axitrace/axitrace-magento-plugin-hyva/issues)
or email `info@axitrace.com`.

## License

MIT — see [LICENSE.md](./LICENSE.md).
