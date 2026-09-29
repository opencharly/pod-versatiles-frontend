# AGENTS.md — pod-versatiles-frontend

Standalone candy repo for the `versatiles-frontend` candy — the VersaTiles
frontend SPA served by a permissive-CORS static server on port 8002 (host
28002), re-exporting the style/fonts/styler bundles single-origin. The candy
lives in `charly.yml` at the repo root.

Canonical files:

- `charly.yml` — the `versatiles-frontend:` candy entity (description, `require`,
  `distro`, `port`, `env_provide`, `service`, `plan`) and its `skill:` entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `CHANGELOG/` — per-CalVer history.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-versa:versatiles-frontend` — the owning skill: the layer properties,
  the pre-built tarball rationale, and the bundle layout. Load before editing,
  building, deploying, or troubleshooting this candy.
- `/charly-versa:versatiles-style` — the bundle re-exported under `/style/`.
- `/charly-versa:versatiles` — the tile-server backend the frontend connects to.
- `/charly-versa:pmtiles-viewer`, `/charly-versa:maputnik-layer` — sibling
  static-SPA layers.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `run:` / `write:` / `check:`, service declarations).
- `/charly-check:check` — the check/R10 framework (`charly check box`,
  `charly check run <bed>`).
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate. Its only workflow file is
  `.github/workflows/tag-on-merge.yml`.
- The candy's own `check:` steps assert the unpacked SPA, the CORS server script,
  and the three re-exported bundles, and — at deploy scope — HTTP 200, the
  `Access-Control-Allow-Origin` header, the running service, a reachable port,
  and a fetchable `/style/`.

## Modify this repo

- Edit the `versatiles-frontend:` candy entity in `charly.yml`; the `skill:`
  entity in the same file is the owning skill's source — a candy change and its
  skill change land together.
- The install step unpacks the pre-built `frontend.tar.gz` from the GitHub
  release API (`curl` + `jq`); keep the `distro` build deps in step.
- The `cp -r` step copies the three sibling bundles under the server root;
  keep the `require:` pins and those copy paths in step.
- The CORS server is written by a `write:` step and must keep sending
  `Access-Control-Allow-Origin`; the deploy-scope check asserts the header.
- The `skill:` entity is the source for `/charly-versa:versatiles-frontend`;
  never edit the generated `SKILL.md` — regenerate it.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
