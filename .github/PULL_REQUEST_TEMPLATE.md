<!--
Thanks for sending a patch. Keep this short; delete sections that do not apply.
See CONTRIBUTING.md for what lands easily and what needs an issue first.
-->

## What and why

<!-- One or two sentences on the user-visible change and the problem it solves. -->

Closes #

## Type of change

- [ ] Bug fix
- [ ] Format coverage for an existing source
- [ ] Redaction or diagnostic improvement
- [ ] Docs
- [ ] Refactor with no command-surface change
- [ ] Surface change (new source kind, adapter contract, or `--redact` profile) — opened an issue first per CONTRIBUTING.md

## Checklist

- [ ] `go test ./...` passes locally (CI also runs `-race`, `go vet`, and `govulncheck`)
- [ ] Added or updated tests covering the change
- [ ] Updated the `## [Unreleased]` section of `CHANGELOG.md` for any user-visible effect (entries describe effects, not commit subjects)
- [ ] No network calls added; StationTrail stays local-only
- [ ] Diagnostic commands (`discover`, `doctor`, `inspect`, `--dry-run`) still do not print generated transcript text
- [ ] No personal details, hostnames, IPs, account names, tokens, or unredacted absolute paths in code, tests, fixtures, or this PR (use `192.0.2.x` for example IPs)
- [ ] Conventional commit messages, no AI co-authorship trailers
