# Based  on the template from https://daveparrish.net/posts/2019-11-16-Better-AppImage-PKGBUILD-template.html
# Maintainer: Luke Arms <luke@arms.to>
# Contributor: Tianrui Wei <archlinux_aur at mail dot tianrui-wei dot com>
# Contributor: Marcio Silva <marcionps at gmail dot com>

_pkgname=todoist

pkgname=${_pkgname}-appimage
pkgver=8.3.2
pkgrel=2
pkgdesc="A To-Do List to Organize Your Work & Life"
arch=('x86_64')
url='https://todoist.com/'
license=('custom')
depends=('zlib' 'appimagelauncher')
options=('!strip')
_appimage=${_pkgname}-${pkgver}.AppImage
source=("${_appimage}::https://electron-dl.todoist.com/linux/Todoist-linux-x86_64-${pkgver}.AppImage")
noextract=("${_appimage}")
sha256sums=('01292393f813af06fb6bdf45717c24b9ef03b39f56bf38be6049a68e21aabf6a')

prepare() {
    cd "${srcdir}"
    chmod +x "${_appimage}"
    "./${_appimage}" --appimage-extract
}

build() {
    cd "${srcdir}"
    # Adjust .desktop so it will work outside of AppImage container
    sed -i -E "s|Exec=AppRun|Exec=env DESKTOPINTEGRATION=false ${_pkgname}|" \
        "squashfs-root/${_pkgname}.desktop"
    # Fix permissions; .AppImage permissions are 700 for all directories
    chmod -R a-x+rX squashfs-root/usr
}

package() {
    # AppImage
    install -Dm755 "${srcdir}/${_appimage}" \
        "${pkgdir}/opt/${pkgname}/${_pkgname}.AppImage"

    # Desktop file
    install -Dm644 "${srcdir}/squashfs-root/${_pkgname}.desktop" \
        "${pkgdir}/usr/share/applications/${_pkgname}.desktop"

    # Icon
    install -dm755 "${pkgdir}/usr/share"
    cp -dR "${srcdir}/squashfs-root/usr/share/icons" \
        "${pkgdir}/usr/share/"
    install -Dm644 "${srcdir}/squashfs-root/usr/share/icons/hicolor/512x512/apps/todoist.png" \
        "${pkgdir}/usr/share/pixmaps/todoist.png"

    # Symlink executable
    install -dm755 "${pkgdir}/usr/bin"
    ln -s "/opt/${pkgname}/${_pkgname}.AppImage" "${pkgdir}/usr/bin/${_pkgname}"
}
