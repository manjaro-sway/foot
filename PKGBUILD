# Maintainer: Daniel Eklöf <daniel at ekloef dot se>

# Select PGO (Performance Guided Optimizations) build type.
#
#  - auto: choose best available option
#
#  - none: disable PGO
#
#  - full-current-session: run a “full” PGO build in an existing
#    Wayland session. This will pop up a foot window running a script
#    that generates random terminal output.
#
#  - full-headless-sway: run a “full” PGO build inside a headless Sway
#    instance. Requires Sway >= 1.7.
#
#  - full-headless-cage: run a “full” PGO build inside a headless Cage
#    instance. Requires cage to be installed. Will generate lots of
#    Cage warnings, but seems to produce a fully working (and well
#    optimized) foot build.
#
#  - partial: run a “partial” PGO build. This requires neither a
#    running Wayland session, nor an installed Wayland compositor, but
#    the resulting binary is slower compared to “full” PGO builds
#    (though still faster than regular release builds).
#
# Note that “full-*” (which “auto” will prefer) requires an UTF-8
# locale. Either make sure LC_CTYPE is set to an UTF-8 locale, or do a
# “partial” PGO build (or disable PGO altoghether).

PGO=auto

pkgdesc="Wayland terminal emulator - fast, lightweight and minimalistic"
pkgname=(foot foot-themes)
pkgver=1.10.3  # Don’t forget to update CHANGELOG.md
pkgrel=1
arch=('x86_64' 'aarch64')
url=https://codeberg.org/dnkl/foot
license=(mit)
changelog=CHANGELOG.md
depends=('libxkbcommon' 'wayland' 'pixman' 'fontconfig' 'libutf8proc' 'ncurses' 'fcft')
makedepends=('meson' 'ninja' 'scdoc' 'python' 'wayland-protocols' 'tllist')  # ‘llvm’, for PGO with clang
checkdepends=('check')
optdepends=("foot-terminfo: alternative to ncurses' terminfo, with additional non-standard capabilities"
            "foot-themes: color schemes"
            "libnotify: desktop notifications"
            "xdg-utils: URI launching"
            "bash-completion: bash completions for foot itself")
source=(${pkgname}-${pkgver}.tar.gz::${url}/archive/${pkgver}.tar.gz)
sha256sums=('24f57d9926ead7bea491f1bdd97eaceae5fdc10c1cb3435ee588a8f9c9af805a')

build() {
  cd foot
  ./pgo/pgo.sh \
    ${PGO} \
    . build \
    --prefix=/usr \
    --wrap-mode=nodownload \
    -Dterminfo=disabled \
    -Dthemes=false
}

check() {
  cd foot
  ninja -C build test
}

package_foot() {
  cd foot
  DESTDIR="${pkgdir}/" ninja -C build install
  install -Dm 644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

package_foot-themes() {
  pkgdesc="Color schemes for the foot terminal emulator"
  depends=(foot)
  optdepends=()
  arch=(any)
  cd foot
  install -d "${pkgdir}/usr/share/foot/themes"
  install -m 644 themes/* "${pkgdir}/usr/share/foot/themes/"
}
