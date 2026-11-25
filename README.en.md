# RUNUP · Shared core

[中文](README.md) · English

`@paobu/core` is the shared core repository for the RunUp main site, knowledge site and tools site: design tokens, base layouts, navigation, header and footer, theme, language switching, routing helpers, SEO/JSON-LD, PWA/offline shell, content contracts and release helpers.

It contains no product pages, no Cesium globe, no route geometry rendering and no tool calculation implementation. Each site builds independently and consumes this repository as a versioned tgz artifact.

## Scope

Lives here:

- design tokens, fonts, colours, spacing and base styles
- shared layouts, navigation, header/footer, theme and language switching
- i18n, route prefixes, SEO/JSON-LD generation
- PWA/offline shell, release manifest and hosting-file generation
- shared buttons, cards, tables, search shell and content validation contracts
- content loading, digest, review and source verification types
- geodesic distance and track simplification helpers

Does not live here:

- Cesium globe, 3D scenes or satellite textures
- route detail templates, route geometry rendering or race content
- tool calculator interaction or concrete calculation code
- any site home page, section page or search business logic

## Install and pack

```bash
pnpm install
node scripts/pack-core.mjs
```

`pack-core.mjs` produces:

```text
core-dist/paobu-core-0.3.3.tgz
core-dist/paobu-core-0.3.3.tgz.sha256
core-dist/core.lock.json
```

A plain `pnpm pack` only produces the npm tarball; it does not write `core-dist`'s `.sha256` and `core.lock.json`. Generate the publishable artifact with `node scripts/pack-core.mjs`. Each site reads the version and SHA-256 from `core-dist/core.lock.json` and pins the same artifact.

## Release

1. Change the version in `package.json`.
2. Run `node scripts/pack-core.mjs`.
3. Tag `core-v0.3.3` and publish a GitHub Release with the tgz, sha256 and `core.lock.json`. The workflow lives in `.github/workflows/release.yml`.
4. In each site, run `node scripts/sync-core.mjs` to update its `core.lock.json`, then reinstall and rebuild.
5. Check that the release bundle records the same `version` and `sha256` for all three sites.

A GitHub Release is not required locally: the sites copy the artifact from `../paobu-core/core-dist` through `scripts/sync-core.mjs`.

## Compatibility

- The package name stays `@paobu/core`.
- Public exports stay stable; removing or renaming an export requires a minor/major version bump.
- Sites consume only `@paobu/core/...` exports and never copy `src/`.
- Product pages do not belong in this repository.

## Licence

MIT. See [LICENSE](LICENSE).

