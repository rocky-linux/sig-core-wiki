---
title: 'SOP: Compose and Repo Sync for Rocky Linux 8'
---

This SOP covers how the Rocky Linux Release Engineering Team handles composes and repository syncs for Rocky Linux 8. It contains information of the scripts that are utilized and in what order, depending on the use case.

Please see the other SOP for Rocky Linux 9+ that are managed via empanadas and peridot.

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

[Pungi](https://git.rockylinux.org/rocky/pungi-rocky) - This repository contains all the necessary pungi configuration files for composes that come from koji. Pungi interacts with koji to build the composes.

[Comps](https://git.rockylinux.org/rocky/comps) - This repository contains all the necessary comps (which are groups and other data) for a given major version. Pungi uses this information to properly build the repositories.

[Toolkit](https://github.com/rocky-linux/sig-core-toolkit) - This repository contains various scripts and utilities used by Release Engineering, such as syncing composes, functionality testing, and mirror maintenance.

## Composing Repositories

For every stable script, there is an equal beta or lookahead script available.

### Mount Structure

There is a designated system that takes care of composing repositories. These systems contain the necessary EFS/NFS mounts for the staging and production repositories as well as composes.

* `/mnt/koji` -> Koji files store
* `/mnt/compose` -> Compose data
* `/mnt/repos-staging` -> Staging
* `/mnt/repos-production` -> Production

### Pungi

Each repository or set of repositories are controlled by various pungi configurations. For example, `r8.conf` will control the absolute base of Rocky Linux 8, which imports other git repository data as well as accompanying json or other configuration files.

### Running a Compose

Inside the `pungi` git repository, the folder `scripts` contain the necessary scripts that are ran to perform a compose. There are different types of composes:

* produce -> Generates a full compose, generally used for minor releases, which generate new ISO's
* update -> Generates a smaller compose, generally used for updates within a minor release cycle - ISO's are not generated

Each script is titled appropriately:

* `produce-X.sh` -> Generates a full compose for X major release, typically set to the current minor release according to `rX.conf`
* `produce-X-full.sh` -> Generates a full compose for X major release, including extras, plus, and devel in one go.
* `updates-X.sh` -> Generates a smaller compose for X major release, typically set to the current minor release according to `rX.conf`
* `updates-X-NAME.sh` -> Generates a compose for the specific compose, such as NFV, Rocky-devel, Extras, or Plus

When these scripts are ran, they generate an appropriate directory under `/mnt/compose/X` with a directory and an accompanying symlink. For example. If an update to `Rocky` was made using `updates-8.sh`, the below would be made:

```
drwxr-xr-x. 5 root  root  6144 Jul 21 17:44 Rocky-8-updates-20210721.1
lrwxrwxrwx. 1 root  root    26 Jul 21 18:26 latest-Rocky-8 -> Rocky-8-updates-20210721.1
```

This setup also allows pungi to reuse previous package set data to reduce the time it takes to build a compose. Typically during a new minor release, all composes should be ran so they can be properly combined. Example of a typical order if releasing 8.X:

```
produce-8.sh
updates-8-devel.sh
updates-8-extras.sh
updates-8-plus.sh

# ! OR !
produce-8-full.sh
```

## Syncing Composes

Syncing utilizes the sync scripts provided in the release engineering toolkit.

When the scripts are being ran, they are usually ran for a specific purpose. They are also ran in a certain order to ensure integrity and consistency of a release.

The below are common vars files. common_X will override what's in common. Typically these set what repositories exist and how they are named or look at the top level. These also set the current major.minor release as necessary.

```
.
├── common
├── common_8
├── common_9
```

These are for the releases in general. What they do is noted below.

```
├── gen-torrents.sh                  -> Generates torrents for images
├── minor-release-sync-to-staging.sh -> Syncs a minor release to staging
├── sign-repos-only.sh               -> Signs the repomd (only)
├── sync-to-prod.sh                  -> Syncs staging to production
├── sync-to-staging.sh               -> Syncs a provided compose to staging
├── sync-to-staging-sig.sh           -> Syncs a sig provided compose to staging
```

Generally, you will only run `minor-release-sync-to-staging.sh` when a full minor release is being produced. So for example, if 8.5 has been built out, you would run that after a compose. `gen-torrents.sh` would be ran shortly after.

When doing updates, the order of operations (preferably) would be:

```
* sync-to-staging.sh
* sync-to-staging-sig.sh -> Only if sigs are updated
* sync-to-prod.sh        -> After the initial testing, it is sent to prod.
```

An example of order:

```
# The below syncs to staging
RLVER=8 bash sync-to-staging.sh Plus
RLVER=8 bash sync-to-staging.sh Extras
RLVER=8 bash sync-to-staging.sh Rocky-devel
RLVER=8 bash sync-to-staging.sh Rocky
```

Once the syncs are done, staging must be tested and vetted before being sent to production. During this stage, the `updateinfo.xml` is also applied where necessary to the repositories to provide errata. Once staging is completed, it is synced to production.

```
bash RLVER=8 sync-to-prod.sh
bash sync-file-list-parallel.sh
```

During this phase, staging is rsynced with production, the file list is updated, and the full time list is also updated to allow mirrors to know that the repositories have been updated and that they can sync.

**Note**: If multiple releases are being updated, it is important to run the syncs to completion before running the file list parallel script.
