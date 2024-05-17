# Maintainer: Sven-Hendrik Haase <svenstaro@archlinux.org>
# Maintainer: Caleb Maclennan <caleb@alerque.com>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>
# Contributor: Florian Walch <florian+aur@fwalch.com>
# Contributor: Florian Hahn <flo@fhahn.com>

pkgname=neovim
pkgver=0.10.0
pkgrel=1
pkgdesc='Fork of Vim aiming to improve user experience, plugins, and GUIs'
arch=('x86_64')
url='https://neovim.io'
license=('custom:neovim')
depends=(
  'libluv'
  'libtermkey'
  'libuv'
  'libvterm>0.1.4'
  'lua51-lpeg'
  'luajit'
  'msgpack-c'
  'tree-sitter'
  'tree-sitter-lua'
  'tree-sitter-query'
  'tree-sitter-vimdoc'
  'unibilium'
)
makedepends=('cmake' 'git' 'ninja' 'lua51-mpack' 'lua51-lpeg' 'unzip')
optdepends=(
  'python-pynvim: for Python plugin support (see :help python)'
  'tree-sitter-bash: for the Bash grammar for tree-sitter'
  'tree-sitter-c: for the C grammar for tree-sitter'
  'tree-sitter-markdown: for the Markdown grammar for tree-sitter'
  'tree-sitter-python: for the Python grammar for tree-sitter'
  'xclip: for clipboard support on X11 (or xsel) (see :help clipboard)'
  'xsel: for clipboard support on X11 (or xclip) (see :help clipboard)'
  'wl-clipboard: for clipboard support on wayland (see :help clipboard)'
)
provides=('vim-plugin-runtime')
backup=('etc/xdg/nvim/sysinit.vim')
source=(
  "https://github.com/neovim/neovim/archive/v${pkgver}/${pkgname}-${pkgver}.tar.gz"
  nvimdoc{,.hook}
)
sha512sums=('878cb58729a18179b38303bd9cf79e38997c87946665ac4e7b76d87e15f5d001028fd1454ce30b20773b20a4fe929dfa399fc2ec7f6db0774a4e864c9e8586c1'
            '22662462c823de243599cdd3483e46ade4ab59b219e907468d34c18e584fe7477548e357ee2ce56bb098cf54b770b108a3511703dd486f0774a65c84af78f6aa'
            '3c6ee1e4646d09c164a2212f9e4d2f53158ff32911b0972e060a395a8d4685334574a7ede995a81680dcc0750cd3327a78beb7904a4bb326b2399d79a8b12d5e')
b2sums=('b9bf44d2f0ae1075196f5f96b2fb5e974907406fa6e5bf09593d1f9f36135dab466b6a063d1d03e3716dce435df296ec6d00a2f015e85d0edd231fb9fd4c9385'
        'd31cf81659e238fada8092755eb9be16f77c00a466107eb5770c6c9c32e043c91e6efada7ddb51663716a0e38ffa6e3d0093b3e6833aa961d845c7451a95491e'
        '26588b9da6459393076723bdfb8d2b16fed882070f2326bf7c35cd272dee9c18df603afb1ae2254cd0a59eff68189caf04828ef165d5de42c7a4222267604101')

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
  ./build/bin/nvim --headless -u NONE -i NONE +q
}

package() {
  install -Dm644 -t "$pkgdir/usr/share/libalpm/hooks/" nvimdoc.hook
  install -Dt "$pkgdir/usr/share/libalpm/scripts/" nvimdoc

  cd ${pkgname}-${pkgver}
  DESTDIR="$pkgdir" cmake --install build

  # Tree-sitter grammars are packaged separately and installed into
  # /usr/lib/tree_sitter.
  ln -s /usr/lib/tree_sitter "$pkgdir"/usr/share/nvim/runtime/parser

  install -Dm644 LICENSE.txt -t "${pkgdir}/usr/share/licenses/${pkgname}/"
  install -Dm644 runtime/nvim.desktop -t "${pkgdir}/usr/share/applications/"
  install -Dm644 runtime/nvim.appdata.xml -t "${pkgdir}/usr/share/metainfo/"
  install -Dm644 runtime/nvim.png -t "${pkgdir}/usr/share/pixmaps/"

  # Make Arch Vim packages work
  mkdir -p "${pkgdir}"/etc/xdg/nvim
  echo "\" This line makes pacman-installed global Arch Linux vim packages work." > "${pkgdir}"/etc/xdg/nvim/sysinit.vim
  echo "source /usr/share/nvim/archlinux.vim" >> "${pkgdir}"/etc/xdg/nvim/sysinit.vim

  mkdir -p "${pkgdir}"/usr/share/vim
  echo "set runtimepath+=/usr/share/vim/vimfiles" > "${pkgdir}"/usr/share/nvim/archlinux.vim
}

# vim:set sw=2 sts=2 et:
