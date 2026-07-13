# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

History is tracked from `1.3.0` onward.

## [Unreleased]

### Deprecated

- **`org.json:json`** is a candidate for removal from the managed catalog. The
  [JSON License](https://www.json.org/license.html) ("The Software shall be used for Good, not Evil") is
  **not OSI-approved** and is problematic for downstream distribution and compliance. Proposed replacement:
  [`jakarta.json`](https://jakarta.ee/specifications/jsonp/) for the standard API, or the already-managed
  `com.google.code.gson:gson`. **No change applied yet** — this is a proposal pending review; consumers
  should start migrating off `org.json`.

## [1.3.0]

_Release in preparation on branch `release/1.3.0` (currently `1.3.0-SNAPSHOT`)._

### Fixed

- `spring-data-bom` was declared as a plain dependency; it now imports correctly with
  `<type>pom</type>` + `<scope>import</scope>`.
- `jackson-bom` now imports with `<scope>import</scope>`.

### Added

- Enforcer rules `banDuplicatePomDependencyVersions` and `requireUpperBoundDeps`, making the BOM the
  convergence authority for consumers that use it as a `<parent>`.
- Opt-in `strict-convergence` profile with `dependencyConvergence` (`-Pstrict-convergence`).
- `analyze` profile completed: JaCoCo line-coverage gate (`jacoco.line.minimum`, `jacoco.halt`),
  dependency-check `nvdApiKey`/`failBuildOnCVSS=7`/parametrized suppression file, and Checkstyle wired to
  the shared `scos-build-config` artifact.
- OSS governance: `CHANGELOG.md`, `CONTRIBUTING.md`, `SECURITY.md`, and `renovate.json`.

### Changed

- Import order in `dependencyManagement`: `spring-data-bom` and `jackson-bom` now precede
  `spring-boot-dependencies` so their pinned versions win.
- POM cleanup: removed placeholder comments; deduplicated enforcer/compiler between `pluginManagement` and
  `build/plugins`; replaced `maven.compiler.source`/`target` with a single `maven.compiler.release=25`;
  renamed properties to kebab-case (`op_querydsl.version` → `querydsl.version`,
  `org.mapstruct.version` → `mapstruct.version`).
- GPG signing mechanics (passphrase, loopback pinentry) moved from `pom.xml` to the
  `.github/actions/setup-gpg` composite action; the POM keeps only the `sign` execution.
- `README.md` rewritten to remove hardcoded version tables (POM is the source of truth) and to fix
  coordinates/badges (`br.com.sawcunhaos`, org `SawCunhaOS`).
- `build.yml` reworked to match a BOM: only `validate`, `verify` (enforcer) and
  `-Panalyze dependency-check:check`.

### Removed

- Dead `nexus-staging-maven-plugin` from `pluginManagement` (release uses
  `central-publishing-maven-plugin`).
- CI steps that referenced non-existent modules (`privacy`, `utils`, `exception`, `audit`, `jdempotent`,
  `security`) and the `spotbugs:check` step (no code / plugin in the BOM).
- Privileged `PAT_TOKEN` from the PR build checkout.

[Unreleased]: https://github.com/SawCunhaOS/sawcunha-open-system-bom/compare/1.3.0...HEAD
[1.3.0]: https://github.com/SawCunhaOS/sawcunha-open-system-bom/releases/tag/1.3.0
