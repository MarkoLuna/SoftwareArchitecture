# 🏷️ Versioning & Development Workflow

# Table of Contents
1. [Semantic Versioning (SemVer)](#1-semantic-versioning-semver)
2. [Conventional Commits](#2-conventional-commits)

---

## 1. Semantic Versioning (SemVer)

Semantic Versioning is a formal convention for determining version numbers for software releases.

### Version Format
Given a version number **MAJOR.MINOR.PATCH**, increment the:

1. **MAJOR** version when you make incompatible API changes.
2. **MINOR** version when you add functionality in a backwards compatible manner.
3. **PATCH** version when you make backwards compatible bug fixes.

---

## 2. Conventional Commits

Conventional Commits is a lightweight convention on top of commit messages. It provides an easy set of rules for creating an explicit commit history, which makes it easier to write automated tools on top of.

### Message Format
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### Common Types
- **feat**: A new feature (correlates with **MINOR** in SemVer).
- **fix**: A bug fix (correlates with **PATCH** in SemVer).
- **docs**: Documentation only changes.
- **style**: Changes that do not affect the meaning of the code (white-space, formatting, etc).
- **refactor**: A code change that neither fixes a bug nor adds a feature.
- **perf**: A code change that improves performance.
- **test**: Adding missing tests or correcting existing tests.
- **chore**: Changes to the build process or auxiliary tools and libraries.

### Breaking Changes
A breaking change can be indicated by:
1. A `!` after the type/scope (e.g., `feat(api)!: remove deprecated endpoints`).
2. A footer starting with `BREAKING CHANGE:`.
This correlates with **MAJOR** in SemVer.

### Examples
- `fix(ui): correct button alignment`
- `feat(auth): add login with Google`
- `docs: update installation instructions`
- `feat(api)!: change user response format`

---

## Why Combine SemVer and Conventional Commits?
- **Automated Changelogs**: Tools like `standard-version` or `semantic-release` can automatically generate a `CHANGELOG.md` based on your commit history.
- **Automated Versioning**: These tools can also calculate the next version number automatically.
- **Clear History**: It makes it much easier for developers to understand the history and scope of changes in a project.
