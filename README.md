# layer-agentteams-snapshot

The `agentteams-snapshot` candy — the Replicator's snapshot tool, carried into a
deployment so a Replicator worker can run `charly agentteams snapshot --out
<bundle>` (the PII-redacted hydration bundle) and `charly agentteams apply -f
<bundle>` (hydrate) **in-venue**.

The snapshot/hydrate cores live in the compiled-in `candy/plugin-agentteams` (the
`charly agentteams` CLI surface). This candy is thin by design: it composes the
`charly` toolchain candy so the binary carrying those commands is present, and
adds only a plan check that the `snapshot` command is registered. It is overlaid
onto a bed's deploy via `add_candy:` rather than composed into a production box.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `agentteams-snapshot` |
| Composes | `charly` (the OpenCharly CLI toolchain) |
| Command surface | `charly agentteams snapshot --out <bundle>`, `charly agentteams apply -f <bundle>` |
| Service / port | none |
| Check | `charly agentteams snapshot --help` contains `snapshot` |

## How to use it

Overlay the candy onto a deploy that already runs the AgentTeams controller:

```yaml
replicator-bed:
  pod:
    image: agentteams
    disposable: true
    add_candy: [agentteams-snapshot]
```

Then, in-venue:

```bash
charly agentteams snapshot --out /tmp/bundle
charly agentteams apply -f /tmp/bundle
```

## Layout

- `charly.yml` — the `agentteams-snapshot:` candy entity: the `candy:` composition
  and the `plan:` check.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Family skill: `/charly-agentteams:agentteams-cli` — the `charly agentteams` management CLI.
- The box: `/charly-agentteams:agentteams` — the full AgentTeams stack.
- Toolchain: `/charly-tools:charly` — the `charly` binary the candy composes.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder.
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella.
