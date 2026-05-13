# taf-bamtools

TAFFISH wrapper for [BamTools](https://github.com/pezmaster31/bamtools), a
command-line toolkit for reading, writing, filtering, sorting, indexing, and
summarizing BAM alignment files.

This repository packages upstream BamTools 2.5.3 as a TAFFISH tool app. It
exposes upstream `bamtools` through a versioned TAFFISH entry point while
keeping the runtime environment inside a portable Debian 12 container image.

## Installation

Install from the public TAFFISH Hub index:

```sh
taf update
taf install bamtools
```

Install the exact release:

```sh
taf install bamtools 2.5.3-r1
```

For local testing before the app is published to the public index:

```sh
taf install --from .
```

## Usage

Show TAFFISH app help:

```sh
taf-bamtools --help
```

Show upstream BamTools help and version:

```sh
taf-bamtools -- --help
taf-bamtools -- --version
```

Run BamTools subcommands:

```sh
taf-bamtools bamtools stats -in input.bam
taf-bamtools bamtools sort -in input.bam -out sorted.bam
taf-bamtools bamtools index -in sorted.bam
taf-bamtools bamtools convert -format sam -in input.bam -out output.sam
```

Because this is a command-mode TAFFISH tool, the first non-option argument is
treated as a command available inside the container image. BamTools subcommands
such as `stats`, `sort`, `index`, and `filter` are not separate executables;
they are subcommands of the `bamtools` executable. Use
`taf-bamtools bamtools <SUBCOMMAND> ...` for normal BamTools workflows.

Do not use:

```sh
taf-bamtools stats -in input.bam
```

`stats` is not a standalone executable in the container image.

When passing an upstream option through the default wrapper, use `--` so the
option is handled by BamTools instead of the TAFFISH wrapper:

```sh
taf-bamtools -- --version
taf-bamtools -- --help
```

## Package

```text
name: bamtools
command: taf-bamtools
version: 2.5.3-r1
kind: tool
image: ghcr.io/taffish/bamtools:2.5.3-r1
```

## Container

The container image is built from `docker/Dockerfile`. It compiles BamTools
2.5.3 from the upstream GitHub release tarball in a builder stage, then copies
only the `bamtools` executable into a slim runtime image.

```text
bamtools
```

The TAFFISH metadata declares a Docker smoke check:

```text
exist: bamtools
test:  bamtools --help
test:  bamtools --version 2>&1 | grep -F 'bamtools 2.5.3' >/dev/null
```

During TAFFISH Hub indexing, this smoke metadata is used to verify that the
published image can be inspected, that the upstream executable is available,
and that the packaged BamTools command reports version 2.5.3.

The builder stage uses `ca-certificates` and `curl` only to fetch the upstream
source tarball over HTTPS. The final runtime image does not keep those build
tools.

## Upstream

- Project: BamTools
- Homepage: <https://github.com/pezmaster31/bamtools/wiki>
- Source: <https://github.com/pezmaster31/bamtools>
- Release tags: <https://github.com/pezmaster31/bamtools/tags>
- Debian package: <https://packages.debian.org/bookworm/bamtools>
- Upstream license: MIT

## Maintainer Notes

Useful checks before publishing:

```sh
taf check
taf compile -- --help
taf compile -- bamtools --help
taf publish --release --dry-run
docker build --check -f docker/Dockerfile .
docker build -t ghcr.io/taffish/bamtools:2.5.3-r1 -f docker/Dockerfile .
docker run --rm ghcr.io/taffish/bamtools:2.5.3-r1 bamtools --help
docker run --rm ghcr.io/taffish/bamtools:2.5.3-r1 bamtools --version
```

The repository wrapper files are licensed under Apache-2.0. BamTools and the
Debian packages installed in the container are distributed under their own
upstream licenses.
