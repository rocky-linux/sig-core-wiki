---
title: Empanadas common.py Configuration
---

The `common.py` configuration contains dictionaries and classes that dictate
most of the functionality of empanadas.

## Config Items

type: Dictionary

### config.rlmacro

type: String

required: True

description: Empanadas expects to run on an EL system. This is part of the
general check up. It should not be hardcoded and use the rpm python module.

### config.dist

type: String

required: False

description: Was the original tag placed in mock configs. This combines `el`
with the rpm python module expansion.  This is no longer required. The
option is still available for future use.

### config.arch

type: String

required: True

description: The architecture of the current running system. This is checked
against the supported architectures in general release configurations. This
should not be hardcoded.

### config.date_stamp

type: String

required: True

description: Date time stamp in the form of YYYYMMDD.HHMMSS. This should not be
hardcoded.

### config.compose_root

type: String

required: True

description: Root path of composes on the system running empanadas.

### config.staging_root

type: String

required: False

description: For future use. Root path of staging repository location where
content will be synced to.

### config.production_root

type: String

required: False

description: For future use. Root path of production repository location where
content will be synced to from staging.

### config.category_stub

type: String

required: True

description: For future use. Stub path that is appended to `staging_root` and
`production_root`.

example: `mirror/pub/rocky`

### config.sig_category_stub

type: String

required: True

description: For future use. Stub path that is appended to `staging_root` and
`production_root` for SIG content.

example: `mirror/pub/sig`

### config.repo_base_url

type: String

required: True

description: URL to the base url's where the repositories live. This is
typically to a peridot instance. This is supplemented by the configuration
`project_id` parameter.

Note that this does not have to be a peridot instance. The combination of this
value and `project_id` can be sufficient enough for empanadas to perform its
work.

### config.mock_work_root

type: String

required: True

description: Hardcoded path to where ISO work is performed within a mock chroot.
This is the default path created by mock and it is recommended not to change
this.

example: `/builddir`

### config.container

type: String

required: True

description: This is the container used to perform all operations in podman.

example: `centos:stream9`

### config.distname

type: String

required: True

description: Name of the distribution you are building or building for.

example: `Rocky Linux`

### config.shortname

type: String

required: True

description: Short name of the distribution you are building or building for.

example: `Rocky`

### config.translators

type: Dictionary

required: True

description: Translates Linux architectures to golang architectures. Reserved
for future use.

### config.aws_region

type: String

required: False

description: Region you are working in with AWS or onprem cloud that supports
this variable.

example: `us-east-2`

### config.bucket

type: String

required: False

description: Name of the S3-compatible bucket that is used to pull images from.
Requires `aws_region`.

### config.bucket_url

type: String

required: False

description: URL of the S3-compatible bucket that is used to pull images from.

## allowed_type_variants items

type: Dictionary

description: Key value pairs of cloud or image variants. The value is either
`None` or a list type.

## Reference Example

```
config = {
    "rlmacro": rpm.expandMacro('%rhel'),
    "dist": 'el' + rpm.expandMacro('%rhel'),
    "arch": platform.machine(),
    "date_stamp": time.strftime("%Y%m%d.%H%M%S", time.localtime()),
    "compose_root": "/mnt/compose",
    "staging_root": "/mnt/repos-staging",
    "production_root": "/mnt/repos-production",
    "category_stub": "mirror/pub/rocky",
    "sig_category_stub": "mirror/pub/sig",
    "repo_base_url": "https://yumrepofs.build.resf.org/v1/projects",
    "mock_work_root": "/builddir",
    "container": "centos:stream9",
    "distname": "Rocky Linux",
    "shortname": "Rocky",
    "translators": {
        "x86_64": "amd64",
        "aarch64": "arm64",
        "ppc64le": "ppc64le",
        "s390x": "s390x",
        "i686": "386"
    },
    "aws_region": "us-east-2",
    "bucket": "resf-empanadas",
    "bucket_url": "https://resf-empanadas.s3.us-east-2.amazonaws.com"
}

ALLOWED_TYPE_VARIANTS = {
        "Azure": None,
        "Container": ["Base", "Minimal", "UBI"],
        "EC2": None,
        "GenericCloud": None,
        "Vagrant": ["Libvirt", "Vbox"],
        "OCP": None

}
```
