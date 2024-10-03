# Maintainer: schuay <jakob.gruber@gmail.com>
# Maintainer: Maxime Gauduin <alucryd@archlinux.org>
# Contributor: Jeremy Newton (Mystro256) <alexjnewt@gmail.com>

pkgbase=dolphin-emu
pkgname=(
  dolphin-emu
  dolphin-emu-tool
)
pkgver=2409
pkgrel=2
epoch=1
pkgdesc='A Gamecube and Wii emulator'
arch=(x86_64)
url=https://dolphin-emu.org
license=(GPL-2.0-or-later)
depends=(
  bluez-libs
  bzip2
  enet
  gcc-libs
  glibc
  hidapi
  libavcodec.so
  libavformat.so
  libavutil.so
  libcurl.so
  libfmt.so
  libgl
  libsfml-network.so
  libsfml-system.so
  libspng.so
  libswscale.so
  libusb-1.0.so
  libx11
  libxi
  libxrandr
  lz4
  lzo
  mbedtls2
  pugixml
  sdl2
  sfml
  speexdsp
  xxhash
  xz
  zstd
)
makedepends=(
  alsa-lib
  cmake
  git
  libevdev
  libminiupnpc.so
  libpulse
  libudev.so
  ninja
  python
  qt6-base
  qt6-svg
)
optdepends=('pulseaudio: PulseAudio backend')
options=(!emptydirs !lto)
_tag=3c4d4fcd09173ea070dc812ab5d64ca3a3af5f29
source=(
  dolphin-emu::git+https://github.com/dolphin-emu/dolphin.git#tag=${_tag}
  git+https://github.com/mozilla/cubeb.git
  git+https://github.com/epezent/implot.git
  git+https://github.com/mgba-emu/mgba.git
  git+https://github.com/zlib-ng/minizip-ng.git
  git+https://github.com/RetroAchievements/rcheevos.git
  git+https://github.com/arsenm/sanitizers-cmake.git
  git+https://github.com/KhronosGroup/SPIRV-Cross.git
  git+https://github.com/syoyo/tinygltf.git
  git+https://github.com/GPUOpen-LibrariesAndSDKs/VulkanMemoryAllocator.git
  git+https://github.com/zlib-ng/zlib-ng.git
)
b2sums=('749cce559d4e120ecd158c133ccee9b8681cb46064e6d1b33f91713c8e3025058a3573a458af3bc9035a860d208db3c94b1122b9b2f5403493ca6ca7c3de58bb'
        'SKIP'
        'SKIP'
        'SKIP'
        'SKIP'
        'SKIP'
        'SKIP'
        'SKIP'
        'SKIP'
        'SKIP'
        'SKIP')

prepare() {
  cd dolphin-emu
  for submodule in Externals/{cubeb/cubeb,implot/implot,mGBA/mgba,minizip-ng/minizip-ng,rcheevos/rcheevos,spirv_cross/SPIRV-Cross,tinygltf/tinygltf,VulkanMemoryAllocator,zlib-ng/zlib-ng}; do
    git submodule init ${submodule}
    git config submodule.${submodule}.url ../${submodule##*/}
    git -c protocol.file.allow=always submodule update ${submodule}
  done
  cd Externals/cubeb/cubeb
  git submodule init cmake/sanitizers-cmake
  git config submodule.cmake/sanitizers-cmake.url "${srcdir}"/sanitizers-cmake
  git -c protocol.file.allow=always submodule update cmake/sanitizers-cmake
}

pkgver() {
  cd dolphin-emu
  git describe --tags
}

build() {
  cmake -S dolphin-emu -B build -G Ninja \
    -DCMAKE_BUILD_TYPE=None \
    -DCMAKE_SKIP_RPATH=ON \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DDISTRIBUTOR=archlinux.org \
    -DENABLE_ANALYTICS=OFF \
    -DENABLE_AUTOUPDATE=OFF \
    -DENABLE_LTO=ON \
    -DENABLE_TESTS=OFF \
    -DUSE_MGBA=ON \
    -Wno-dev
  cmake --build build
}

package_dolphin-emu() {
  depends+=(
    alsa-lib
    hicolor-icon-theme
    libevdev
    libminiupnpc.so
    libpulse
    libudev.so
    qt6-base
    qt6-svg
  )

  DESTDIR="${pkgdir}" cmake --install build
  install -Dm 644 dolphin-emu/Data/51-usb-device.rules -t "${pkgdir}"/usr/lib/udev/rules.d/
  rm -rf "${pkgdir}"/usr/{bin/dolphin-tool,include,lib/libdiscord-rpc.a}
}

package_dolphin-emu-tool() {
  install -Dm 755 build/Binaries/dolphin-tool -t "${pkgdir}"/usr/bin/
}

# vim: ts=2 sw=2 et:
