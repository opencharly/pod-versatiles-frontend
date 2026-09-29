# pod-versatiles-frontend

The `versatiles-frontend` candy of the OpenCharly candy library, as a standalone
repo (the candy de-submodule cutover, kind-prefixed naming). It provides the
VersaTiles frontend SPA — a tile-content explorer — served by a permissive-CORS
static server on port 8002 (host 28002).

## What it provides

Unpacks the pre-built versatiles-org/versatiles-frontend TypeScript SPA (the
full-feature `frontend` release variant) at `/opt/versatiles-frontend/` with
`index.html` at the root, and serves it with a custom permissive-CORS
python-stdlib static server on port 8002 (host 28002). The same server re-exports
the sibling layers' bundles under `/style/` (versatiles-style), `/fonts/`
(versatiles-fonts SDF glyphs), and `/styler/` (maplibre-versatiles-styler), so a
notebook's MapLibre iframe loads every asset single-origin without a CDN.

| Property | Value |
|---|---|
| Requires | `layer-supervisord`, `layer-versatiles-style`, `layer-versatiles-fonts`, `layer-maplibre-versatiles-styler` |
| Port | `8002` (host-mapped 28002) |
| Service | `versatiles-frontend` (`python3 …cors-server.py 8002 …`, `restart: always`, priority 38) |
| Env (provided) | `VERSATILES_STYLE_PUBLIC_URL`, `VERSATILES_ASSETS_PUBLIC_URL` |
| Static dist | `/opt/versatiles-frontend/` |

The custom server exists because plain `python -m http.server` sends no
`Access-Control-Allow-Origin`, which would block the cross-origin `/style/`,
`/fonts/`, and `/styler/` fetches.

## How to use it

```yaml
my-image:
  candy:
    - '@github.com/opencharly/pod-versatiles-frontend:<tag>'
```

Then open `http://127.0.0.1:28002/` to explore tile contents or test custom
styles.

## Verification

The candy's `check:` plan asserts the unpacked SPA `index.html`, the CORS server
script, and the three re-exported bundles, and — at deploy scope — HTTP 200 on
`GET /`, the `Access-Control-Allow-Origin` header, the running service, a
reachable port, and a fetchable `/style/`.

## Layout

- `charly.yml` — the `versatiles-frontend:` candy entity (description, `require`,
  `distro`, `port`, `env_provide`, `service`, `plan`) plus its `skill:` entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `CHANGELOG/` — per-CalVer history.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-versa:versatiles-frontend` — the layer properties, the
  pre-built tarball rationale, and the bundle layout.
- `/charly-versa:versatiles` — the tile-server backend the frontend connects to.
- `/charly-versa:pmtiles-viewer`, `/charly-versa:maputnik-layer` — sibling
  static-SPA layers.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
