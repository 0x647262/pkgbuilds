# Maintainer: Sven-Hendrik Haase <svenstaro@archlinux.org>
# Maintainer: Caleb Maclennan <caleb@alerque.com>
# Contributor: Florian Walch <florian+aur@fwalch.com>
# Contributor: Florian Hahn <flo@fhahn.com>

pkgname=neovim
pkgver=0.7.0
pkgrel=1
pkgdesc='Fork of Vim aiming to improve user experience, plugins, and GUIs'
arch=('x86_64')
url='https://neovim.io'
backup=('etc/xdg/nvim/sysinit.vim')
license=('custom:neovim')
provides=('vim-plugin-runtime')
depends=('libtermkey' 'libuv' 'msgpack-c' 'unibilium' 'libvterm01' 'luajit' 'libluv' 'tree-sitter')
makedepends=('cmake' 'ninja' 'lua51-mpack' 'lua51-lpeg' 'gperf')
optdepends=('python-neovim: for Python 3 plugin support (see :help python)'
            'xclip: for clipboard support on X11 (or xsel) (see :help clipboard)'
            'xsel: for clipboard support on X11 (or xclip) (see :help clipboard)'
            'wl-clipboard: for clipboard support on wayland (see :help clipboard)')
source=("https://github.com/neovim/neovim/archive/v${pkgver}/${pkgname}-${pkgver}.tar.gz")
sha512sums=('3597c54fb925a4d607bca9ba0fdb37df90ecb816da99f52baf46cc2ec79727a55048ba1d8d22c8e7d61f0e8e35546326b1d0d15c0a91de8bf5bc529c45fb1ce0')

build() {
  cd neovim-${pkgver}
  cmake \
    -Bbuild \
    -GNinja \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DUSE_BUNDLED=OFF
  cmake --build build
}

check() {
  cd neovim-${pkgver}/build
  ./bin/nvim --version
  ./bin/nvim --headless -u NONE -i NONE -c ':quit'
}

package() {
  cd neovim-${pkgver}
  DESTDIR="${pkgdir}" cmake --install build

  cd "${srcdir}/neovim-${pkgver}"
  install -Dm644 LICENSE.txt "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.txt"
  install -Dm644 runtime/nvim.desktop "${pkgdir}/usr/share/applications/nvim.desktop"
  install -Dm644 runtime/nvim.appdata.xml "${pkgdir}/usr/share/metainfo/nvim.appdata.xml"
  install -Dm644 runtime/nvim.png "${pkgdir}/usr/share/pixmaps/nvim.png"

  # Make Arch vim packages work
  mkdir -p "${pkgdir}"/etc/xdg/nvim
  echo "\" This line makes pacman-installed global Arch Linux vim packages work." > "${pkgdir}"/etc/xdg/nvim/sysinit.vim
  echo "source /usr/share/nvim/archlinux.vim" >> "${pkgdir}"/etc/xdg/nvim/sysinit.vim

  mkdir -p "${pkgdir}"/usr/share/vim
  echo "set runtimepath+=/usr/share/vim/vimfiles" > "${pkgdir}"/usr/share/nvim/archlinux.vim
}

# vim:set ft=sh sw=2 sts=2 et:
