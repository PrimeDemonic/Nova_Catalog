# Nova MCP Catalog

Public manifest + binaries for the **Nova Toolkit**'s MCP Catalog feature. The toolkit GUI fetches `catalog.json` from this repo at session start and once per 24 h to discover available MCP servers, version updates, and tarball download URLs.

This repo is intentionally separate from the (private) toolkit source so consultant machines can fetch catalog state without GitHub credentials.

## Layout

```
catalog.json                                # Live manifest (toolkit fetches this URL)
                                            # https://raw.githubusercontent.com/.../master/catalog.json

Releases (one per upstream MCP version):
  v<mcp-version>/                           # GitHub release tag
    <id>-bundle-<version>.tgz               # Tarball asset
                                            # https://github.com/.../releases/download/v<version>/<filename>
```

## Adding a server

1. Build the tarball using the toolkit's `scripts/build-<id>-catalog-tarball.sh` (or equivalent for non-Playwright MCPs)
2. Compute SHA-256
3. Create a release in this repo with the tarball as an asset
4. Update `catalog.json` on master with the new entry (or version bump on an existing entry)

The toolkit's GUI fetcher honours a 24 h cache TTL, so changes here propagate to consultants on next launch + a daily-tick.

## Contents are public by design

`catalog.json` and the tarball assets must be readable without authentication — the toolkit GUI's fetcher has no GitHub credentials. Don't put anything sensitive here.

## See also

- Toolkit source (private): https://github.com/PrimeDemonic/Nova_Toolkit
- Catalog spec: `docs/specs/mcp-catalog.md` in the toolkit repo
- User-facing docs: `docs/mcp-catalog.md` in the toolkit repo
