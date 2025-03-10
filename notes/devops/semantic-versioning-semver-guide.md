---
icon: code-pull-request-draft
---

# Semantic Versioning (SemVer) Guide

Semantic Versioning is a versioning system that uses `MAJOR.MINOR.PATCH` format:

* **MAJOR**: Breaking changes (not backwards compatible)
* **MINOR**: New features (backwards compatible)
* **PATCH**: Bug fixes (backwards compatible)

***

### 🌟 Semantic Versioning Basics

Semantic Versioning (SemVer) is a versioning convention that helps developers communicate changes in software projects clearly. It uses a three-part version number: `MAJOR.MINOR.PATCH`.

#### Version Components:

* **MAJOR**: Represents breaking changes. Increment this when you make changes that break backward compatibility.
* **MINOR**: Represents backward-compatible new features. Increment this when new features are added in a backward-compatible manner.
* **PATCH**: Represents backward-compatible bug fixes. Increment this for bug fixes that do not change the functionality.

#### Rules for Versioning:

1. **PATCH Version** (e.g., `2.4.1` -> `2.4.2`)
   * 🐛 Bug fixes
   * 🔧 Small changes that don't affect existing functionality
2. **MINOR Version** (e.g., `2.4.1` -> `2.5.0`)
   * 🚀 New features
   * 🎉 Additions to functionality
   * 🔄 Reset PATCH number to 0
3. **MAJOR Version** (e.g., `2.4.1` -> `3.0.0`)
   * 💥 Breaking changes
   * 🔄 API changes that break backward compatibility
   * 🔄 Reset MINOR and PATCH numbers to 0

### 📝 Examples of Version Changes:

* **Initial Release**: `1.0.0` - First stable release of a software.
* **Bug Fix**: `1.0.1` - Minor bug fix, backward-compatible.
* **New Feature**: `1.1.0` - Introduced a new feature, backward-compatible.
* **Breaking Change**: `2.0.0` - Implemented significant changes, not backward-compatible.
* **Post-Breaking Change Fix**: `2.0.1` - Bug fix after a major release.

### 🎯 Implementation and Usage

When planning a software release, determine the nature of your changes:

* **For Bug Fixes**: If you're fixing a bug and no existing functionality is altered, increment the PATCH number.
* **For New Features**: If you add new features or functionality that are backward-compatible, increment the MINOR number.
* **For Breaking Changes**: If your changes break backward compatibility, increment the MAJOR number and reset the MINOR and PATCH numbers to 0.

#### Example Scenario:

Let's say your current version is `1.3.5`:

* A small bug fix would change it to `1.3.6`.
* Adding a new feature would change it to `1.4.0`.
* Releasing a breaking change would change it to `2.0.0`.

***

By following these guidelines, you ensure that users and developers can understand the nature of changes in your software at a glance. Happy versioning!&#x20;
