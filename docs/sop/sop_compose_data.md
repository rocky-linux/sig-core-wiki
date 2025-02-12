---
title: 'SOP: Pungi and Comps Sync
---

This SOP covers how the Rocky Linux Release Engineering Team handles the synchronization of pungi and comps data from upstream. This data is used to help with composes of all Rocky Linux releases.

## Contact Information
| | |
| - | - |
| **Owner** | Release Engineering Team |
| **Email Contact** | releng@rockylinux.org |
| **Email Contact** | infrastructure@rockylinux.org |
| **Mattermost Contacts** | `@label` `@mustafa` `@neil` `@tgo` |
| **Mattermost Channels** | `~Development` |

## Related Git Repositories

There are several git repositories used in the overall composition of a repository or a set of repositories.

[Pungi](https://git.rockylinux.org/rocky/pungi-rocky) - This repository contains all the necessary pungi configuration files that peridot translates into its own configuration. Pungi is no longer used for Rocky Linux.

[Comps](https://git.rockylinux.org/rocky/comps) - This repository contains all the necessary comps (which are groups and other data) for a given major version. Peridot (and pungi) use this information to properly build repositories.

## Comps

Comps files define how packages are bundled for an installation. They typically define package groups and environments with varying settings for architectures and repository designations. A single "comps" file is split for each repository per architecture as necessary, which are then inserted into the repositories themselves. These comps help not only for users who run `dnf group list` or `dnf group install`, but it primarily aids in system installation through anaconda.

### Syncing Comps from Upstream

The comps repository has a single branch of "main". All scripts that are needed to sync from upstream are found in the `scripts` directory.

```
.
├── commit                  -> Used to determine what files were changed
├── comps-distro-only.xsl   -> Distribution specific settings
├── conv-to-stable.sh       -> Unused
├── Makefile                -> Only used when `make` is called in a pungi run
├── sync-from-upstream      -> Syncs CentOS Stream 9 comps data
├── sync-from-upstream-10   -> Syncs CentOS Stream 10 comps data
└── update-comps            -> Converts the synced comps file to a usable format
```

When running a sync, the typical order is:

```
bash scripts/sync-from-upstream
bash scripts/sync-from-upstream-10
bash scripts/commit
git push origin main
```

## Pungi

Pungi is a distribution compose tool. Composes are essentially a release snapshot, in which repositories are generated with comps, repodata, potential kickstart trees, and bootable ISO's. Pungi can do "full" runs which pretty much do everything listed, but it can also be instructed to do a simple snapshot and not create ISO's, for example.

### Syncing Pungi from Upstream

The pungi repository is sorted into different branches, based on the version that is being targetted. The format of the branches are like so:

* `rX` (legacy)
* `rX-beta` (legacy)
* `rXs`
* `rX.Y`

During a sync from upstream, only the `rXs` branch receives changes. These changes are performed by running:

```
bash scripts/sync-from-upstream
bash scripts/commit
```

Typically, only the "shared" directory receives modifications. All other overrides or changes specifically for Rocky Linux goes into the `rocky` or `rocky-lookahead` directory.

### Branch-off Procedure

When it is determined that the current commit for the `rXs` branch is no longer receiving changes for the upcoming release, it is simply branched off using regular git procedures and then pushed normally. Replace X and Y as needed.

```
git checkout -b rX.Y
git push origin rX.Y
```
