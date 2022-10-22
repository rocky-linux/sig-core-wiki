---
title: Rocky Release Procedures for SIG/Core (RelEng/Infrastructure)
---

This SOP contains all the steps required by SIG/Core (a mix of Release
Engineering and Infrastructure) to perform releases of all Rocky Linux
versions. Work is in all collaboration within the entire group of
engineerings.

## Contact Information

| | |
| - | - |
| **Owner** | SIG/Core (Release Engineering & Infrastructure) |
| **Email Contact** | infrastructure@rockylinux.org |
| **Email Contact** | releng@rockylinux.org |
| **Mattermost Contacts** | `@label` `@neil` `@tgo` `@skip77` `@mustafa` `@sherif` `@pgreco` |
| **Mattermost Channels** | `~Infrastructure` |

## Preparation

## Notes about Release Day

Within a minimum of two (2) days, the following should be true:

1. Torrents should be setup. All files can be synced with the seed box(es) but
not yet published. The data should be verified using sha256sum and compared to
the CHECKSUM files provided with the files.

2. Website should be ready (typically with an open PR in github). The content
should be verified that the design and content are correct and finalized.

3. Enough mirrors should be setup. This essentially means that all content for
a release should be synced to our primary mirror with the executable bit turned
off, and the content should also be hard linked. In theory, mirror manager can
be queried to verify if mirrors are or appear to be in sync.

## Notes about Patch Days

Within a minimum of one (1) to two (2) days, the following should be true:

1. Updates should be completed in the build system, and verified in staging.

2. Updates should be sent to production and file lists updated to allow mirrors
to sync.

## Prior to Release Day notes

Ensure the SIG/Core Checklist is read thoroughly and executed as listed.

## Release Day

### Priorities

During release day, these should be verified/completed in order:

1. Website - The primary website and user landing at rockylinux.org should allow
the user to efficiently click through to a download link of an ISO, image, or
torrent. It must be kept up.

2. Torrent - The seed box(es) should be primed and ready to go for users
downloading via torrent.

3. Release Notes & Documentation - The release notes are often on the same
website as the documentation. The main website and where applicable in the docs
should refer to the Release Notes of Rocky Linux.

4. Wiki - If applicable, the necessary changes and resources should be available
for a release. In particular, if a major release has new repos, changed repo names,
this should be documented.

5. Everything else!

## Resources

## SIG/Core Checklist

### Beta

* Compose Completed
* Repoclosure must be checked and pass
* Lorax Run
* ISO's are built
* Cloud Images built
* Live Images built
* Compose Synced to Staging
* AWS/Azure Images in Marketplace
* Vagrant Images
* Container Images
* Mirror Manager

    * Ready to Migrate from previous beta release (rltype=beta)
    * Boot image install migration from previous beta release

* Pass image to Testing Team for final validation

### Release Candidate

* Compose Completed
* Repoclosure must be checked and pass
* Lorax Run
* ISO's are built
* Cloud Images built
* Live Images built
* Compose Synced to Staging
* AWS/Azure Images in Marketplace
* Vagrant Images
* Container Images
* Mirror Manager

    * Ready to Migrate from previous release
    * Boot image install migration from previous release

* Pass image to Testing Team for validation

### Final

* Compose Completed
* Repoclosure must be checked and pass
* Lorax Run
* ISO's are built
* Cloud Images built
* Live Images built
* Compose Synced to Staging
* AWS/Azure Images in Marketplace
* Vagrant Images
* Container Images
* Mirror Manager

    * Ready to Migrate from previous release
    * Boot image install migration from previous release

* Pass image to Testing Team for final validation
* Sync to Production
* Sync to Europe Mirror if applicable
* Hardlink Run
* Bitflip after 24-48 Hours

{% include "resources_bottom.md" %}
