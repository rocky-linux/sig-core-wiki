---
title: Rocky Linux Package Patching
---

In Rocky Linux, some packages are patched either for build issues or removing Red Hat references. Some packages are completely self-managed by Release Engineering. Some packages have Red Hat or CentOS assets that cannot be shipped (for obvious reasons). Performing these types of changes are necessary for Rocky Linux to exist.

## What is Debranding?

Debranding is for the case of removing Red Hat or CentOS assets from a package and either A) Using Rocky Linux or RESF assets or B) Forcing the use of generic assets. The former is the most common and the latter happens once in a blue moon. A debrand can come in the following forms:

* Changing "Red Hat" to "Rocky"
* Changing "RHEL" or "Red Hat Enterprise Linux" to "Rocky Linux"
* Changing some "Red Hat" or "RHEL" wording to be agnostic, such as "Enterprise Linux"
* Changing bug tracker URL's to our project's specific bug tracker site
* Replacing appropriate assets (e.g. the logos packages)

The most obvious example would be httpd or nginx. While these rely on `rocky-logos-httpd` for specific pages, some assets are also replaced. This is particularly obvious with the test pages with distribution specific information.

## How are packages "debranded"?

Debrands are done using the `srpmproc` utility. The utility source code is [here](https://github.com/rocky-linux/srpmproc).

## What are "self-managed" packages?

Rocky Linux may have packages that are self-managed. Self-managed packages are packages that Red Hat may ship themselves, but it's simply easier for Release Engineering to manage them, thus reducing srpmproc headaches.

## What are "build issues"?

Some packages have to be patched, either a source code patch or a spec file change, to address a build issue. A build issue can come in multiple forms:

* Failing tests that should not fail in normal circumstances
* OOM issues for heavy packages
* Build issues where a package cannot be built properly in a container

These do not happen often. These types of changes do not affect the compatibility of the packages to our upstream.

## What type of other changes can be done?

There are very rare cases where Red Hat completely disables a package from being built or even being provided. In these very rare cases, if there is user demand, we may ensure these packages are built or provided in some way. A very common example is `openldap-servers`, which is no longer built by default. However, we enable this and provide it in our `plus` repository.

## Can I get a list of changes?

Of course, head to the [changes](changes.md) page for a list of known and documented changes that we make to our packages.

