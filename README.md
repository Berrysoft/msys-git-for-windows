This repo is a `PKGBUILD` for [`git-for-windows`](https://github.com/git-for-windows/).
There's already one in the [official repo](https://github.com/git-for-windows/MINGW-packages/tree/main/mingw-w64-git),
but it uses the old Makefile build system and is complicated to port to multiple toolchains in msys2.

This repo uses the new CMake build system to build the git binaries.
The Makefile build system is still used to build the docs.
