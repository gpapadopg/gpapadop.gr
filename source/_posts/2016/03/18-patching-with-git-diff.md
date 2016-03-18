---
title: Patching with git diff
tags:
    - Git
    - Patch
---

If you want to create a patch file via `git diff` that can be applied using `patch`, use the following:

	git diff --no-prefix > patchfile

and apply the patch with:

	patch -p0 < patchfile

or, without the `--no-prefix` option, with:

	patch -p1 < patchfile

This will ignore the default a/ b/ source prefixes.
