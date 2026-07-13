# SCOS BOM (SawCunha Open System)

[![Build & Test](https://github.com/SawCunhaOS/sawcunha-open-system-bom/actions/workflows/build.yml/badge.svg)](https://github.com/SawCunhaOS/sawcunha-open-system-bom/actions/workflows/build.yml)
[![Maven Central](https://img.shields.io/maven-central/v/br.com.sawcunhaos/scos-bom.svg?label=Maven%20Central)](https://central.sonatype.com/artifact/br.com.sawcunhaos/scos-bom)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Java 25+](https://img.shields.io/badge/Java-25%2B-orange.svg)](https://www.java.com)
[![Maven 3.9.12+](https://img.shields.io/badge/Maven-3.9.12%2B-blue.svg)](https://maven.apache.org/)

Official Bill of Materials (BOM) that standardizes and aligns dependency versions across all SCOS projects.

- **Coordinates:** `br.com.sawcunhaos:scos-bom`
- **Packaging:** `pom` (no sources — this repository ships only dependency and build governance)
- **Organization:** [SawCunhaOS](https://github.com/SawCunhaOS)

---

## 📦 Overview

The **SCOS BOM** provides centralized dependency management and build conventions for all projects within
the SawCunha Open System ecosystem.

By adopting this BOM, applications and libraries can:

* Use consistent, tested dependency versions across all modules
* Reduce version conflicts (dependency hell)
* Simplify dependency upgrades and maintenance
* Inherit SCOS build conventions (enforcer, compiler, analysis) when used as a `<parent>`

This project follows semantic versioning and is designed for production use.

---

## 🔖 Source of truth for versions

**This README does not list managed versions.** Hardcoded version tables drift out of sync with the POM
and become misleading. The single source of truth is [`pom.xml`](pom.xml).

To see exactly what a given release manages, resolve the effective POM:

```bash
# From a checkout of this repo (or a consumer project):
mvn help:effective-pom

# A single managed version, e.g. Spring Boot:
mvn help:evaluate -Dexpression=spring-boot-dependencies.version -q -DforceStdout
```

The managed versions are declared as `*.version` properties at the top of [`pom.xml`](pom.xml) and are
kept up to date automatically by Renovate (see [`renovate.json`](renovate.json)).

> Optional: a release step could regenerate a version table from `mvn help:evaluate` per property. If that
> is ever added to `scripts/release.sh`, document it here so the table has a clear, automated origin.

---

## 🚀 Usage

There are **two distinct ways** to consume this BOM. They are not equivalent.

### A) As a `<parent>` — versions **and** build conventions

Use this when your project is a first-class SCOS module and wants the SCOS build baseline: the enforcer
rules (Maven/Java floor, duplicate/upper-bound checks), the compiler configuration (`release`, Lombok +
MapStruct processors), and the `analyze` profile (JaCoCo, dependency-check, Checkstyle).

```xml
<parent>
  <groupId>br.com.sawcunhaos</groupId>
  <artifactId>scos-bom</artifactId>
  <version>1.2.0</version> <!-- última estável; confira o badge do Maven Central -->
  <relativePath/>
</parent>
```

### B) Via `<scope>import</scope>` — versions **only**

Use this when you only want aligned dependency versions and want to keep full control of your own build.
**Importing a BOM brings only `dependencyManagement`. It does NOT bring plugins, plugin configuration, or
profiles** — so the enforcer, compiler and `analyze` conventions are *not* inherited this way.

```xml
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>br.com.sawcunhaos</groupId>
      <artifactId>scos-bom</artifactId>
      <version>1.2.0</version> <!-- última estável; confira o badge do Maven Central -->
      <type>pom</type>
      <scope>import</scope>
    </dependency>
  </dependencies>
</dependencyManagement>
```

Then declare dependencies **without versions** (they are managed by the BOM):

```xml
<dependencies>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
  <dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
  </dependency>
</dependencies>
```

### Gradle (import only)

```kotlin
dependencyManagement {
    imports {
        mavenBom("br.com.sawcunhaos:scos-bom:1.2.0")
    }
}
```

---

## 🔁 Coordinate migration

The published coordinates are **`br.com.sawcunhaos:scos-bom`**.

If you previously referenced the old coordinates `io.github.sawcunha:scos-bom` (or the org `sawcunha`
instead of `SawCunhaOS`), update your `<groupId>` to `br.com.sawcunhaos`. The artifactId (`scos-bom`) is
unchanged. Older artifacts under the previous groupId, if any exist on Central, are not maintained.

---

## 🧭 Branching & release model

This is why a `release/x.y.z` branch carries an `x.y.z-**SNAPSHOT**` version: the branch holds
work-in-progress toward that release, and the concrete release version is only stamped at close time.

```
develop ──────────────────────────────────────────────►  (next major, x.0.0-SNAPSHOT)
   │
   └── release/1.3.0  ── carries 1.3.0-SNAPSHOT ──► close (scripts/release.sh)
                                                      │
                                                      ├─ strips -SNAPSHOT → 1.3.0
                                                      ├─ git tag 1.3.0
                                                      ├─ deploy to Maven Central
                                                      └─ merge-back to develop, open next
```

- **`develop`** — integration branch; always a SNAPSHOT of the next line.
- **`release/x.y.z`** — a minor being stabilized; SNAPSHOT until closed.
- **`fix/x.y.z`** — a patch line branched from its release.
- **Closing** is done by [`scripts/release.sh`](scripts/release.sh) (`major|minor|fix`): it removes the
  `-SNAPSHOT`, tags, deploys, merges back, and opens the next SNAPSHOT.

Snapshots are published continuously by [`publish-snapshot.yml`](.github/workflows/publish-snapshot.yml)
after a green build on `develop`, `release/*` and `fix/*`.

---

## 🧰 Enforcer

The BOM acts as the convergence authority for projects that use it as a `<parent>`. The default
`enforce-maven` execution runs on every `verify`:

- `requireMavenVersion` — Maven 3.9.12+
- `requireJavaVersion` — Java 25+
- `banDuplicatePomDependencyVersions` — no duplicated managed versions
- `requireUpperBoundDeps` — a transitive is never silently downgraded

A stricter, **opt-in** profile is available:

```bash
mvn -Pstrict-convergence verify
```

`strict-convergence` adds `dependencyConvergence`, which fails the build on *any* version divergence in the
dependency graph. It is intentionally **off by default** because it is noisy on real applications — enable
it deliberately (e.g. a periodic hygiene job) rather than on every build.

---

## 🔍 Analysis profile

`mvn -Panalyze verify` enables:

- **JaCoCo** — coverage report + a line-coverage gate. Minimum is `jacoco.line.minimum` (default `0.00`
  here on the BOM; consumers raise it) and the gate only fails the build when `-Djacoco.halt=true`.
- **OWASP dependency-check** — `failBuildOnCVSS=7`, `nvdApiKey` from the `NVD_API_KEY` env var, and a
  suppression file at `dependency-check.suppressionFile` (default `etc/dependency-check/suppressions.xml`).
- **Checkstyle** — ruleset loaded from the shared `scos-build-config` artifact
  (`configLocation=checkstyle/checkstyle.xml`), so every SCOS project shares one config instead of a path
  baked into each repo.

> **`scos-build-config`** is a small sibling artifact (`br.com.sawcunhaos:scos-build-config`) that packages
> `checkstyle/checkstyle.xml` and `dependency-check/suppressions.xml` under `src/main/resources`. It is kept
> as a separate repository so the BOM stays single-module. Checkstyle is wired to consume it but is not run
> in this repo's CI; publish `scos-build-config` before enabling `checkstyle:check` in a consumer.

---

## 📊 Minimum compatibility matrix

Minimum/tested baseline per BOM line. Exact patch versions live in [`pom.xml`](pom.xml); this table tracks
only the low-churn majors used to decide compatibility.

| SCOS BOM | Java | Maven    | Spring Boot | scos-foundation |
|----------|------|----------|-------------|-----------------|
| **1.3.x**| 25+  | 3.9.12+  | 4.1.x       | 1.3.x           |

**Notes:**
- Each BOM line represents a tested, compatible set of dependencies.
- Mixing dependencies from different BOM lines is discouraged.
- `scos-foundation` aligns its minor to the BOM line; see the scos-foundation releases for exact versions.

---

## 🔢 Versioning

Semantic Versioning:

```
1.2.3
│ │ └─ PATCH: dependency bumps, security fixes
│ └──── MINOR: new managed dependencies, backward-compatible
└────── MAJOR: breaking changes in managed dependencies
```

---

## 🤝 Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for the branch flow and how to propose a version bump. Report
security issues via [SECURITY.md](SECURITY.md). Notable changes are tracked in [CHANGELOG.md](CHANGELOG.md).

Before submitting a change:

1. Open an issue describing the proposal and its compatibility impact.
2. Verify locally: `mvn -B verify` (and `mvn -B -Panalyze verify` for the full analysis).
3. Follow the commit conventions described in [CONTRIBUTING.md](CONTRIBUTING.md).

---

## 📜 License

Licensed under the **Apache License 2.0**. See [LICENSE](LICENSE).

---

## 🧭 About SawCunha Open System (SCOS)

SawCunha Open System is an open ecosystem of modular, production-grade software components. The SCOS BOM is
the foundational dependency and build standard for all SCOS projects.

## 🔗 Quick links

- **Maven Central:** https://central.sonatype.com/artifact/br.com.sawcunhaos/scos-bom
- **Repository:** https://github.com/SawCunhaOS/sawcunha-open-system-bom
- **Organization:** https://github.com/SawCunhaOS
