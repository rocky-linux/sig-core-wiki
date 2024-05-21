---
title: Empanadas
---

This page goes over `empanadas`, which is part of the SIG/Core toolkit. Empanadas
assists SIG/Core is composing repositories, creating ISO's, creating images, and
various other activities in Rocky Linux. It is also used for general testing and
debugging of repositories and its metadata.

## Contact Information

| | |
| - | - |
| **Owner** | SIG/Core (Release Engineering & Infrastructure) |
| **Email Contact** | releng@rockylinux.org |
| **Mattermost Contacts** | `@label` `@neil` |
| **Mattermost Channels** | `~Development` |

## General Information

`empanadas` is a python project using poetry, containing various built-in modules
with the goal to try to emulate the Fedora Project's pungi to an extent. While it
is not perfect, it achieves the very basic goals of creating repositories, images
and ISO's for consumption by the end user. It also has interactions with peridot,
the build system used by the RESF to build the Rocky Linux distribution.

For performing syncs, it relies on the use of podman to perform syncing in a
parallel fashion. This was done because it is not possible to run multiple dnf
transactions at once on a single system and looping one repository at a time is
not sustainable (nor fast).

## Requirements

* Poetry must be installed on the system
* Podman must be installed on the system
* `fpart` must be installed on the system (available in EPEL on EL systems)
* Enough storage should be available if repositories are being synced
* `mock` must be installed if building live images
* System must be an Enterprise Linux system or Fedora with the `%rhel` macro set

## Features

As of this writing, `empanadas` has the following abilities:

* Repository syncing via dnf from a peridot instance or applicable repos
* Per profile dnf repoclosure checking for all applicable repos
* Per profile dnf repoclosure checking for peridot instance repositories
* Basic ISO Building via `lorax`
* Extra ISO Building via `xorriso` for DVD and minimal images
* Live ISO Building using `livemedia-creator` and `mock`
* Anaconda treeinfo builder
* Cloud Image builder

## Installing Empanadas

The below is how to install empanadas from the development branch on a Fedora
system.

```
% dnf install git podman fpart poetry mock -y
% git clone https://git.resf.org/sig_core/toolkit.git -b devel
% cd toolkit/iso/empanadas
% poetry install
```

## Configuring Empanadas

Depending on how you are using empanadas will depend on how your configurations
will be setup.

* `empanadas/common.py`
* `empanadas/config/*.yaml`
* `empanadas/sig/*.yaml`

These configuration files are delicate and can control a wide variety of the
moving parts of empanadas. As these configurations are fairly massive, we
recommend checking the [reference guides](./references/) for deeper details into
configuring for base distribution or "SIG" content.

## Using Empanadas

The most common way to use empanadas is to sync repositories from a peridot
instance. This is performed upon each release or on each set of updates as they
come from upstream. Below lists how to use `empanadas`, as well as the common
options.

Note that for each of these commands, it is fully expected you are running
`poetry run` in the root of empanadas.

```
# Syncs all repositoryes for the "9" release
% poetry run sync-from-peridot --release 9 --clean-old-packages

# Syncs only the BaseOS repository without syncing sources
% poetry run sync-from-peridot --release 9 --clean-old-packages --repo BaseOS --ignore-source

# Syncs only AppStream for ppc64le
% poetry run sync-from-peridot --release 9 --clean-old-packages --repo AppStream --arch ppc64le
```

{% include "resources_bottom.md" %}
