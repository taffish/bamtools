taf-bamtools 2.5.3-r1

TAFFISH wrapper for BamTools, a command-line toolkit for reading, writing,
filtering, sorting, indexing, and summarizing BAM alignment files.

Usage:
  taf-bamtools [TAF-APP-OPTION]
  taf-bamtools -- [BAMTOOLS-OPTION...]
  taf-bamtools bamtools [BAMTOOLS-COMMAND] [COMMAND-ARGS...]
  taf-bamtools <COMMAND> [COMMAND-ARGS...]

TAF app options:
  -h, --help       Show this help text
  -v, --version    Show package and command version
  --compile        Print generated shell code instead of running it
  --               Stop parsing TAFFISH wrapper options

Upstream help:
  taf-bamtools -- --help
  taf-bamtools -- --version
  taf-bamtools bamtools --help

Recommended BamTools examples:
  taf-bamtools bamtools stats -in input.bam
  taf-bamtools bamtools sort -in input.bam -out sorted.bam
  taf-bamtools bamtools index -in sorted.bam
  taf-bamtools bamtools header -in input.bam
  taf-bamtools bamtools convert -format sam -in input.bam -out output.sam

Common BamTools subcommands:
  convert
  count
  coverage
  filter
  header
  index
  merge
  random
  resolve
  revert
  sort
  split
  stats

Notes:
  - This command runs BamTools inside the TAFFISH container image.
  - BamTools subcommands such as "stats" and "sort" are subcommands of the
    "bamtools" executable, not separate commands in PATH.
  - In command mode, use "taf-bamtools bamtools <SUBCOMMAND> ..." for normal
    BamTools workflows.
  - Use "--" before upstream options when an option may be handled by the
    TAFFISH wrapper itself, such as "--help" or "--version".
  - Do not use "taf-bamtools stats ..."; "stats" is not a standalone
    executable in the container image.
  - Input and output paths should be accessible from the current working
    directory or from mounted user paths.
  - This image packages upstream BamTools 2.5.3 in a Debian 12 runtime image.

Container:
  image: ghcr.io/taffish/bamtools:2.5.3-r1
  supported backends: apptainer, podman, docker

Upstream:
  project: BamTools
  homepage: https://github.com/pezmaster31/bamtools/wiki
  source:  https://github.com/pezmaster31/bamtools
