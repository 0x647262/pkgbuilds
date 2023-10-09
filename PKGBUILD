# Maintainer: Sven-Hendrik Haase <svenstaro@archlinux.org>
# Maintainer: Caleb Maclennan <caleb@alerque.com>
# Contributor: Florian Walch <florian+aur@fwalch.com>
# Contributor: Florian Hahn <flo@fhahn.com>

pkgname=neovim
pkgver=0.9.4
pkgrel=1
pkgdesc='Fork of Vim aiming to improve user experience, plugins, and GUIs'
arch=('x86_64')
url='https://neovim.io'
backup=('etc/xdg/nvim/sysinit.vim')
license=('custom:neovim')
provides=('vim-plugin-runtime')
depends=('libtermkey' 'libuv' 'msgpack-c' 'unibilium' 'libvterm>0.1.4' 'luajit' 'libluv' 'tree-sitter')
makedepends=('cmake' 'git' 'ninja' 'lua51-mpack' 'lua51-lpeg' 'unzip')
optdepends=('python-pynvim: for Python plugin support (see :help python)'
            'xclip: for clipboard support on X11 (or xsel) (see :help clipboard)'
            'xsel: for clipboard support on X11 (or xclip) (see :help clipboard)'
            'wl-clipboard: for clipboard support on wayland (see :help clipboard)')
source=("https://github.com/neovim/neovim/archive/v${pkgver}/${pkgname}-${pkgver}.tar.gz")
sha512sums=('a9bac18aeecd99dfeab79b367c3f0c46003b95d057edb6fd18ba178d6b6f22434689508d0bfe91b2f771ef0a23a4888815e8c4001abb76f2a60357bab0cd7004')
b2sums=('f98b9737df537be9a6f9bfba0e48f47f33cacdf5aa5f9fb3b47a693ea9fa5fbe32aa8628403fdb136b625ccad30c8aad1c25abe280384515df603e92d9ed898a')

build() {
  cd ${pkgname}-${pkgver}
  cmake \
    -Bbuild \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DUSE_BUNDLED=OFF \
    -W no-dev
  cmake --build build --verbose
}

check() {
  cd ${pkgname}-${pkgver}
  ./build/bin/nvim --version
  ./build/bin/nvim --headless -u NONE -i NONE -c ':quit'
}

package() {
  cd ${pkgname}-${pkgver}
  DESTDIR="$pkgdir" cmake --install build

  install -Dm644 LICENSE.txt -t "${pkgdir}/usr/share/licenses/${pkgname}/"
  install -Dm644 runtime/nvim.desktop -t "${pkgdir}/usr/share/applications/"
  install -Dm644 runtime/nvim.appdata.xml -t "${pkgdir}/usr/share/metainfo/"
  install -Dm644 runtime/nvim.png -t "${pkgdir}/usr/share/pixmaps/"

  # Make Arch vim packages work
  mkdir -p "${pkgdir}"/etc/xdg/nvim
  echo "\" This line makes pacman-installed global Arch Linux vim packages work." > "${pkgdir}"/etc/xdg/nvim/sysinit.vim
  echo "source /usr/share/nvim/archlinux.vim" >> "${pkgdir}"/etc/xdg/nvim/sysinit.vim

  mkdir -p "${pkgdir}"/usr/share/vim
  echo "set runtimepath+=/usr/share/vim/vimfiles" > "${pkgdir}"/usr/share/nvim/archlinux.vim
}

# vim:set sw=2 sts=2 et:
