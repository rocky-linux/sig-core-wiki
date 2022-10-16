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
the build system used by Rocky Linux to build the distribution.

For performing syncs, it relies on the use of podman to perform syncing in a
parallel fashion. This was done because it is not possible to run multiple dnf
transactions at once on a single system and looping one repository at a time is
not sustainable.

## Requirements

* Poetry must be available on the system
* Podman must be installed on the system
* fpart must be installed on the system (available in EPEL)
* Enough storage should be available if repositories are being synced

## Using Empanadas

### 

{% include "resources_bottom.md" %}
