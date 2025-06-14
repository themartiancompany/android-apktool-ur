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

# Maintainer:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer:
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Contributor:
#   Muflone
#     <http://www.muflone.com/contacts/english>
# Contributor:
#   navigaid
#     <navigaid@gmail.com>

_java_ver_auto_detect() {
  local \
    _ver_rev
  _ver_rev="$( \
    pacman \
      -Qs \
      openjdk | \
      grep \
        "local/openjdk-" | \
	head \
	  -n \
	    1 | \
          awk \
            '{print $2}')"
  _java_ver="${_ver_rev%-*}"
  _java_majver="${_java_ver%%.*}"
}
_os="$( \
  uname \
    -o)"
if [[ "${_os}" == "Android" ]]; then
  _java_majver="21"
  _java_ver_auto_detect
  # Termux should really add a provide
  # or make a meta-package.
  # Or me should, I don't know.
  _java="openjdk-${_java_majver}"
  _build="false"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _java="java-environment"
  _build="true"
fi
_pkg="apktool"
_Pkg="Apktool"
pkgname="android-${_pkg}"
pkgver=2.11.1
pkgrel=1
_pkgdesc=(
  "A tool for reengineering"
  "Android apk files."
)
pkgdesc="${_pkgdesc[*]}"
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
if [[ "${_os}" == "Android" ]]; then
  depends+=(
    "aapt2"
  )
fi
makedepends=(
  "${_java}"
  # Be careful as gradle 9
  # may be incompatible
  # "gradle<9"
  'gradle'
)
_tarname="${_Pkg}-${pkgver}"
_tmc_ns="themartiancompany"
_apktool_commit="82b3a56eb9deeba118a738149177abc54970b546"
_fur_url="${_http}/${_tmc_ns}/fur/raw/${_apktool_commit}"
_url="${_fur_url}/arch/any/${pkgname}-${pkgver}-${pkgrel}-any.pkg.tar.xz"
if [[ "${_os}" == "GNU/Linux" ]]; then
  _src="${_tarname}.tar.gz::${url}/archive/refs/tags/v${pkgver}.tar.gz"
  _sum='1c1ac3add61c5d9043b5efdb228fbd2be7c3bd329bb6ed82228eedacef90bcb9'
elif [[ "${_os}" == "Android" ]]; then
  _src="${_tarname}.tar.xz::${_url}"
  _sum="6eac3efff81c062133078311de478b0bbce2831c374c0f483d5c86ce7b01c231"
fi
source=(
  "${_src}"
)
sha256sums=(
  "${_sum}"
)

_usr_get() {
  local \
    _bin
  _bin="$( \
    dirname \
      "$(command \
           -v \
	   "env")")"
  dirname \
    "${_bin}"
}

build() {
  local \
    _usr
  _usr="$( \
    _usr_get)"
  export \
    JAVA_HOME="${_usr}/usr/lib/jvm/default"
  if [[ "${_build}" == "true" ]]; then
    cd \
      "${_tarname}"
    gradle \
      build \
        --no-daemon \
        shadowJar \
        proguard
  fi
}

package() {
  local \
    _usr
  _usr="$( \
    _usr_get)"
  if [[ "${_build}" == "false" ]]; then
    ls
    cp \
      -r \
      "usr" \
      "${pkgdir}"
    rm \
      "${pkgdir}/usr/bin/${_pkg}"
  elif [[ "${_build}" == "true" ]]; then
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
  fi
  ln \
    -s \
    "${_usr}/share/${pkgname}/${_pkg}" \
    "${pkgdir}/usr/bin/${_pkg}"
}
