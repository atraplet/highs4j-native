# Highs Solver for Java Native Libraries

*Work in progress*

[![Build](https://github.com/atraplet/highs4j-native/actions/workflows/build.yml/badge.svg)](https://github.com/atraplet/highs4j-native/actions/workflows/build.yml)
[![Maven Central](https://img.shields.io/maven-central/v/com.ustermetrics/highs4j-native)](https://central.sonatype.com/artifact/com.ustermetrics/highs4j-native)
[![Apache License, Version 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://github.com/atraplet/highs4j-native/blob/master/LICENSE)

highs4j-native (Highs Solver for Java Native Libraries) contains shared library release binaries
of [Highs](https://ergo-code.github.io/HiGHS) for [highs4j](https://github.com/atraplet/highs4j) for Linux (linux_64),
Windows (windows_64), and MacOS (osx_arm64).

## Usage

### Dependency

Add the latest version from [Maven Central](https://central.sonatype.com/artifact/com.ustermetrics/highs4j-native) to
your `pom.xml`

```
<dependency>
    <groupId>com.ustermetrics</groupId>
    <artifactId>highs4j-native</artifactId>
    <version>x.y.z</version>
    <scope>runtime</scope>
</dependency>
```

## Build

## Release

Update the version in the `pom.xml`, create a tag, and push it by running

```
export HIGHS_VERSION=X.Y.Z
export VERSION=X.Y.Z
export VERSION=$VERSION-$HIGHS_VERSION
git checkout --detach HEAD
sed -i -E "s/<version>[0-9]+\-SNAPSHOT<\/version>/<version>$VERSION<\/version>/g" pom.xml
git commit -m "v$VERSION" pom.xml
git tag v$VERSION
git push origin v$VERSION
```

This will trigger the upload of the package to Maven Central via GitHub Actions.

Then, go to the GitHub repository [releases page](https://github.com/atraplet/highs4j-native/releases) and update the
release.

## Credits

This project is based on the native open source mathematical programming
solver [Highs](https://ergo-code.github.io/HiGHS), which is developed and maintained by Julian Hall, Ivet Galabova, Qi
Huangfu, Leona Gottwald, Michael Feldmeier, and other contributors. For details see https://ergo-code.github.io/HiGHS
and https://github.com/ERGO-Code/HiGHS.
