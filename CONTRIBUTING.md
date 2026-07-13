# Contributing to SCOS BOM

Thanks for helping keep the SawCunha Open System dependency baseline healthy. This repository is a
**Bill of Materials** (`packaging pom`, no code): contributions are almost always changes to managed
versions, build conventions, CI, or documentation.

## Ground rules

- Open an **issue first** for anything beyond a trivial fix, describing the change and its compatibility
  impact.
- Keep the build green: `mvn -B verify` (and `mvn -B -Panalyze verify` for the full analysis).
- One logical change per pull request.
- `@sawcunha` owns all protected paths (see [CODEOWNERS](.github/CODEOWNERS)) and must approve.

## Commit conventions

This project uses [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <subject>
```

Common types: `feat`, `fix`, `refactor`, `docs`, `ci`, `chore`. Example:
`fix(bom): corrige import de spring-data-bom`.

## Branching model

| Branch          | Meaning                                             | Version           |
|-----------------|-----------------------------------------------------|-------------------|
| `develop`       | Integration; next major line                        | `x.0.0-SNAPSHOT`  |
| `release/x.y.z` | A minor being stabilized                            | `x.y.z-SNAPSHOT`  |
| `fix/x.y.z`     | A patch line, branched from its release             | `x.y.z-SNAPSHOT`  |

- Target **`develop`** for new managed dependencies and conventions.
- Target the relevant **`release/*`** branch for stabilization of an upcoming minor.
- Target the relevant **`fix/*`** branch for patch-level security/dependency bumps.

A `release/x.y.z` branch legitimately carries `x.y.z-SNAPSHOT` — the concrete release version is stamped
only when the branch is closed. Releases are cut by maintainers via
[`scripts/release.sh`](scripts/release.sh) (`major|minor|fix`), which strips `-SNAPSHOT`, tags, deploys to
Maven Central, merges back, and opens the next SNAPSHOT.

## Proposing a version bump

The managed versions are `*.version` properties at the top of [`pom.xml`](pom.xml). To change one:

1. **Prefer Renovate.** Most bumps arrive automatically as grouped PRs (see
   [`renovate.json`](renovate.json)). Review, verify, and merge those rather than hand-editing.
2. **Manual bump.** Edit the single `*.version` property (never hardcode a version inline on a dependency).
   Justify the bump in the PR: what changed upstream, why it is safe, and any transitive impact.
3. **Verify convergence.** Run `mvn -B verify`. For a deeper check on a consumer, run
   `mvn -Pstrict-convergence verify`.
4. **Semantic impact.** A new managed dependency or a change that can break consumers is a **minor**
   (target `develop`/`release/*`); a straight security/patch bump is a **patch** (target `fix/*`).
5. **Document** the change in [CHANGELOG.md](CHANGELOG.md) under `[Unreleased]`.

## Local checks

```bash
mvn -B validate              # POM model is well-formed
mvn -B verify                # enforcer rules (Maven/Java floor, duplicates, upper bounds)
mvn -B -Panalyze verify      # + JaCoCo / dependency-check / Checkstyle
mvn help:effective-pom       # inspect the fully-resolved model
```
