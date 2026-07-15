# Realm Ruckus Web

This repository is the standalone website and deployment boundary for Realm Ruckus.

## Current contents

```text
index.html
assets/visual-anchor/realm-ruckus-visual-anchor-v1.png
dependencies.lock.json
docs/migration/
```

The migrated source is a static landing-page snapshot. The reviewed source did not contain a package manifest, build system, deployment configuration, web test suite or content synchronization program. Those capabilities must not be described as already migrated.

## Authority

Web owns:

- website frontend and website-specific static assets;
- SEO and deployment configuration when present;
- website tests and content synchronization tools when present;
- the explicit dependency lock used to select Core and Classic content versions.

Web does not own:

- shared rules, mechanism data, engine, AI, simulation or baselines;
- reusable physical-production specifications or tools;
- the authoritative Realm Ruckus theme document and asset library.

## Dependency and deployment rule

`dependencies.lock.json` fixes the Core and Classic commits used by this website snapshot. Web must not follow another repository's `main` branch as a long-term dependency.

Deployment platforms may bind only to this repository. Commits in Core, Production or Classic must not automatically deploy the website. An upgrade requires an explicit dependency-lock update and a Web commit.

Current migration source: `anxiannet/realmruckus@1773d97d2da2c6bdc467bf5f3a97eefe7965d386`.
