# AGENTS.md — layer-omarchy-themes

Standalone candy repo for the Omarchy themes layer — selects one of the 22
bundled themes and renders it into every app config it drives. The repo is
multi-candy-shaped: the root `charly.yml` carries only the repo shape
(`discover:`), and the member candy lives in
`candy/omarchy-themes/charly.yml`.

This repo has **no `skill:` entity** in its candy manifest, so there is no
dedicated owning skill projected into the marketplace corpus. The gap is recorded
against `opencharly/opencharly#291` (the batch that authors missing `skill:`
entities).

Canonical files:

- `charly.yml` — the repo shape (`repo:` + `discover:`).
- `candy/omarchy-themes/charly.yml` — the candy entity: the `require:` on the
  foundation layer, the `OMARCHY_THEME` var/env_accept, and the `plan:`
  `run:`/`check:` steps.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-distros:omarchy-base` — the family foundation skill: the package
  sources, the pinned mirror snapshot, and the runtime this layer builds on.
  Load before editing or troubleshooting.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, per-distro `distro:` arms, package/repo
  sections, and service declarations). Load before editing any entity field or
  plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` steps are the functional evidence — the `run:` step bakes
  the theme and verifies request-vs-recorded, and the `check:` steps assert the
  applied theme content matches the bundled theme, the current-theme directory
  resolves, the full theme set is present, and `omarchy-theme-set` is on PATH.

## Modify this repo

- There is no `skill:` entity to keep in sync; if one is added (per #291), it
  must be edited together with the candy entity in the same change.
- Keep `OMARCHY_THEME`'s **request-vs-recorded** comparison in the `run:` step,
  not a `check:` step: the var is a candy var the step compiler expands, while
  the check runner has no candy vars and the check would silently SKIP with
  "unresolved variables".
- Keep `THEME_HEADLESS=1` and the `XDG_RUNTIME_DIR`/`GSETTINGS_BACKEND`
  workarounds — they are what let `omarchy-theme-set` run in a build container.
- New behaviour claims belong in the `plan:` as an observable `check:` step.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
