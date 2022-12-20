---
title: Rebuild Version Bump
---

In some cases, a package has to be rebuilt. A package may be rebuilt for these reasons:

* Underlying libraries have been rebased
* ABI changes that require a rebuild (mass rebuilds, though they are rare)
* New architecture added to a project

This typically applies to packages being built from a given `src` subgroup. Packages pulled from upstream don't fall into this category in normal circumstances. In those cases, they receive `.0.1` and so on as standalone rebuilds.
