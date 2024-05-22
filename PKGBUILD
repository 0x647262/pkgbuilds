# Maintainer: schuay <jakob.gruber@gmail.com>
# Maintainer: Maxime Gauduin <alucryd@archlinux.org>
# Contributor: Jeremy Newton (Mystro256) <alexjnewt@gmail.com>

pkgbase=dolphin-emu
pkgname=(
  dolphin-emu
  dolphin-emu-tool
)
pkgver=5.0.r21582.c8ea116658
pkgrel=1
epoch=1
pkgdesc='A Gamecube and Wii emulator'
arch=(x86_64)
url=https://dolphin-emu.org
license=(GPL2)
depends=(
  alsa-lib
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
  libevdev
  libfmt.so
  libgl
  libpulse
  libsfml-network.so
  libsfml-system.so
  libspng.so
  libswscale.so
  libudev.so
  libusb-1.0.so
  libx11
  libxi
  libxrandr
  lzo
  minizip-ng
  pugixml
  sfml
  speexdsp
  xz
  zstd
)
makedepends=(
  cmake
  git
  miniupnpc
  ninja
  python
  qt6-base
  qt6-svg
)
optdepends=('pulseaudio: PulseAudio backend')
options=(!emptydirs !lto)
_commit=c8ea116658ab814751c514d9062d694547bf3a6d
source=(
  dolphin-emu::git+https://github.com/dolphin-emu/dolphin.git#commit=${_commit}
  git+https://github.com/mozilla/cubeb.git
  git+https://github.com/epezent/implot.git
  git+https://github.com/mgba-emu/mgba.git
  git+https://github.com/RetroAchievements/rcheevos.git
  git+https://github.com/arsenm/sanitizers-cmake.git
  git+https://github.com/KhronosGroup/SPIRV-Cross.git
  git+https://github.com/syoyo/tinygltf.git
  git+https://github.com/GPUOpen-LibrariesAndSDKs/VulkanMemoryAllocator.git
  git+https://github.com/zlib-ng/zlib-ng.git
)
b2sums=('4974296e149ddb8cd369041f5b8f81973108472cf58ecde1a72778d2ee8bff3c31b2dc58c73a86cfd5c9bf95f53023585979ff591ccd275dd647761de28f4068'
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
  for submodule in Externals/{cubeb/cubeb,implot/implot,mGBA/mgba,rcheevos/rcheevos,spirv_cross/SPIRV-Cross,tinygltf/tinygltf,VulkanMemoryAllocator,zlib-ng/zlib-ng}; do
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
  git describe | sed 's/-/.r/; s/-g/./'
}

build() {
  cmake -S dolphin-emu -B build -G Ninja \
    -DCMAKE_BUILD_TYPE=None \
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
    hicolor-icon-theme
    libminiupnpc.so
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
