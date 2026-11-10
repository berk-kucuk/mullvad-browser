# Maintainer: Berk Kucuk <dev.berkkucukk@gmail.com>

pkgname=mullvad-browser
pkgver=16.0.1
pkgrel=2
pkgdesc='Privacy-focused web browser developed by Mullvad VPN and the Tor Project'
arch=('x86_64')
url='https://mullvad.net/browser'
license=('MPL-2.0')
depends=('gtk3' 'libxt' 'dbus-glib' 'alsa-lib' 'hicolor-icon-theme')
optdepends=('pulse-native-provider: audio support'
            'ffmpeg: H.264 video support')
provides=('mullvad-browser')
conflicts=('mullvad-browser-bin')
options=('!strip' '!debug')
source=("https://cdn.mullvad.net/browser/${pkgver}/${pkgname}-linux-${arch[0]}-${pkgver}.tar.xz"{,.asc}
        "${pkgname}.sh"
        "${pkgname}.desktop"
        "policies.json")
sha256sums=('c1760c918232a2a20bb91ad2fa23ca1f6639af3a4e91f774cbab0589d6c22446'
            'SKIP'
            '208f848e18d8e57972d6c6480f17ed4db4415ff59c190377e85b945fbb6feed5'
            'be68773ea965ab55f196f08041e73216bca36c8da70140f84c20b65ca56767da'
            'e977c3d690224dd727d8761d2c0a66ba41bfd02ae0fd7850c4545d06b5a8e63c')
# Tor Browser Developers (signing key)
validpgpkeys=('EF6E286DDA85EA2A4BA7DE684E2C6E8793298290')

package() {
  install -d "${pkgdir}/opt"
  cp -a "${srcdir}/${pkgname}/Browser" "${pkgdir}/opt/${pkgname}"
  # Upstream ships Browser/ as 0700, make it usable by all users
  chmod -R u+rwX,go+rX,go-w "${pkgdir}/opt/${pkgname}"

  # Profile lives in ~/.mullvad-browser instead of the install dir
  touch "${pkgdir}/opt/${pkgname}/is-packaged-app"

  # Updates are handled by pacman
  install -Dm644 "${srcdir}/policies.json" "${pkgdir}/opt/${pkgname}/distribution/policies.json"
  rm -f "${pkgdir}/opt/${pkgname}/updater" "${pkgdir}/opt/${pkgname}/updater.ini" \
        "${pkgdir}/opt/${pkgname}/start-mullvad-browser.desktop"

  install -Dm755 "${srcdir}/${pkgname}.sh" "${pkgdir}/usr/bin/${pkgname}"
  install -Dm644 "${srcdir}/${pkgname}.desktop" "${pkgdir}/usr/share/applications/${pkgname}.desktop"

  for i in 16 32 48 64 128; do
    install -Dm644 "${pkgdir}/opt/${pkgname}/browser/chrome/icons/default/default${i}.png" \
      "${pkgdir}/usr/share/icons/hicolor/${i}x${i}/apps/${pkgname}.png"
  done

  install -d "${pkgdir}/usr/share/licenses/${pkgname}"
  cp -a "${pkgdir}/opt/${pkgname}/MullvadBrowser/Docs/Licenses/." "${pkgdir}/usr/share/licenses/${pkgname}/"
}
