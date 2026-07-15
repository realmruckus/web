# Web migration audit

Date: 2026-07-15

Result: PASS

Validated:

- required static website files and migration records exist;
- dependency lock contains the expected fixed Core and Classic commits;
- no Core engine, simulator, tests or rule data are present;
- all local HTML references resolve;
- the page and visual asset are served successfully by a local static HTTP server;
- no claim is made that an external deployment platform is configured or tested.

Tracked files at audit time:

```text
.github/workflows/web-migration-audit.yml
README.md
assets/visual-anchor/realm-ruckus-visual-anchor-v1.png
dependencies.lock.json
docs/migration/web-source-audit-2026-07-15.md
index.html
```

SHA-256:

```text
bc3e461ef3f7df77e1882fa5e9ed2a3111b8c9d8d0a99236f2d237f35d68a90c  README.md
95258c2853185535a2bcf69985551b8685a3ca50bfdf341abb349f3de3e0e1f0  index.html
11b7d0fb6e6e346789e016eae3b76fdf28371c480ed82dbd1f137251185862a7  dependencies.lock.json
68d2809d1766cb1894351ca45bd6958449d76b07e6298d09b7d11655bd23dd60  docs/migration/web-source-audit-2026-07-15.md
80da47c116d98b8b112a27011d5a319be662f65a252ca84eb1c0d4bd09bcfc4c  assets/visual-anchor/realm-ruckus-visual-anchor-v1.png
```
