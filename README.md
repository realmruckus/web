# Realm Ruckus Web

This repository is the standalone website and deployment boundary for Realm Ruckus.

## Current contents

```text
index.html
assets/visual-anchor/realm-ruckus-visual-anchor-v1.png
dependencies.lock.json
docs/migration/
```

The migrated source is a static landing-page snapshot. The reviewed source did not contain a package manifest, build system, deployment configuration, web test suite or automatic content synchronization program. Those capabilities must not be described as already migrated.

## Authority

Web owns:

- website frontend and website-specific static assets;
- SEO and deployment configuration when present;
- website tests and content synchronization tools when present;
- the explicit dependency lock used to select Core and Classic content versions.

Web does not own:

- shared rules, mechanism data, engine, AI, simulation or baselines;
- product-wide identity metadata;
- reusable physical-production specifications or tools;
- the authoritative Realm Ruckus theme document and asset library.

Product name, publisher, copyright owner, registered country, trademark output and official website are sourced from the pinned Core version of:

```text
docs/product-metadata.md
```

The current static footer renders the approved full copyright output from that pinned metadata. Any metadata change requires an explicit Core dependency-lock update and a Web commit; Core changes do not deploy this repository automatically.

## Dependency and deployment rule

`dependencies.lock.json` fixes the Core and Classic commits used by this website snapshot. Web must not follow another repository's `main` branch as a long-term dependency.

Deployment platforms may bind only to this repository. Commits in Core, Production or Classic must not automatically deploy the website. An upgrade requires an explicit dependency-lock update and a Web commit.

Current migration source: `anxiannet/realmruckus@1773d97d2da2c6bdc467bf5f3a97eefe7965d386`.
