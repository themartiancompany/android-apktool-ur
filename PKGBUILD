# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Truocolo <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Maintainer: Pellegrino Prevete (dvorak) <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Maintainer: Muflone http://www.muflone.com/contacts/english/
# Contributor: navigaid <navigaid@gmail.com>

_java_ver_auto_detect() {
  local \
    _ver_rev
  _ver_rev="$( \
    pacman \
      -Qs \
      openjdk | \
      grep \
        "local/openjdk-" | \
        awk \
          '{print $2}')"
  _java_ver="${_ver_rev%-*}"
}
_os="$( \
  uname \
    -o)"
if [[ "${_os}" == "Android" ]]; then
  _java_ver="21"
  _java_ver_auto_detect
  # Termux should really add a provide
  # or make a meta-package.
  # Or me should, I don't know.
  _java="opendjk-${_java_ver}"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _java="java-environment"
fi
_pkg="apktool"
_Pkg="Apktool"
pkgname="android-${_pkg}"
pkgver=2.11.1
pkgrel=1
_pkgdesc=(
  "A tool for reengineering"
  "Android apk files"
)
arch=(
  'any'
)
_http="https://github.com"
_ns="iBotPeaches"
url="${_http}/${_ns}/${_Pkg}"
license=(
  'Apache-2.0'
)
depends=(
  "${_java}"
)
makedepends=(
  "${_java}"
  'gradle'
)
_tarname="${_Pkg}-${pkgver}"
source=(
  "${_tarname}.tar.gz::${url}/archive/refs/tags/v${pkgver}.tar.gz"
)
sha256sums=(
  '1c1ac3add61c5d9043b5efdb228fbd2be7c3bd329bb6ed82228eedacef90bcb9')

build() {
  cd \
    "${_tarname}"
  if [[ "${_os}" == "GNU/Linux" ]]; then
    export \
      JAVA_HOME="/usr/lib/jvm/default"
  fi
  gradle \
    build \
      --no-daemon \
      shadowJar \
      proguard
}

package() {
  cd \
    "${_tarname}"
  install \
    -Dm644 \
    "brut.${_pkg}/${_pkg}-cli/build/libs/${_pkg}-cli.jar" \
    "${pkgdir}/usr/share/${pkgname}/${_pkg}.jar"
  install \
    -Dm755 \
    "scripts/linux/${_pkg}" \
    "${pkgdir}/usr/share/${pkgname}/${_pkg}"
  install \
    -dm755 \
    "${pkgdir}/usr/bin"
  ln \
    -s \
    "/usr/share/${pkgname}/${_pkg}" \
    "${pkgdir}/usr/bin/${_pkg}"
}
