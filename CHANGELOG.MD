# Changelog

## [0.1.1] - 2026-01-05
### Changed
- Updated README to English
- Improved package description on PyPI

## [0.1.0] - 2026-01-05

### Added
- Conscious filesystem diff and sync tool
- `diff`, `sync` and `check` modes
- Dry-run with full audit report
- Hash-based comparison (opt-in)
- Strict filesystem mode (`--strict-fs`)
- Exit codes inspired by rsync
- CLI command `fsync`

### Philosophy
- Explicit diff before sync
- No silent failures
- No automatic renaming or fixing
