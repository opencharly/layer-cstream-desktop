# AGENTS.md — layer-cstream-desktop

Standalone candy repo for the `cstream-desktop` metalayer — the streamed
Hyprland desktop composed from `pod-cstream` (the Wayland parent) and
`pod-hyprland` (the nested compositor). The entire candy lives in `charly.yml`
at the repo root: the two member `candy:` refs and the single composition
`check:`. It carries no install content and declares **no `skill:` entity**; the
closest family skill is `/charly-distros:omarchy-cstream` (the gap is tracked in
`opencharly/opencharly#291`).

Canonical files:

- `charly.yml` — the `cstream-desktop:` candy entity (no `skill:` entity).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-distros:omarchy-cstream` — the closest family skill (the streamed
  Omarchy desktop that consumes this transport). Load before editing or
  troubleshooting the metalayer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, the composition `candy:` field, service
  declarations). Load before editing any entity field or plan step.
- **Missing owning skill:** this candy has no `skill:` entity of its own, so no
  page is projected from this repo. The gap is recorded against the named batch
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The metalayer's `plan:` `check:` is the functional evidence: after composition,
  one binary from each member must exist. It must stay valid on every distro arm
  it runs on.

## Modify this repo

- There is no `skill:` entity here to edit; the members own their own behaviour,
  so this repo carries only the composition order and its assertion.
- Keep the member refs pinned (`@github.com/opencharly/pod-*:v…`) — a bare name
  resolves only inside a project that happens to have the member in scan range.
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
