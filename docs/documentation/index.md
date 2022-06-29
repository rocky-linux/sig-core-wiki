---
title: Composing Releases
---

This section goes over at a high level how we compose releases for Rocky Linux.
As most of our tools are home grown, we have made sure that the tools are open
source and in our git services.

This page should serve as an idea of the steps we generally take and we hope
that other projects out there who wish to also use our tools can make sure they
can use them in this same way, whether they want to be an Enterprise Linux
derivative or another project entirely.

## Build System and Tools

The tools in use for the distribution are in the table below.

| Tool            | Maintainer        | Code Location                                                       |
|-----------------|-------------------|---------------------------------------------------------------------|
| srpmproc        | SIG/Core at Rocky | [GitHub](https://github.com/rocky-linux/srpmproc)                   |
| empanadas       | SIG/Core at Rocky | [sig-core-toolkit](https://github.com/rocky-linux/sig-core-toolkit) |
| Peridot         | SIG/Core at Rocky |                                                                     |
| MirrorManager 2 | Fedora Project    | [MirrorManager2](https://github.com/fedora-infra/mirrormanager2)    |

For Rocky Linux to be build, we use `Peridot` as the build system and
`empanadas` to "compose" the distribution. As we do not use Koji for Rocky
Linux beyond version 9, pungi no longer can be used. Peridot instead takes
pungi configuration data and comps and transforms them into a format it
can understand. Empanadas then comes in to do the "compose" and sync
all the repositories down.

## Full Compose (major or minor releases)

Step by step, it looks like this:

* Distribution is built and maintained in Peridot
* Comps and pungi configuration is converted into the peridot format for the project
* Repositories are created in yumrepofs based on the configuration provided
* In Parallel:
  * Repositories are synced as a "full run" in empanadas
  * Lorax is ran using empanadas in the peridot cluster
* Lorax results are pulled down from an S3 bucket
* DVD images are built for each architecture
* Compose directory is synced to staging for verification

## General Updates

Step by step, it looks like this:

* Distribution is maintained in Peridot
* Updates are built, repos are then "hashed" in yumrepofs
* Empanadas syncs updates as needed, either per repo or all repos at once
* Updates are synced to staging to be verified
* Staging is synced to production to allow mirror syncing

{% include "resources_bottom.md" %}
