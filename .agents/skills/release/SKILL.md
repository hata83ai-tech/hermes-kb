---
name: release
description: "Use when the user asks to create a release, tag version, or publish a package."
license: MIT
metadata:
  author: hermes-kb
  version: "1.0"
  source: https://github.com/heyitsnoah/claudesidian
---

# Release Workflow

Systematic approach to creating and publishing releases.

## Release Checklist

### Pre-Release
- [ ] All features complete
- [ ] Tests pass on CI
- [ ] Changelog updated
- [ ] Version bumped

### Release Day
- [ ] Create release branch
- [ ] Run final checks
- [ ] Create GitHub release
- [ ] Publish to package manager

## Version Numbering (SemVer)

- **MAJOR** - Breaking changes
- **MINOR** - New features, backward compatible
- **PATCH** - Bug fixes

## Changelog Format

```markdown
## [Version] - YYYY-MM-DD

### Added
- Feature A

### Changed
- Improvement to X

### Fixed
- Bug in Y
```

## Git Tagging

```bash
git tag -a v1.0.0 -m "Release v1.0.0"
git push origin v1.0.0
```
