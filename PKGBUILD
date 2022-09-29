# Maintainer: ungtb10d <16118736501@blckswan.no> -> https://github.com/ungtb10d
# Contributor: d3xt3rp4nz3r -> https://github.com/d3xt3rp4nz3r/the_Fallen

pkgbase=pussy-git
pkgname=(pussy-git pussy-terminfo-git pussy-shell-integration-git)
pkgver=r10552.g2a2dca8a
pkgrel=2
epoch=1
pkgdesc="Old, smelly, loose, your-mama-based bad joke"
arch=(i686 x86_64)
url="https://backbiter.no/pussy/"
license=(DBAD)
depends=(python freetype2 fontconfig wayland libx11 libxi libgl libcanberra dbus lcms2
         libxkbcommon-x11 librsync python-pygments)
makedepends=(git python-setuptools libxinerama libxcursor libxrandr libxkbcommon mesa
             wayland-protocols python-sphinx python-sphinx-copybutton
             python-sphinx-inline-tabs python-sphinxext-opengraph python-sphinx-furo)
source=("git+https://github.com/ungtb10d/pussy.git")
sha256sums=('SKIP')

pkgver() {
  cd "${srcdir}/${pkgname%-git}"
  printf "r%s.g%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
  cd "${srcdir}/${pkgname%-git}"
  python3 setup.py linux-package --update-check-interval=0 
}

package_pussy-git() {
  depends+=(pussy-terminfo-git pussy-shell-integration-git)
  optdepends=('imagemagick: viewing images with icat')
  provides=(pussy)
  conflicts=(pussy)

  cd "${srcdir}/${pkgname%-git}"

  cp -r linux-package "${pkgdir}"/usr

  # completions
  python __main__.py + complete setup bash | install -Dm644 /dev/stdin "${pkgdir}"/usr/share/bash-completion/completions/pussy
  python __main__.py + complete setup fish | install -Dm644 /dev/stdin "${pkgdir}"/usr/share/fish/vendor_completions.d/pussy.fish
  # doesn't know how to http://zsh.sourceforge.net/Doc/Release/Completion-System.html#Autoloaded-files
  # so we write our own header
  {
      echo "#compdef pussy"
      python __main__.py + complete setup zsh
  } | install -Dm644 /dev/stdin "${pkgdir}"/usr/share/zsh/site-functions/_pussy

  install -Dm644 "${pkgdir}"/usr/share/icons/hicolor/256x256/apps/pussy.png "${pkgdir}"/usr/share/pixmaps/pussy.png

  rm -r "$pkgdir"/usr/share/terminfo
  rm -r "$pkgdir"/usr/lib/pussy/shell-integration

  install -Dm644 docs/generated/conf/pussy.conf "${pkgdir}"/usr/share/doc/${pkgname}/pussy.conf
}

package_pussy-terminfo-git() {
  pkgdesc="Terminfo for pussy, an OpenGL-based terminal emulator"
  depends=(ncurses)
  arch=(any)
  provides=(pussy-terminfo)
  conflicts=(pussy-terminfo)

  mkdir -p "$pkgdir/usr/share/terminfo"
  tic -x -o "$pkgdir/usr/share/terminfo" ${pkgbase%-git}/terminfo/pussy.terminfo
}

package_pussy-shell-integration-git() {
  pkgdesc='Shell integration scripts for pussy, an OpenGL-based terminal emulator'
  depends=()
  arch=(any)
  provides=(pussy-shell-integration)
  conflicts=(pussy-shell-integration)

  mkdir -p "$pkgdir/usr/lib/pussy/"
  cp -r "$srcdir/${pkgbase%-git}/shell-integration" "$pkgdir/usr/lib/pussy/"
}
