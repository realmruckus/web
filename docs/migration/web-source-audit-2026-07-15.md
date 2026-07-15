# Web source audit

Date: 2026-07-15

## Source reviewed

- `anxiannet/realmruckus@1773d97d2da2c6bdc467bf5f3a97eefe7965d386`
- migrated snapshot in `realmruckus/web`

## Confirmed Web content

- `index.html`
- `assets/visual-anchor/realm-ruckus-visual-anchor-v1.png`

The source snapshot contains a single static landing page and its referenced visual asset. No package manifest, build system, deployment configuration, web tests, content synchronization program or additional website source tree was confirmed in the reviewed snapshot.

## Excluded from Web

- Core rules, mechanism data, engine, simulation and tests;
- reusable production specifications and tools;
- Classic theme authority documents and source asset library beyond the website-specific copied asset;
- Qiangdipan theme content.

## Boundary decision

Web is the only deployment repository. Core and Classic changes do not deploy Web automatically. Web adopts new content only through an explicit update of `dependencies.lock.json` followed by a Web commit.

This audit records the actual source state only. It does not claim that a deployment platform is currently connected or that a production deployment has been tested.