# Maintainer: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Florian Walch <florian+aur@fwalch.com>
# Contributor: Florian Hahn <flo@fhahn.com>

pkgname=neovim
pkgver=0.1.6
pkgrel=1
pkgdesc='Fork of Vim aiming to improve user experience, plugins, and GUIs'
arch=('i686' 'x86_64')
url='https://neovim.io'
license=('custom:neovim')
depends=('jemalloc' 'libtermkey' 'libuv' 'msgpack-c' 'unibilium' 'libvterm' 'gperf')
makedepends=('cmake' 'luajit' 'lua51-mpack' 'lua51-lpeg')
optdepends=('python2-neovim: for Python 2 plugin support (see :help nvim-python)'
            'python-neovim: for Python 3 plugin support (see :help nvim-python)'
            'xclip: for clipboard support (or xsel) (see :help nvim-clipboard)'
            'xsel: for clipboard support (or xclip) (see :help nvim-clipboard)')
source=("https://github.com/neovim/neovim/archive/v${pkgver}.tar.gz")
sha256sums=('a9fe7aadd38ef015f82ec340f6b6c0629d02c9ca4d85352db0934ae511d2f02a')
install=neovim.install

build() {
  mkdir -p "${srcdir}/build"
  cd "${srcdir}/build"

  cmake "../neovim-${pkgver}" \
        -DCMAKE_BUILD_TYPE=RelWithDebInfo \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DENABLE_JEMALLOC=ON
  make
}

check() {
  cd "${srcdir}/build"
  ./bin/nvim --version
  ./bin/nvim --headless -u NONE -i NONE -c ':quit'
}

package() {
  cd "${srcdir}/build"
  make DESTDIR="${pkgdir}" install
  install -Dm644 "${srcdir}/neovim-${pkgver}"/LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

# vim:set sw=2 sts=2 et:
