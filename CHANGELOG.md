# Changelog

All notable changes to this project are documented in this file.

---

## [0.2.0] - 2026-01-07

### Added
- Full **machine-readable audit output** via `--json-audit`
- Official **JSON Schema** (`fsync-report.schema.json`) defining the audit contract
- Deterministic separation between:
  - filesystem diff (state)
  - execution audit (effects)
- Comprehensive **test suite**:
  - unit tests for core diff logic
  - black-box tests for CLI behavior
  - JSON audit validation against schema
- CI-ready architecture with explicit exit codes

### Changed
- Core refactored to **remove global state**
- Audit data is now **explicitly returned and managed by the CLI**
- Unreadable files are no longer treated as “updated” during hash comparison
- `--audit-only` now guarantees **no human-oriented output**
- Exit codes are now consistently derived from diff + audit results
- README significantly expanded and clarified:
  - philosophy
  - safety guarantees
  - OS support
  - real-world use cases

### Clarified
- `--json` exports **diff-only** output
- `--json-audit` is the **official machine interface**
- `mtime` remains the default comparison strategy
- Hashing is **opt-in by design**, not required

### Notes
- This release introduces a **stable audit contract**
- `--json` is considered legacy and may be deprecated in `v1.0.0`
- No backward-incompatible changes to core CLI usage

---

## [0.1.1] - 2026-01-05

### Changed
- Updated README to English
- Improved package description on PyPI

---

## [0.1.0] - 2026-01-05

### Added
- Conscious filesystem diff and sync tool
- `diff`, `sync`, and `check` modes
- Dry-run with full audit logic
- Hash-based comparison (opt-in)
- Strict filesystem mode (`--strict-fs`)
- Exit codes inspired by `rsync`
- CLI command `fsync`

### Philosophy
- Explicit diff before sync
- No silent failures
- No automatic renaming or fixing
