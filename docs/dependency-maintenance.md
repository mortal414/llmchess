# Dependency maintenance

The November 2026 cleanup refreshes the UI lockfile within its existing dependency ranges, including Next.js 16.4.5. It keeps the dependency audit enabled.

The scoped override in `ui/package.json` selects minimatch 11.0.1 for `@typescript-eslint/typescript-estree` to address [GHSA-g3hv-75wg-m249](https://github.com/advisories/GHSA-g3hv-75wg-m249). This intentionally crosses that package's declared minimatch 10 range; it is not a routine patch update. The consumer's `minimatch(path, pattern, { dot: true })` call remains supported, representative matching cases were checked, and UI lint, typechecking and build passed. The UI does not currently enable the parser's `allowDefaultProject` option.

Revisit the override when TypeScript-ESLint accepts a patched version directly. Keep the Node 22 CI checks and audit passing before changing it. No `npm audit fix --force` downgrade was applied.
