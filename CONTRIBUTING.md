# Contributing to StationTrail

StationTrail is a local-only scanner and exporter that turns AI agent session logs into portable `miseledger.adapter.v1` JSONL. It is part of the local evidence stack alongside [MiseLedger](https://github.com/escoffier-labs/miseledger) and [SourceHarvest](https://github.com/escoffier-labs/sourceharvest). Patches are welcome. Before you start, please skim this file so we both spend our time on the right things.

## What kinds of changes land easily

- **Bug fixes** in the source scanners (`codex`, `claude`, `openclaw`, `hermes`, `opencode`) or in the `discover`, `doctor`, `inspect`, and export paths.
- **New format coverage** for a source StationTrail already supports, when a real harness version emits a shape the parser does not yet handle.
- **Redaction improvements**: sharper patterns for the existing `--redact` value types, with a test that proves the before-and-after behavior.
- **Diagnostic clarity**: better warnings, counts, or file manifests in `doctor` and `inspect`, as long as they do not print generated transcript text.
- **Test coverage** for any of the above.

## What needs a conversation first

- **A new source kind** (a new harness adapter). Open an issue first describing the harness, where it stores sessions, and the file shapes. Source kinds are part of the `capabilities` contract that MiseLedger reads, so adding or renaming one is a public-surface change.
- **Changes to the adapter contract** (`miseledger.adapter.v1` record shape, the `capabilities` JSON object, or the `--redact` profile names). MiseLedger consumes these directly.
- **Anything that scopes StationTrail beyond agent-session harnesses.** Crawler adapters and general local note or file harvesting belong in SourceHarvest, not here. Archive, storage, search, and evidence bundles belong in MiseLedger.

## What does not land

- Network calls. StationTrail makes none by design, and it must stay that way. No telemetry, no remote fetches, no phone-home.
- Diagnostic commands that print generated transcript text. `discover`, `doctor`, `inspect`, and any `--dry-run` summary report structure, counts, and manifests only.
- Personal details, hostnames, IPs, account IDs, tokens, or unredacted absolute paths in code, tests, or fixtures. Use `192.0.2.x` (RFC 5737) for example IPs. The leak gate will fail if it finds any.
- AI-co-authorship trailers on commits (`Co-Authored-By: <model>`). Conventional commits only.

## Local dev

```bash
git clone https://github.com/escoffier-labs/stationtrail.git
cd stationtrail
go build -o bin/stationtrail ./cmd/stationtrail
go test ./...
```

CI also runs the suite with the race detector and a vulnerability check, so before opening a PR:

```bash
go test -race ./...
go vet ./...
```

To smoke-test an export end-to-end against your own local sources:

```bash
bin/stationtrail capabilities --json
bin/stationtrail discover --json
bin/stationtrail all --dry-run --json
```

`--dry-run` counts files, generated records, and warnings without writing any adapter records, so it is safe to run repeatedly while you work.

## Adding format coverage to an existing source

Source scanners live under `internal/sources/<kind>/`. Each one walks supported files for that source and normalizes them into adapter records. To extend a scanner:

1. Add a fixture under the source's test data that reproduces the new shape.
2. Extend the parser to handle it, preserving raw references (path, hash, ordinal).
3. Add a test that proves the new records normalize correctly and that nothing in the privacy boundary regresses.
4. Update the relevant doc under `docs/` if the user-visible behavior changes.

## Filing issues

Please use the templates under `.github/ISSUE_TEMPLATE/`. They exist to save you from re-typing the version and source shape every time.

The most useful first report is a redacted scan summary. Before posting any output, run with redaction and remove tokens, private hostnames, private repo names, private account names, and unredacted absolute paths:

```bash
stationtrail doctor --json
stationtrail <source> <path> --dry-run --json
```

A misclassified or dropped record is a real bug in a scanner, not a corner case. If a session that should have produced records produced none (or the wrong shape), we want to see it.

## License

By contributing you agree that your contribution is licensed under the MIT License, same as the rest of the repo.
