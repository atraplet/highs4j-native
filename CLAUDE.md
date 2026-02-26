# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

highs4j-native bundles pre-compiled native shared libraries of the [HiGHS](https://highs.dev) mathematical programming solver for use with the [highs4j](https://github.com/atraplet/highs4j) Java wrapper. It distributes platform-specific binaries (Linux x86_64, Windows x86_64, macOS ARM64) via Maven Central.

**This project contains no Java source code in `src/main/java`.** It is purely a native library resource bundler. The only Java code is a single test.

## Build Commands

```bash
# Build and run tests
mvn clean verify

# Run only the test (LoadLibraryTest verifies native library loading)
mvn test

# Package with platform-specific classifier JARs
mvn package
```

Tests require `--enable-native-access=ALL-UNNAMED` JVM flag (configured in pom.xml via maven-surefire-plugin). Requires JDK 25.

OpenBLAS must be installed on the system for tests to pass (the native HiGHS library depends on it). On Ubuntu: `sudo apt-get install libopenblas-dev`.

## Architecture

### Native Library Distribution

- Native binaries live in `src/main/resources/natives/{platform}/` where platform is `linux_64`, `windows_64`, or `osx_arm64`
- The maven-jar-plugin creates four JARs during `package`: one default JAR with all platforms, plus three classifier-specific JARs (`linux_64`, `windows_64`, `osx_arm64`) each containing only that platform's library
- Consumers use the `classifier` element in their Maven dependency to pull a specific platform

### Native Library Build Pipeline

Native libraries are **not built locally**. They are compiled via GitHub Actions (`build_natives.yml`) which:
1. Checks out HiGHS source at a pinned version (currently v1.13.1)
2. Applies patches from `patches/` (adds `-DJAVA=ON` build option to HiGHS CMakeLists.txt)
3. Builds on each platform with CMake flags: `-DHIGHSINT64=ON -DJAVA=ON -DHIPO=ON -DCMAKE_BUILD_TYPE=Release`
4. Uploads the resulting shared libraries as artifacts

### Release Process

Releases are triggered by pushing a `v*` tag. The `release.yml` workflow builds natives, tests on all platforms, signs with GPG, and publishes to Maven Central. See README.md for the exact version/tag commands.

### Module System

Uses `Automatic-Module-Name: com.ustermetrics.highs4j_native` for JPMS compatibility. The `build-helper-maven-plugin` converts the artifact ID hyphens to underscores for the module name.
