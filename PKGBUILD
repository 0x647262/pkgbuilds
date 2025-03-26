# Maintainer: Sven-Hendrik Haase <svenstaro@archlinux.org>
# Maintainer: Caleb Maclennan <caleb@alerque.com>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>
# Contributor: Florian Walch <florian+aur@fwalch.com>
# Contributor: Florian Hahn <flo@fhahn.com>

pkgname=neovim
pkgver=0.11.0
pkgrel=2
pkgdesc='Fork of Vim aiming to improve user experience, plugins, and GUIs'
arch=(x86_64)
url=https://neovim.io
license=(
  Apache-2.0
  LicenseRef-vim
)
depends=(
  libluv
  libutf8proc
  libuv
  'libvterm>=0.3.3'
  lua51-lpeg
  luajit
  msgpack-c
  tree-sitter
  tree-sitter-c
  tree-sitter-lua
  tree-sitter-markdown
  tree-sitter-query
  tree-sitter-vim
  tree-sitter-vimdoc
  unibilium
)
makedepends=(
  cmake
  git
  ninja
  lua51-mpack
  lua51-lpeg
  unzip
)
optdepends=(
  'python-pynvim: for Python plugin support (see :help python)'
  'xclip: for clipboard support on X11 (or xsel) (see :help clipboard)'
  'xsel: for clipboard support on X11 (or xclip) (see :help clipboard)'
  'wl-clipboard: for clipboard support on wayland (see :help clipboard)'
)
provides=(
  nvim
  vim-plugin-runtime
)
backup=(etc/xdg/nvim/sysinit.vim)
source=(
  "git+https://github.com/neovim/neovim.git#tag=v$pkgver"
  nvimdoc{,.hook}
)
sha512sums=('ac6298c3d9b60a06739fd85a5365d4fdc26fae2d4d69a07c21b175f4c403924816c9b0d752a5f7a1e129da1dbb05bd99ca722dd1fd9d83018e79c0a849629395'
            '22662462c823de243599cdd3483e46ade4ab59b219e907468d34c18e584fe7477548e357ee2ce56bb098cf54b770b108a3511703dd486f0774a65c84af78f6aa'
            '3c6ee1e4646d09c164a2212f9e4d2f53158ff32911b0972e060a395a8d4685334574a7ede995a81680dcc0750cd3327a78beb7904a4bb326b2399d79a8b12d5e')

build() {
  cd ${pkgname}
  cmake \
    -B build \
    -G Ninja \
    -D CMAKE_BUILD_TYPE=RelWithDebInfo \
    -D CMAKE_INSTALL_PREFIX=/usr \
    -D USE_BUNDLED=OFF \
    -D ENABLE_TRANSLATIONS=ON \
    -W no-dev
  cmake --build build --verbose
}

check() {
  cd ${pkgname}
  ./build/bin/nvim --version
  ./build/bin/nvim --headless -u NONE -i NONE +q
}

package() {
  install -Dm644 -t "${pkgdir}"/usr/share/libalpm/hooks/ nvimdoc.hook
  install -Dt "${pkgdir}"/usr/share/libalpm/scripts/ nvimdoc

  cd ${pkgname}
  DESTDIR="${pkgdir}" cmake --install build

  install -Dm644 LICENSE.txt -t "${pkgdir}"/usr/share/licenses/${pkgname}/
  install -Dm644 runtime/nvim.desktop -t "${pkgdir}"/usr/share/applications/
  install -Dm644 runtime/nvim.appdata.xml -t "${pkgdir}"/usr/share/metainfo/
  install -Dm644 runtime/nvim.png -t "${pkgdir}"/usr/share/pixmaps/

  # Tree-sitter grammars are packaged separately and installed into
  # /usr/lib/tree_sitter.
  ln -s /usr/lib/tree_sitter "${pkgdir}"/usr/share/nvim/runtime/parser

  # Include system-wide Vim directory in runtimepath
  mkdir -p "${pkgdir}"/etc/xdg/nvim
  echo 'source /usr/share/nvim/archlinux.lua' > "${pkgdir}"/etc/xdg/nvim/sysinit.vim

  mkdir -p "${pkgdir}"/usr/share/vim
  cat > "${pkgdir}"/usr/share/nvim/archlinux.lua << EOF
-- Modify runtimepath to also search the system-wide Vim directory
-- (eg. for Vim runtime files from Arch Linux packages)
vim.opt.runtimepath:append({ '/usr/share/vim/vimfiles', '/usr/share/vim/vimfiles/after' })
EOF
}
