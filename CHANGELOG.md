# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- `stationtrail capabilities [--json]` command that emits a stable JSON object
  (`tool`, `version`, `schema`, `sources`, `redaction_profiles`) for MiseLedger
  to detect incompatible binaries.
- `CHANGELOG.md` following Keep a Changelog.

### Changed

- CI now runs the test suite with the race detector (`go test -race ./...`).
- `opencode` usage now documents the supported `--since` flag.

### Fixed

- `opencode` string timestamps are normalized to UTC RFC3339Nano so `--since`
  filtering works against ISO-8601 export times.
- `opencode` reports an actionable message when the external `opencode` binary
  is not on `PATH` instead of an opaque exec error.
- `inspect` bounds object recursion to a fixed depth and records a warning when
  nested keys are truncated.

### Security

- Hardened secret redaction with patterns for AWS access keys
  (`AKIA…`), GitHub tokens (`ghp_`/`gho_`/`ghu_`/`ghs_`/`ghr_`), JSON Web
  Tokens, and PEM private key blocks.
- Tightened the hostname redaction pattern to a known TLD set so ordinary
  dotted text (for example `file.go`, `package.json`, `e.g.`) is no longer
  redacted as a hostname.

## [0.1.4] - 2026-06-03

### Added

- Redaction profiles for the `--redact` flag (`safe`, `none`, and the
  `paths`/`secrets`/`emails`/`urls`/`hostnames`/`all` toggles).

## [0.1.3] - 2026-06-03

### Added

- Multi-source export checks (`all`, `discover`, `doctor`, `inspect`).

## [0.1.2] - 2026-06-03

### Added

- Hermes session snapshot and trajectory scanner support.

## [0.1.1] - 2026-06-03

### Changed

- Release workflow publishes release assets in a single job.

## [0.1.0] - 2026-06-03

### Added

- Initial session exporter scaffold emitting `miseledger.adapter.v1` JSONL.
- OpenCode sanitized-export support and release workflows.

[Unreleased]: https://github.com/escoffier-labs/stationtrail/compare/v0.1.4...HEAD
[0.1.4]: https://github.com/escoffier-labs/stationtrail/compare/v0.1.3...v0.1.4
[0.1.3]: https://github.com/escoffier-labs/stationtrail/compare/v0.1.2...v0.1.3
[0.1.2]: https://github.com/escoffier-labs/stationtrail/compare/v0.1.1...v0.1.2
[0.1.1]: https://github.com/escoffier-labs/stationtrail/compare/v0.1.0...v0.1.1
[0.1.0]: https://github.com/escoffier-labs/stationtrail/releases/tag/v0.1.0
