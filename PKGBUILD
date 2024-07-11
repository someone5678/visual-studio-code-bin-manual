# Maintainer: D. Can Celasun <can[at]dcc[dot]im>

pkgname=visual-studio-code-bin-manual
pkgname_build=visual-studio-code-bin
_pkgname=visual-studio-code
pkgver=1.96.3
pkgrel=1
pkgdesc="Visual Studio Code (vscode): Editor for building and debugging modern web and cloud applications (official binary version)"
arch=('x86_64' 'aarch64' 'armv7h')
url="https://code.visualstudio.com/"
license=('custom: commercial')
provides=('code' 'vscode' 'visual-studio-code-bin')
conflicts=('code')
install=$pkgname_build.install
# lsof: needed for terminal splitting, see https://github.com/Microsoft/vscode/issues/62991
# xdg-utils: needed for opening web links with xdg-open
depends=(libxkbfile gnupg gtk3 libsecret nss gcc-libs libnotify libxss glibc lsof shared-mime-info xdg-utils alsa-lib)
optdepends=('glib2: Needed for move to trash functionality'
            'libdbusmenu-glib: Needed for KDE global menu'
            'org.freedesktop.secrets: Needed for settings sync'
             # See https://github.com/MicrosoftDocs/live-share/issues/4650
            'icu69: Needed for live share' )
source=(code.desktop.in::https://raw.githubusercontent.com/microsoft/vscode/${pkgver}/resources/linux/code.desktop
        code-url-handler.desktop.in::https://raw.githubusercontent.com/microsoft/vscode/${pkgver}/resources/linux/code-url-handler.desktop
        code-workspace.xml.in::https://raw.githubusercontent.com/microsoft/vscode/${pkgver}/resources/linux/code-workspace.xml
        ${_pkgname}-bin.sh)
source_x86_64=(code_x64_${pkgver}.tar.gz::https://update.code.visualstudio.com/${pkgver}/linux-x64/stable)
source_aarch64=(code_arm64_${pkgver}.tar.gz::https://update.code.visualstudio.com/${pkgver}/linux-arm64/stable)
source_armv7h=(code_armhf_${pkgver}.tar.gz::https://update.code.visualstudio.com/${pkgver}/linux-armhf/stable)

sha256sums=('SKIP'
            'SKIP'
            'SKIP'
            'SKIP')
sha256sums_x86_64=('SKIP')
sha256sums_aarch64=('SKIP')
sha256sums_armv7h=('SKIP')

_set_meta_info() {
  sed 's/@@NAME_LONG@@/Visual Studio Code/g' "$1" |\
    sed 's/@@NAME_SHORT@@/Code/g' |\
    sed 's/@@NAME@@/code/g' |\
    sed 's#@@EXEC@@#/usr/bin/code#g' |\
    sed 's/@@ICON@@/visual-studio-code/g' |\
    sed 's/@@URLPROTOCOL@@/vscode/g'
}

_pkg() {
  if [ "${CARCH}" = "aarch64" ]; then
    echo 'VSCode-linux-arm64'
  elif [ "${CARCH}" = "armv7h" ]; then
    echo 'VSCode-linux-armhf'
  elif [ "${CARCH}" = "i686" ]; then
    echo 'VSCode-linux-ia32'
  else
    echo 'VSCode-linux-x64'
  fi
}

prepare() {
  _set_meta_info "${srcdir}/code.desktop.in" > "${srcdir}/code.desktop"
  _set_meta_info "${srcdir}/code-url-handler.desktop.in" > "${srcdir}/code-url-handler.desktop"
  _set_meta_info "${srcdir}/code-workspace.xml.in" > "${srcdir}/code-workspace.xml"
}

package() {
  _pkg=VSCode-linux-x64
  if [ "${CARCH}" = "aarch64" ]; then
    _pkg=VSCode-linux-arm64
  fi
  if [ "${CARCH}" = "armv7h" ]; then
    _pkg=VSCode-linux-armhf
  fi
  if [ "${CARCH}" = "i686" ]; then
    _pkg=VSCode-linux-ia32
  fi

  install -d "${pkgdir}/usr/share/licenses/${_pkgname}"
  install -d "${pkgdir}/opt/${_pkgname}"
  install -d "${pkgdir}/usr/bin"
  install -d "${pkgdir}/usr/share/applications"
  install -d "${pkgdir}/usr/share/icons"
  install -d "${pkgdir}/usr/share/mime/packages"

  install -m644 "${srcdir}/$(_pkg)/resources/app/LICENSE.rtf" "${pkgdir}/usr/share/licenses/${_pkgname}/LICENSE.rtf"
  install -m644 "${srcdir}/$(_pkg)/resources/app/resources/linux/code.png" "${pkgdir}/usr/share/icons/${_pkgname}.png"
  install -m644 "${srcdir}/code.desktop" "${pkgdir}/usr/share/applications/code.desktop"
  install -m644 "${srcdir}/code-url-handler.desktop" "${pkgdir}/usr/share/applications/code-url-handler.desktop"
  install -m644 "${srcdir}/code-workspace.xml" "${pkgdir}/usr/share/mime/packages/${pkgname_build}-workspace.xml"
  install -Dm 644 "${srcdir}/$(_pkg)/resources/completions/bash/code" "${pkgdir}/usr/share/bash-completion/completions/code"
  install -Dm 644 "${srcdir}/$(_pkg)/resources/completions/zsh/_code" "${pkgdir}/usr/share/zsh/site-functions/_code"

  cp -r "${srcdir}/$(_pkg)/"* "${pkgdir}/opt/${_pkgname}"

  # Launcher
	install -m755 "${srcdir}/${_pkgname}-bin.sh" "${pkgdir}/usr/bin/code"
}

