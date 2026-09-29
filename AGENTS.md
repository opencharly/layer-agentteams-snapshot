# AGENTS.md — layer-agentteams-snapshot

Standalone candy repo for the `agentteams-snapshot` layer — the Replicator's
snapshot tool. The candy lives in `charly.yml` at the repo root: it composes the
`charly` toolchain candy (which carries the compiled-in `charly agentteams`
command surface) and adds a `plan:` check that the `snapshot` command is
registered. The snapshot/hydrate cores live in `candy/plugin-agentteams`; this
candy only pre-stages the binary that carries them.

The candy carries **no `skill:` entity**, so no per-repo corpus page is
projected; the owning guidance is the family skill
`/charly-agentteams:agentteams-cli`. The gap is tracked in
`opencharly/opencharly#291`.

Canonical files:

- `charly.yml` — the `agentteams-snapshot:` candy entity (the `candy:`
  composition and the `plan:` check).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-agentteams:agentteams-cli` — the owning (family) skill. The
  compiled-in `charly agentteams` management CLI for the controller REST API.
  Load before editing or troubleshooting the layer.
- `/charly-agentteams:agentteams` — the AgentTeams stack box (the substrate a
  snapshot/hydrate deploy runs on).
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  the `candy:` composition list, `plan:` step verbs incl. `check:`, and service
  declarations). Load before editing any entity field or plan step.
- `/charly-internals:plugin` — the compiled-in command-plugin model (why the
  snapshot core lives in `candy/plugin-agentteams` and not here).

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- Validate the manifest with `charly box validate` at the repo root.
- The candy's `plan:` `check:` is the functional evidence: it probes
  `charly agentteams snapshot --help` at depth 2 (a depth-1
  `charly agentteams --help` is intercepted by a generic out-of-process-plugin
  stub and cannot enumerate subcommands). The `charly`-binary presence check is
  intentionally NOT duplicated here — the composed `charly` candy's own plan
  already bakes it (R3).

## Modify this repo

- Edit the `agentteams-snapshot:` candy entity in `charly.yml`. The candy is a
  thin composition: put snapshot/hydrate behaviour in `candy/plugin-agentteams`
  in the charly repo, not here.
- Keep the `charly` composition — it is what puts the command-carrying binary in
  the venue.
- New behaviour claims belong in the `plan:` as an observable `check:` step.

## Landing

- The authoritative landing mechanics are `/charly-internals:git-workflow` and
  the umbrella `AGENTS.md` in `opencharly/opencharly`; this signpost does not
  restate them.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time).
