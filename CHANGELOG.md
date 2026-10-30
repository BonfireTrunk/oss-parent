# Changelog

All notable changes to this project are documented here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and versions follow [Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added

- `org.jspecify:jspecify` 1.0.1 is managed. A child declares it without a version to use JSpecify nullness annotations.
- Every jar gets an OSGi manifest from maven-bundle-plugin, plus `Automatic-Module-Name` when the child sets `module.name`. A child no longer needs its own maven-bundle-plugin and maven-jar-plugin setup for this.
- `LICENSE` and `NOTICE` from the project root are packaged into the jar under `META-INF/`.
- maven-failsafe-plugin is managed at Surefire's version, with the same test JVM settings. A child adds it with its `integration-test` and `verify` goals.
- The Testcontainers BOM 2.0.5 is imported, so a child declares Testcontainers modules without a version.

### Changed

- maven-surefire-plugin 3.6.0 to 3.6.1.

### Fixed

- The opt-in JaCoCo plugin now records coverage. The Surefire setting that loads Mockito's agent replaced the agent JaCoCo adds, so no coverage data or report was written.

## [2.1.1] - 2026-10-29

### Added

- Releases of this parent are also published to GitHub Packages, from the same signed files as Maven Central.

### Changed

- The `upload` and `publish` profiles send Maven Central only the checksums it requires (`.md5` and `.sha1`). A jar library release drops from 24 files to 16, which counts less toward Central's monthly publishing limits.
- The `publish` profile waits only until Central has validated a release, not until it is published. Central still publishes it on its own after validation.

## [2.1.0] - 2026-10-28

### Added

- Java formatting with Spotless and palantir-java-format. Local builds format sources automatically; CI builds (`CI=true`) only check, and fail on unformatted code.

## [2.0.0] - 2026-10-28

First release of the 2.x line. The [README](https://github.com/BonfireTrunk/oss-parent/blob/prime/README.md) lists what a child inherits.
