# SPDX-License-Identifier: AGPL-3.0
#
# Contributor: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Contributor: Truocolo <truocolo@aol.com>

_py="python"
_pkgname="gobject-introspection"
pkgbase="${_pkgname}-git"
pkgname=(
  "${_pkgname}-git"
  "${_pkgname}-runtime-git"
)
pkgver=1.64.0+75+g8b0a7f4c
pkgrel=1
pkgdesc="Introspection system for GObject-based libraries"
url="https://wiki.gnome.org/Projects/GObjectIntrospection"
_ns="GNOME"
_http="https://gitlab.gnome.org"
_url="${_http}/${_ns}/${_pkgname}"
arch=(
  x86_64
  arm
  aarch64
  powerpc
  pentium4
  i686
  armv7h)
license=(
  LGPL
  GPL)
depends=(
  "${_py}-mako"
  "${_py}-markdown"
)
makedepends=(
  cairo
  git
  gtk-doc
  "${_py}-sphinx"
  meson
  glib2)
provides=(
  "${_pkgname}=${pkgver}")
conflicts=(
  "${_pkgname}")
options=(
  !emptydirs)
source=(
  "git+${_url}.git"
  "git+${_http}/${_ns}/glib.git")
sha512sums=(
  'SKIP'
  'SKIP')
validpgpkeys=(
  # Philip Withnall <philip@tecnocode.co.uk>
  '923B7025EE03C1C59F42684CF0942E894B2EAFA0')

pkgver() {
  cd \
    "${_pkgname}"
  git \
    describe \
    --tags | \
    sed \
      's/-/+/g'
}

_meson_options=(
  -D gtk_doc=false
  -D glib_src_dir="${srcdir}/glib"
)

build() {
  local \
    _cflags=()
  _cflags=(
    "-I$( \
       dirname \
         "$(cc \
              -v 2>&1 |
              grep \
                "InstalledDir" | \
                awk '{print $2}')")/include/glib-2.0"
    "${CFLAGS}"
  )
  CFLAGS="${_cflags[*]}" \
    arch-meson \
      "${_pkgname}" \
      build \
      "${_meson_options[@]}"
  ninja \
    -C build
}

check() {
  meson \
    test \
    -C build
}

package_gobject-introspection-git() {
  depends+=("gobject-introspection-runtime")

  DESTDIR="${pkgdir}" \
    meson \
      install \
      -C build

  python \
    -m compileall \
    -d "/usr/lib/${_pkgname}" \
    "${pkgdir}/usr/lib/${_pkgname}"
  python \
    -O \
    -m compileall \
    -d "/usr/lib/${_pkgname}" \
    "${pkgdir}/usr/lib/${_pkgname}"

### Split runtime
  mkdir \
    -p \
    "${srcdir}/runtime/lib"
  mv \
    "${pkgdir}/usr/lib/"{lib*,girepository-*} \
    "${srcdir}/runtime/lib"
}

package_gobject-introspection-runtime-git() {
  pkgdesc+=" (runtime library)"
  depends=(
    glib2)
  provides=(
    "${_pkgname}-runtime=${pkgver}"
    "libgirepository-1.0.so")
  conflicts=(
    "${_pkgname}-runtime")

  mv \
    "${srcdir}/runtime" \
    "${pkgdir}/usr"
}

# vim:set sw=2 sts=-1 et:
