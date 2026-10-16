# Features

One folder per feature: `docs/features/<feature-name>/README.md`, paired with `manifest/features/<feature-name>-package.xml`.

Create both by copying `_TEMPLATE/README.md` to `<feature-name>/README.md` and `manifest/features/_TEMPLATE-package.xml` to `manifest/features/<feature-name>-package.xml`, then add a row below. Feature names are kebab-case and stable for the life of the feature (`intake-form`, `case-routing`).

Both files are living documents: any change to the feature updates the README and the manifest in the same commit. The README is where decisions, discussions, open questions, and context live so that anyone can pick the work up. The manifest is the complete, explicit list of what deploys.

Deploy one feature: `sf project deploy start --manifest manifest/features/<name>-package.xml --target-org <alias>`. Validate first with `--dry-run --test-level RunSpecifiedTests --tests <FeatureTestClass>`.

| Feature | Status | README | Manifest |
|---|---|---|---|
| | | | |
