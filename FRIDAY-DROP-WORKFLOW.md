# Friday Drop Documentation Checklist

Use the storefront repository's
`docs/friday-drops/README.md` as the canonical weekly-release runbook.

For each new documented product:

1. Create `<product-slug>/README.md` and `<product-slug>/installation.md`.
2. Confirm the resource folder name from the actual archive.
3. Confirm compatibility, texture count, polygon count, and clothing component.
4. Do not invent a vMenu item number when other installed EUP resources can
   change it.
5. Add both pages to `SUMMARY.md`.
6. Save optimized promotional WebPs in `.gitbook/assets/`.
7. Verify every relative image and content reference.
8. Confirm the expected public route:
   `https://docs.sonoransoftware.com/store/<product-slug>`.
9. Run `git diff --check`, review the rendered GitBook result when available,
   and commit only the new drop documentation.
