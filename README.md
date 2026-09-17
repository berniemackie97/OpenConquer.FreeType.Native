# OpenConquer.FreeType.Native

Reviewed native FreeType runtime package for OpenConquer.

This repository owns the source baseline, cross-platform native builds, package verification, and
NuGet release pipeline for:

```text
OpenConquer.FreeType.Native
```

OpenConquer applications consume the published NuGet package. They do not build the vendored
FreeType source as part of their own repositories.

## Supported Runtimes

The package contains native FreeType runtime assets for exactly these .NET runtime identifiers:

```text
win-x64
win-arm64
osx-x64
osx-arm64
linux-x64
linux-arm64
```

The build rejects unsupported runtime identifiers and mismatches between the declared runtime
identifier and target architecture.

## Source Baseline

FreeType source is vendored under:

```text
upstream/freetype/
```

It is intentionally not a Git submodule.

The exact upstream base, reviewed backports, resulting source-tree identity, security-review
boundary, licensing requirements, and build contract are documented in
[PROVENANCE.md](PROVENANCE.md).

Vendored FreeType source must not be updated independently of that provenance record.

## Build

Canonical platform and architecture configuration is defined in `CMakePresets.json`.

Configure and build using the preset matching the target runtime:

```bash
cmake --preset osx-arm64
cmake --build --preset osx-arm64
```

Generated output is written beneath:

```text
artifacts/
```

Official release assets are built by CI for all supported runtime identifiers rather than by
combining locally produced binaries.

## Package

`OpenConquer.FreeType.Native.csproj` produces a native-only NuGet package.

The package contains the six supported runtime assets together with the required FreeType licensing
and provenance files. It intentionally contains no managed assemblies, build targets, analyzers,
tools, or other managed package surface.

## Releases

Normal pushes and pull requests build and verify the native runtime matrix and NuGet package.

Publication is restricted to release tags matching:

```text
freetype-native-v*
```

The release workflow validates that the tagged commit is contained in `main` and that the release
tag version matches the generated NuGet package version before publication.

NuGet publication uses GitHub Actions OpenID Connect trusted publishing rather than a long-lived
NuGet API key.

## Consumers

The primary consumer is [OpenConquerClient](https://github.com/berniemackie97/OpenConquerClient).

Consumer repositories depend only on the published `OpenConquer.FreeType.Native` package. Native
source, build infrastructure, provenance maintenance, and package publication belong in this
repository.
