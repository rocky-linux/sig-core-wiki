---
title: Empanadas config yaml Configuration
---

Each file in `empanads/config/` is a yaml file that contains configuration
items for the distribution release version. The configuration can heavily
dictate the functionality and what features are directly supported by empanadas
when ran.

See the items below to see which options are mandatory and optional.

# Config Items

## Top Level

The Top Level is the name of the profile and starts the YAML dictionary for the
release.  It is alphanumeric and accepts punctuation within reason. Common
examples:

* `9`
* `9-beta`
* `8-lookahead`

### fullname

type: String

required: True

description: Needed for treeinfo and discinfo generation.

### revision

type: String

required: True

description: Full version of a release

### rclvl

type: String

required: True

description: Release Candidate or Beta descriptor. Sets names and versions with
this descriptor if enabled.

### major

type: String

required: True

description: Major version of a release

### minor

type: String

required: True

description: Minor version of a release

### profile

type: String

required: True

description: Matches the top level of the release. This should not differ from
the top level assignment.

### disttag

type: String

required: True

description: Sets the dist tag for mock configs.

### bugurl

type: String

required: True

description: A URL to the bug tracker for this release or distribution.

### checksum

type: String

required: True

description: Checksum type. Used when generating checksum information for
images.

### fedora_major

type: String

required: False

description: For future use with icicle.

### gpg_key

type: List

required: False

description: List of GPG keys for a given repository

### repo_gpg_key

type: List

required: False

description: List of GPG keys for a given repository. Use this if the signing key for the repo is different from packages.

### allowed_arches

type: list

required: True

description: List of supported architectures for this release.

### provide_multilib

type: boolean

required: True

description: Sets if architecture x86_64 will be multilib. It is recommended
that this is set to `True`.

### project_id

type: String

required: True

description: Appended to the base repo URL in common.py. For peridot, it is the
project id that is generated for the project you are pulling from. It can be set
to anything else if need be for non-peridot use.

### repo_symlinks

type: dict

required: False

description: For future use. Sets symlinks to repositories for backwards
compatibility. Key value pairs only.

### renames

type: dict

required: False

description: Renames a repository to the value set. For example, renaming `all`
to `devel`. Set to `{}` if no renames are goign to occur.

### all_repos

type: list

required: True

description: List of repositories that will be synced/managed by empanadas.

### structure

type: dict

required: True

description: Key value pairs of `packages` and `repodata`. These are appended
appropriately during syncing and ISO actions. Setting these are mandatory.

### iso_map

type: dictionary

required: True if building ISO's and operating with lorax.

description: Controls how lorax and extra ISO's are built.

If are you not building images, set to `{}`

#### xorrisofs

type: boolean

required: True

description: Dictates of xorrisofs is used to build images. Setting to false
uses genisoimage. It is recommended that xorrisofs is used.

#### iso_level

type: boolean

required: True

description: Set to false if you are using xorrisofs. Can be set to true when
using genisoimage.

#### images

type: dict

required: True

description: Dictates the ISO images that will be made or the treeinfo that will
be generated.

**Note**: The primary repository (for example, BaseOS) will need to be listed to
ensure the treeinfo data is correctly generated. `disc` should be set to `False`
and `isoskip` should be set to `True`. See the example section for an example.

##### name.disc

type: boolean

required: True

description: This tells the iso builder if this will be a generated ISO.

##### name.isoskip

type: boolean

required: False

description: This tells the iso builder if this will be skipped, even if `disc`
is set to `True`. Default is `False`.

##### name.variant

type: string

required: True

description: Names the primary variant repository for the image. This is set in
.treeinfo.

##### name.repos

type: list

required: True

description: Names of the repositories included in the image. This is added to
.treeinfo.

##### name.volname

type: string

required: True

required value: `dvd`

description: This is required if building more than the DVD image. By default,
the the name `dvd` is harcoded in the buildImage template.

#### lorax

type: dict

required: True if building lorax images.

description: Sets up lorax images and which repositories to use when building
lorax images.

##### lorax.repos

type: list

required: True

description: List of repos that are used to pull packages to build the lorax
images.

##### lorax.variant

type: string

required: True

description: Base repository for the release

##### lorax.lorax_removes

type: list

required: False

description: Excludes packages that are not needed when lorax is running.

##### lorax.required_pkgs

type: list

required: True

description: Required list of installed packages needed to build lorax images.

#### livemap

type: dict

required: False

description: Dictates what live images are built and how they are built.

##### livemap.git_repo

type: string

required: True

description: The git repository URL where the kickstarts live

##### livemap.branch

type: string

required: True

description: The branch being used for the kickstarts

##### livemap.ksentry

type: dict

required: True

description: Key value pairs of the live images being created. Key being the
name of the live image, value being the kickstart name/path.

##### livemap.allowed_arches

type: list

required: True

description: List of allowed architectures that will build for the live images.

##### livemap.required_pkgs

type: list

required: True

description: Required list of packages needed to build the live images.

#### cloudimages

type: dict

required: False

description: Cloud related settings.

Set to `{}` if not needed.

##### cloudimages.images

type: dict

required: True

description: Cloud images that will be generated and in a bucket to be pulled,
and their format.

###### cloudimages.images.name

type: dict

required: True

description: Name of the cloud image being pulled.

Accepted key value options:

* `format`, which is `raw`, `qcow2`, `vhd`, `tar.xz`
* `variants`, which is a list
* `primary_variant`, which symlinks to the "primary" variant in the variant list

#### repoclosure_map

type: dict

required: True

description: Repoclosure settings. These settings are absolutely required when
doing full syncs and need to check repositories for consistency.

##### repoclosure_map.arches

type: dict

required: True

description: For each architecture (key), dnf switches/settings that dictate how
repoclosure will check for consistency (value, string).

example: `x86_64: '--forcearch=x86_64 --arch=x86_64 --arch=athlon --arch=i686 --arch=i586 --arch=i486 --arch=i386 --arch=noarch'`

##### repoclosure_map.repos

type: dict

required: True

description: For each repository that is pulled for a given release(key),
repositories that will be included in the repoclosure check. A repository that
only checks against itself must have a value of `[]`.

#### extra_files

type: dict

required: True

description: Extra files settings and where they come from. Git repositories are
the only supported method.

##### extra_files.git_repo

type: string

required: True

description: URL to the git repository with the extra files.

##### extra_files.git_raw_path

type: string

required: True

description: URL to the git repository with the extra files, but the "raw" url
form.

example: `git_raw_path: 'https://git.rockylinux.org/staging/src/rocky-release/-/raw/r9/'`

##### extra_files.branch

type: string

required: True

description: Branch where the extra files are pulled from.

##### extra_files.gpg

type: dict

required: True

description: For each gpg key type (key), the relative path to the key in the
git repository (value).

These keys help set up the repository configuration when doing syncs.

By default, the RepoSync class sets `stable` as the gpgkey that is used.

##### extra_files.list

type: list

required: True

description: List of files from the git repository that will be used as "extra"
files and placed in the repositories and available to mirrors and will appear on
ISO images if applicable.

# Reference Example

```
---
'9':
  fullname: 'Rocky Linux 9.0'
  revision: '9.0'
  rclvl: 'RC2'
  major: '9'
  minor: '0'
  profile: '9'
  disttag: 'el9'
  bugurl: 'https://bugs.rockylinux.org'
  checksum: 'sha256'
  fedora_major: '20'
  allowed_arches:
    - x86_64
    - aarch64
    - ppc64le
    - s390x
  provide_multilib: True
  project_id: '55b17281-bc54-4929-8aca-a8a11d628738'
  repo_symlinks:
    NFV: 'nfv'
  renames:
    all: 'devel'
  all_repos:
    - 'all'
    - 'BaseOS'
    - 'AppStream'
    - 'CRB'
    - 'HighAvailability'
    - 'ResilientStorage'
    - 'RT'
    - 'NFV'
    - 'SAP'
    - 'SAPHANA'
    - 'extras'
    - 'plus'
  structure:
    packages: 'os/Packages'
    repodata: 'os/repodata'
  iso_map:
    xorrisofs: True
    iso_level: False
    images:
      dvd:
        disc: True
        variant: 'AppStream'
        repos:
          - 'BaseOS'
          - 'AppStream'
      minimal:
        disc: True
        isoskip: True
        repos:
          - 'minimal'
          - 'BaseOS'
        variant: 'minimal'
        volname: 'dvd'
      BaseOS:
        disc: False
        isoskip: True
        variant: 'BaseOS'
        repos:
          - 'BaseOS'
          - 'AppStream'
    lorax:
      repos:
        - 'BaseOS'
        - 'AppStream'
      variant: 'BaseOS'
      lorax_removes:
        - 'libreport-rhel-anaconda-bugzilla'
      required_pkgs:
        - 'lorax'
        - 'genisoimage'
        - 'isomd5sum'
        - 'lorax-templates-rhel'
        - 'lorax-templates-generic'
        - 'xorriso'
  cloudimages:
    images:
      EC2:
        format: raw
      GenericCloud:
        format: qcow2
  livemap:
    git_repo: 'https://git.resf.org/sig_core/kickstarts.git'
    branch: 'r9'
    ksentry:
      Workstation: rocky-live-workstation.ks
      Workstation-Lite: rocky-live-workstation-lite.ks
      XFCE: rocky-live-xfce.ks
      KDE: rocky-live-kde.ks
      MATE: rocky-live-mate.ks
    allowed_arches:
      - x86_64
      - aarch64
    required_pkgs:
      - 'lorax-lmc-novirt'
      - 'vim-minimal'
      - 'pykickstart'
      - 'git'
  variantmap:
    git_repo: 'https://git.rockylinux.org/rocky/pungi-rocky.git'
    branch: 'r9'
    git_raw_path: 'https://git.rockylinux.org/rocky/pungi-rocky/-/raw/r9/'
  repoclosure_map:
    arches:
      x86_64: '--forcearch=x86_64 --arch=x86_64 --arch=athlon --arch=i686 --arch=i586 --arch=i486 --arch=i386 --arch=noarch'
      aarch64: '--forcearch=aarch64 --arch=aarch64 --arch=noarch'
      ppc64le: '--forcearch=ppc64le --arch=ppc64le --arch=noarch'
      s390x: '--forcearch=s390x --arch=s390x --arch=noarch'
    repos:
      devel: []
      BaseOS: []
      AppStream:
        - BaseOS
      CRB:
        - BaseOS
        - AppStream
      HighAvailability:
        - BaseOS
        - AppStream
      ResilientStorage:
        - BaseOS
        - AppStream
      RT:
        - BaseOS
        - AppStream
      NFV:
        - BaseOS
        - AppStream
      SAP:
        - BaseOS
        - AppStream
        - HighAvailability
      SAPHANA:
        - BaseOS
        - AppStream
        - HighAvailability
  extra_files:
    git_repo: 'https://git.rockylinux.org/staging/src/rocky-release.git'
    git_raw_path: 'https://git.rockylinux.org/staging/src/rocky-release/-/raw/r9/'
    branch: 'r9'
    list:
      - 'SOURCES/Contributors'
      - 'SOURCES/COMMUNITY-CHARTER'
      - 'SOURCES/EULA'
      - 'SOURCES/LICENSE'
      - 'SOURCES/RPM-GPG-KEY-Rocky-9'
      - 'SOURCES/RPM-GPG-KEY-Rocky-9-Testing'
...
```
