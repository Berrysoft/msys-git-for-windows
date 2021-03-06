# Copyright statements
Many `PKGBUILD` are not totally written by myself, and the MIT `LICENSE` here may conflict with the original one.
If you care about the license, please review the code and compare it with the following repos:

* [MSYS2 packages](https://github.com/msys2/MSYS2-packages)
* [MinGW packages](https://github.com/msys2/MINGW-packages)
* [git-for-windows](https://github.com/git-for-windows/MINGW-packages)
* [build-extra](https://github.com/git-for-windows/build-extra)
* Arch Linux packages

# `mingw-w64-git`
This repo is a `PKGBUILD` for [`git-for-windows`](https://github.com/git-for-windows/).
There's already one in the [official repo](https://github.com/git-for-windows/MINGW-packages/tree/main/mingw-w64-git),
but it uses the old Makefile build system and is complicated to port to multiple toolchains in msys2.

## Build system
This repo uses the new CMake build system to build the git binaries.
The Makefile build system is still used to build the docs and some other tools.

## Patches
I add some patches for multiple proposes.

1. 32-bit build and clang builds need some changes.
2. The version generated by `CMakeLists.txt` is wrong. I also add a custom postfix.
3. `CMakeLists.txt` lacks pcre2 support. Tracking: [git/git#1267](https://github.com/git/git/pull/1267)
4. `headless-git` should also be installed.
5. Remove `UNICODE` definitions. Tracking: [git/git#1269](https://github.com/git/git/pull/1269)
6. Fix shebang parsing. Tracking: [git-for-windows/git#3869](https://github.com/git-for-windows/git/pull/3869)

# `mingw-w64-git-cal`
It shows a GitHub like calendar.
Some terminals won't show correct format due to the width of characters.

# `mingw-w64-git-credential-manager`
It doesn't build from source, because .NET SDK is not part of MSYS2.

# `mingw-w64-git-crypt`
It is (maybe) newer than the MSYS2 one.

# `mingw-w64-git-delta`
A colorful diff plugin for `git`.
It is actually `delta`, but mostly works for `git`.

# `mingw-w64-git-extras`
They are just a series of scripts for `git`.

# `mingw-w64-git-filter-repo`
It is said to perform better than `git-filter-branch`.

# `mingw-w64-git-fire`
In case on fire, it is very useful to save your works.

# `mingw-w64-git-flow`
This repo is NOT maintained with `git-flow`.

# `mingw-w64-git-lfs`
It is newer than the one in the [official repo](https://github.com/msys2/MINGW-packages/tree/master/mingw-w64-git-lfs).

# `mingw-w64-git-recall`
Recall works with interactive UI.

# `mingw-w64-git-sizer`
It builds from source, and installs to `libexec/git-core`.

# `mingw-w64-git-smash`
No documents because the author is [working](https://github.com/anthraxx/git-smash/issues/1) on it.

# `mingw-w64-git-warp-time`
A tool fixes timestamps. The version is postfixed.

# Usage
Note that some packages should be built before `mingw-w64-git` is installed,
because two `git` will confuse each other.

``` bash
$ git clone https://github.com/Berrysoft/msys-git-for-windows.git
$ cd msys-git-for-windows
$ # cd to the package folder...
$ makepkg-mingw -sifC
```
