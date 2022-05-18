# Maintainer: Berrysoft <Strawberry_Str@hotail.com>

_realname=git
pkgbase="mingw-w64-${_realname}"
pkgname=("${MINGW_PACKAGE_PREFIX}-${_realname}")
tag=2.36.1.windows.1
pkgver=2.36.1.1.e2ff68a2d1
pkgrel=1
pkgdesc="The fast distributed version control system (mingw-w64)"
arch=('any')
url="https://git-for-windows.github.io/"
license=('GPL2')

options=('strip')

makedepends=("docbook-xsl"
             "git"
             "make"
             "patch"
             "tar"
             "xmlto"
             "${MINGW_PACKAGE_PREFIX}-asciidoc"
             "${MINGW_PACKAGE_PREFIX}-cc"
             "${MINGW_PACKAGE_PREFIX}-cmake"
             "${MINGW_PACKAGE_PREFIX}-ninja"
             "${MINGW_PACKAGE_PREFIX}-pkgconf")

depends=("${MINGW_PACKAGE_PREFIX}-curl"
         "${MINGW_PACKAGE_PREFIX}-ca-certificates"
         "${MINGW_PACKAGE_PREFIX}-expat>=2.0"
         "${MINGW_PACKAGE_PREFIX}-gettext"
         "${MINGW_PACKAGE_PREFIX}-libiconv"
         "${MINGW_PACKAGE_PREFIX}-openssl"
         "${MINGW_PACKAGE_PREFIX}-pcre2"
         "${MINGW_PACKAGE_PREFIX}-tcl"
         "${MINGW_PACKAGE_PREFIX}-tk"
         "${MINGW_PACKAGE_PREFIX}-zlib"
         "perl>=5.14.0"
         "perl-Authen-SASL"
         "perl-Error"
         "perl-libwww"
         "perl-MIME-tools"
         "perl-Net-SMTP-SSL"
         "perl-TermReadKey")

optdepends=("mintty")

conflicts=("${MINGW_PACKAGE_PREFIX}-${_realname}-doc-html"
	       "${MINGW_PACKAGE_PREFIX}-${_realname}-doc-man")

source=("${_realname}"::"git+https://github.com/git-for-windows/git.git#tag=v$tag"
        "1-fallback-path.patch"
        "2-mingw-clang.patch"
        "3-pcre2.patch")

sha256sums=('SKIP'
            '014035f317ca89d15790b114301529bcbcea1110fc1287b571a4cb475cd8b649'
            '6005df74de976731bda3ff8d04416b5f7cd7d1b9f97a8fc8714c790f6d48a6b9'
            'af2cdf0b91764c1b5c4428681b08a843a2883dfaafa436b57638edc67c4e3efc')

pkgver() {
    cd "$srcdir/git"
    basever=${tag%.windows.*}
    test "$tag" = "$basever" ||
    basever="$basever.${tag##*.windows.}"
    basever="${basever/-/.}"
    printf "%s.%s" "${basever}" "$(git rev-parse --short HEAD)"
}

prepare() {
    cd "$srcdir/git"
    if test false != "$(git config core.autocrlf)"
        then
        git config core.autocrlf false
        rm .git/index
        git reset --hard
    fi
    git apply ${srcdir}/*.patch
}

package () {
    cd "$srcdir"/git

    make -C gitk-git prefix="$pkgdir/$MINGW_PREFIX" install

    DESTDIR="$pkgdir" make -C git-gui gitexecdir="$MINGW_PREFIX/libexec/git-core" install

    DESTDIR="$pkgdir" make -C gitweb prefix="$MINGW_PREFIX" gitwebdir="$MINGW_PREFIX/var/www/cgi-bin" install

    make prefix="$pkgdir/$MINGW_PREFIX" install-html
    make prefix="$pkgdir/$MINGW_PREFIX" install-man

    # subtree, for backwards-compatibility with MSys2's Git package
    make -C contrib/subtree prefix="$pkgdir/$MINGW_PREFIX" install
    make -C contrib/subtree prefix="$pkgdir/$MINGW_PREFIX" install-html
    make -C contrib/subtree prefix="$pkgdir/$MINGW_PREFIX" install-man

    # completions
    install -d "$pkgdir/$MINGW_PREFIX/share/git/completion/"
    install contrib/completion/* "$pkgdir/$MINGW_PREFIX/share/git/completion/"

    # Git wants to decide itself whether to use ANSI stdio emulation or not
    CPPFLAGS="$(echo "$CPPFLAGS" |
        sed 's/-D__USE_MINGW_ANSI_STDIO\(=[0-9]*\)\?//')"

    [ -d ${srcdir}/build ] || mkdir ${srcdir}/build
    cd ${srcdir}/build
    MSYS2_ARG_CONV_EXCL="-DFALLBACK_RUNTIME_PREFIX=" ${MINGW_PREFIX}/bin/cmake.exe ${srcdir}/git/contrib/buildsystems \
        -GNinja \
        -DUSE_VCPKG=off \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/$MINGW_PREFIX" \
        -DFALLBACK_RUNTIME_PREFIX="$MINGW_PREFIX"
    ${MINGW_PREFIX}/bin/ninja.exe install
}
