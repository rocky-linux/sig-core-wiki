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

<h4>Resources</h4>

=== "Account Services"

    **URL**: [https://accounts.rockylinux.org](https://accounts.rockylinux.org)

    **Purpose**: Account Services maintains the accounts for almost all components of the Rocky ecosystem

    **Technology**: Noggin used by Fedora Infrastructure

    **Contact**: `~Infrastructure` in Mattermost and `#rockylinux-infra` in Libera IRC

=== "Git (RESF Git Service)"

    **URL**: [https://git.resf.org](https://git.resf.org)

    **Purpose**: General projects, code, and so on for the Rocky Enterprise Software Foundation.

    **Technology**: [Gitea](https://gitea.io/en-us/)

    **Contact**: `~Infrastructure`, `~Development` in Mattermost and `#rockylinux-infra`, `#rockylinux-devel` in Libera IRC

=== "Git (Rocky Linux GitHub)"

    **URL**: [https://github.com/rocky-linux](https://github.com/rocky-linux)

    **Purpose**: General purpose code, assets, and so on for Rocky Linux. Some content is mirrored to the RESF Git Service.

    **Technology**: [GitHub](https://github.com)

    **Contact**: `~Infrastructure`, `~Development` in Mattermost and `#rockylinux-infra`, `#rockylinux-devel` in Libera IRC


=== "Git (Rocky Linux GitLab)"

    **URL**: [https://git.rockylinux.org](https://git.rockylinux.org)

    **Purpose**: Packages and light code for the Rocky Linux distribution

    **Technology**: [GitLab](https://gitlab.com)

    **Contact**: `~Infrastructure`, `~Development` in Mattermost and `#rockylinux-infra`, `#rockylinux-devel` in Libera IRC

=== "Mail Lists"

    **URL**: [https://lists.resf.org](https://lists.resf.org)

    **Purpose**: Users can subscribe and interact with various mail lists for the Rocky ecosystem

    **Technology**: Mailman 3 + Hyper Kitty

    **Contact**: `~Infrastructure` in Mattermost and `#rockylinux-infra` in Libera IRC

=== "Contacts"

    | Name                            | Email                   | Mattermost Name   | IRC Name           |
    |---------------------------------|-------------------------|-------------------|--------------------|
    | Louis Abel                      | label@rockylinux.org    | @label            | Sokel/label/Sombra |
    | Mustafa Gezen                   | mustafa@rockylinux.org  | @mustafa          | mstg               |
    | Skip Grube                      | skip@rockylinux.org     | @skip77           |                    |
    | Sherif Nagy                     | sherif@rockylinux.org   | @sherif           |                    |
    | Pablo Greco                     | pgreco@rockylinux.org   | @pgreco           | pgreco             |
    | Neil Hanlon                     | neil@resf.org           | @neil             | neil               |
    | Taylor Goodwill                 | tg@resf.org             | @tgo              | tg                 |

