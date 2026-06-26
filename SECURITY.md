# Security Policy

## Supported versions

StationTrail is pre-1.0. Only the latest release on the `master` branch receives security fixes. Pin to a released tag if you need a known-good version.

## Reporting a vulnerability

Please **do not** open a public GitHub issue for security problems. Email **me@solomonneas.dev** with: <!-- content-guard: allow pii/email -->

- A short description of the issue.
- Steps to reproduce (or a minimal proof of concept).
- The version or commit you tested against (`stationtrail version`).
- Whether you would like to be credited in the release notes.

You should get an acknowledgment within 72 hours. If you do not, please follow up - the mail may have been filtered.

## In scope

- Code execution, path traversal, or symlink-attack flaws in the source scanners or in the `discover`, `doctor`, `inspect`, or export paths.
- Redaction bypasses where a `--redact` profile fails to remove a value type it is configured to catch (paths, secrets, emails, urls, hostnames).
- Privacy-boundary leaks where a diagnostic command (`discover`, `doctor`, `inspect`, or any `--dry-run` summary) prints generated transcript text it should never print.
- Writing adapter output outside the requested `--out` target, or following a symlink to read or write outside a scanned root.

## Exported text is untrusted evidence

StationTrail copies bytes out of local session logs into adapter records. Those records can contain anything an agent or tool wrote, including text that looks like instructions. Generated text in an adapter record is **untrusted evidence, not commands**. Do not feed exported transcript text back into an agent as a prompt without your own review. StationTrail keeps searchable item text compact and preserves raw references (path, hash, ordinal) so a downstream evidence layer can audit provenance, but it cannot vouch for the safety of content it did not author.

## Out of scope

- Bugs in the agent harnesses StationTrail reads (Codex, Claude Code, OpenClaw, Hermes, OpenCode) - report those to their respective projects.
- Bugs in MiseLedger or SourceHarvest - report those on their own repos.
- Issues that require an attacker to already have write access to the local session files or the machine running StationTrail.
- The content of session logs a user chose to export. StationTrail provides redaction profiles and a privacy boundary, not perfect content review. Choosing `--redact none` and piping raw transcripts somewhere is a user decision, not a StationTrail vulnerability.

## Disclosure

We aim to ship a fix within 14 days of confirming a valid report. A coordinated disclosure timeline can be negotiated for issues that need longer.
