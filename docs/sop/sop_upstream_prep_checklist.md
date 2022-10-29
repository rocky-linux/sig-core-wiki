---
title: Generalized Prep Checklist for Upcoming Releases
---

This SOP contains general checklists required by SIG/Core to prepare and plan
for the upcoming release. This work, in general, is required to be done on a
routine basis, even months out before the next major or minor release, as it
requires monitoring of upstream's (CentOS Stream) work to ensure Rocky Linux
will remain ready and compatible with Red Hat Enterprise Linux.

## Contact Information

| | |
| - | - |
| **Owner** | SIG/Core (Release Engineering & Infrastructure) |
| **Email Contact** | infrastructure@rockylinux.org |
| **Email Contact** | releng@rockylinux.org |
| **Mattermost Contacts** | `@label` `@neil` `@tgo` `@skip77` `@mustafa` `@sherif` `@pgreco` |
| **Mattermost Channels** | `~Infrastructure` |

## General Upstream Monitoring

It is expected to monitor the following repositories upstream, as these will
indicate what is coming up for a given major or point release. These
repositories are found at the Red Hat gitlab.

* centos-release
* centos-logos
* pungi-centos
* comps
* module-defaults

These repositories can be monitored by setting to "all activity" on the bell
icon.

Upon changes to the upstream repositories, SIG/Core member should analyze the
changes and apply the same to the lookahead branches:

* rocky-release

    * Manual changes required

* rocky-logos

    * Manual changes required

* pungi-rocky

    * Run `sync-from-upstream`

* peridot-rocky

    * Configurations are generated using peridot tools

* comps

    * Run `sync-from-upstream`

* rocky-module-defaults

    * Run `sync-from-upstream`

## General Downward Merging

Repositories that generally track for LookAhead and Beta releases will flow
downward to the stable branch. For example:

```
* rXs / rXlh
      |
      |----> rX-beta
                |
                |----> rX
```

This applies to any specific rocky repo, such as comps, pungi, peridot-config,
and so on. As it is expected some repos will deviate in commit history, it is OK
to force push, under the assumption that changes made in the lower branch exists
in the upper branch. That way you can avoid changes/functionality being reverted
on accident.
