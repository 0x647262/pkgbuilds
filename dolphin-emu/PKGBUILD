# Maintainer: schuay <jakob.gruber@gmail.com>
# Maintainer: Maxime Gauduin <alucryd@archlinux.org>
# Contributor: Jeremy Newton (Mystro256) <alexjnewt@gmail.com>

pkgbase=dolphin-emu
pkgname=(
  dolphin-emu
  dolphin-emu-tool
)
pkgver=2503
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
  #libfmt.so
  libgl
  #libsfml-network.so
  #libsfml-system.so
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
  #sfml
  vulkan-headers
)
optdepends=('pulseaudio: PulseAudio backend')
options=(!emptydirs !lto)
_tag=9763c0a1e2b9db0c3861d25bc2f5a0ace6a15ee3
source=(
  dolphin-emu::git+https://github.com/dolphin-emu/dolphin.git#tag=${_tag}
  git+https://github.com/mozilla/cubeb.git
  git+https://github.com/epezent/implot.git
  git+https://github.com/fmtlib/fmt.git
  git+https://github.com/mgba-emu/mgba.git
  git+https://github.com/zlib-ng/minizip-ng.git
  git+https://github.com/RetroAchievements/rcheevos.git
  git+https://github.com/arsenm/sanitizers-cmake.git
  git+https://github.com/KhronosGroup/SPIRV-Cross.git
  git+https://github.com/syoyo/tinygltf.git
  git+https://github.com/GPUOpen-LibrariesAndSDKs/VulkanMemoryAllocator.git
  git+https://github.com/zlib-ng/zlib-ng.git
)
b2sums=('33965c02b34ad449f16e267d853e4b9869902a4c74d169d3b34eb762b07167228e13df5c5dba632acf49183066bd62f947f045e83be927896bb0259adf9e7c6f'
        'SKIP'
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
  for submodule in Externals/{cubeb/cubeb,implot/implot,fmt/fmt,mGBA/mgba,minizip-ng/minizip-ng,rcheevos/rcheevos,spirv_cross/SPIRV-Cross,tinygltf/tinygltf,VulkanMemoryAllocator,zlib-ng/zlib-ng}; do
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
    -DCMAKE_POLICY_VERSION_MINIMUM=3.5 \
    -DDISTRIBUTOR=archlinux.org \
    -DENABLE_ANALYTICS=OFF \
    -DENABLE_AUTOUPDATE=OFF \
    -DENABLE_LTO=OFF \
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
  depends+=(
    alsa-lib
    libevdev
    libpulse
    libudev.so
    qt6-base
  )
  install -Dm 755 build/Binaries/dolphin-tool -t "${pkgdir}"/usr/bin/
}

# vim: ts=2 sw=2 et:
