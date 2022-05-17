# Maintainer: Berrysoft <Strawberry_Str@hotail.com>

_realname=git
pkgbase="mingw-w64-${_realname}"
pkgname=("${MINGW_PACKAGE_PREFIX}-${_realname}"
	     "${MINGW_PACKAGE_PREFIX}-${_realname}-doc-html"
	     "${MINGW_PACKAGE_PREFIX}-${_realname}-doc-man")
tag=2.36.1.windows.1
pkgver=2.36.1.1.e2ff68a2d14
pkgrel=1
pkgdesc="The fast distributed version control system (mingw-w64)"
arch=('any')
url="https://git-for-windows.github.io/"
license=('GPL2')

options=()
makedepends=("git"
             "docbook-xsl"
             "${MINGW_PACKAGE_PREFIX}-cmake"
             "${MINGW_PACKAGE_PREFIX}-make"
             "${MINGW_PACKAGE_PREFIX}-ninja"
             "${MINGW_PACKAGE_PREFIX}-asciidoc"
             "${MINGW_PACKAGE_PREFIX}-cc")

depends=("${MINGW_PACKAGE_PREFIX}-curl"
         "${MINGW_PACKAGE_PREFIX}-ca-certificates"
         "${MINGW_PACKAGE_PREFIX}-expat>=2.0"
         "${MINGW_PACKAGE_PREFIX}-openssl"
         "${MINGW_PACKAGE_PREFIX}-tcl"
         "${MINGW_PACKAGE_PREFIX}-tk"
         "perl-Error"
         "perl>=5.14.0"
         "perl-Authen-SASL"
         "perl-libwww"
         "perl-MIME-tools"
         "perl-Net-SMTP-SSL"
         "perl-TermReadKey")

optdepends=("mintty")

source=("${_realname}"::"git+https://github.com/git-for-windows/git.git#tag=v$tag")

sha256sums=('SKIP')

options+=('strip')

pkgver() {
    cd "$srcdir/git"
    basever=${tag%.windows.*}
    test "$tag" = "$basever" ||
    basever="$basever.${tag##*.windows.}"
    basever="${basever/-/.}"
    printf "%s.%s" "${basever}" "$(git rev-parse --short HEAD)"
}

build() {
    pushd "$srcdir/git"
        if test false != "$(git config core.autocrlf)"
            then
            git config core.autocrlf false
            rm .git/index
            git reset --hard
        fi
    popd

    # Git wants to decide itself whether to use ANSI stdio emulation or not
    CPPFLAGS="$(echo "$CPPFLAGS" |
        sed 's/-D__USE_MINGW_ANSI_STDIO\(=[0-9]*\)\?//')"

    [ -d ${srcdir}/build ] || mkdir ${srcdir}/build
    cd ${srcdir}/build
    pkgdir_git=${pkgdir}/../"${MINGW_PACKAGE_PREFIX}-${_realname}"/${MINGW_PREFIX}
    ${MINGW_PREFIX}/bin/cmake.exe ../git/contrib/buildsystems -GNinja -DUSE_VCPKG=off -DCMAKE_INSTALL_PREFIX=${pkgdir_git}
    ${MINGW_PREFIX}/bin/ninja.exe
}

package_git () {
    cd ${srcdir}/build
    ${MINGW_PREFIX}/bin/ninja.exe install
}

package_git-doc-html () {
    cd "$srcdir"/git

    make prefix="$pkgdir/$MINGW_PREFIX" install-html

    # subtree, for backwards-compatibility with MSys2's Git package
    make -C contrib/subtree prefix="$pkgdir/$MINGW_PREFIX" install-html
}

package_git-doc-man () {
    cd "$srcdir"/git

    make prefix="$pkgdir/$MINGW_PREFIX" install-man

    # subtree, for backwards-compatibility with MSys2's Git package
    make -C contrib/subtree prefix="$pkgdir/$MINGW_PREFIX" install-man
}

package_mingw-w64-i686-git () {
    package_git
}

package_mingw-w64-i686-git-doc-html () {
    package_git-doc-html
}

package_mingw-w64-i686-git-doc-man () {
    package_git-doc-man
}

package_mingw-w64-x86_64-git () {
    package_git
}

package_mingw-w64-x86_64-git-doc-html () {
    package_git-doc-html
}

package_mingw-w64-x86_64-git-doc-man () {
    package_git-doc-man
}

package_mingw-w64-ucrt-x86_64-git () {
    package_git
}

package_mingw-w64-ucrt-x86_64-git-doc-html () {
    package_git-doc-html
}

package_mingw-w64-ucrt-x86_64-git-doc-man () {
    package_git-doc-man
}
