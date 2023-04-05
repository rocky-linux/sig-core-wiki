---
title: 'SOP: Compose and Repo Sync for Rocky Linux Special Interest Groups'
---

This SOP covers how the Rocky Linux Release Engineering Team handles composes and repository syncs for Special Interest Groups.

## Contact Information
| | |
| - | - |
| **Owner** | Release Engineering Team |
| **Email Contact** | releng@rockylinux.org |
| **Email Contact** | infrastructure@rockylinux.org |
| **Mattermost Contacts** | `@label` `@mustafa` `@neil` `@tgo` |
| **Mattermost Channels** | `~Development` |

## Composing Repositories

### Mount Structure

There is a designated system that takes care of composing repositories. These systems contain the necessary EFS/NFS mounts for the staging and production repositories as well as composes.

* `/mnt/compose` -> Compose data
* `/mnt/repos-staging` -> Staging
* `/mnt/repos-production` -> Production

### Empanadas

Each repository or set of repositories are controlled by various comps and pungi configurations that are translated into peridot. Empanadas is used to run a reposync from peridot's yumrepofs repositories, generate ISO's, and create a pungi compose look-a-like. Because of this, the comps and pungi-rocky configuration is not referenced with empanadas.

### Running a Compose

First, the toolkit must be cloned. In the `iso/empanadas` directory, run `poetry install`. You'll then have access to the various commands needed:

* `sync_sig`

To perform a compose of a SIG, it must be defined in the configuration. As an example, here is composing the `core` sig.

```
# This creates a brand new directory under /mnt/compose/X and symlinks it to latest-SIG-Y-X
~/.local/bin/poetry run sync_sig --release 9 --sig core --hashed --clean-old-packages --full-run

# This assumes the directories already exist and will update in place.
~/.local/bin/poetry run sync_sig --release 9 --sig core --hashed --clean-old-packages
```

## Syncing Composes

Syncing utilizes the sync scripts provided in the release engineering toolkit.

When the scripts are being ran, they are usually ran with a specific purpose, as each major version may be different.

For SIG's, the only files you'll need to know of are `sync-to-staging-sig.sh` and `sync-to-prod-sig.sh`. The former syncs a specific sig over to staging. As of this writing, the latter syncs everything in staging to production. **Both scripts will delete packages and data that are no longer in the compose.**

```
# The below syncs the core 8 repos to staging
RLVER=8 bash sync-to-staging-sig.sh core
# The below syncs the core 9 repos to staging
RLVER=9 bash sync-to-staging-sig.sh core

# The below syncs everything in staging for 8 to prod
RLVER=8 bash sync-to-prod-sig.sh

# The below syncs everything in staging for 9 to prod
RLVER=9 bash sync-to-prod-sig.sh
```

Once staging is completed and reviewed, it is synced to production.

```
bash sync-file-list-parallel.sh
```

During this phase, staging is rsynced with production, the file list is updated, and the full time list is also updated to allow mirrors to know that the repositories have been updated and that they can sync.
