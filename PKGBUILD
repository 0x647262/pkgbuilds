# Maintainer: schuay <jakob.gruber@gmail.com>
# Maintainer: Maxime Gauduin <alucryd@archlinux.org>
# Contributor: Jeremy Newton (Mystro256) <alexjnewt@gmail.com>

pkgbase=dolphin-emu
pkgname=(
  dolphin-emu
  dolphin-emu-tool
)
pkgver=2603.1
_gittag=2603a
pkgrel=1
epoch=1
pkgdesc='A Gamecube and Wii emulator'
arch=(x86_64)
url=https://dolphin-emu.org
license=(GPL-2.0-or-later)
depends=(
  bluez-libs
  bzip2
  enet
  glibc
  glslang
  hidapi
  libavcodec.so
  libavformat.so
  libavutil.so
  libcurl.so
  #libfmt.so
  libgcc
  libgl
  libsfml-network.so
  libsfml-system.so
  libspng.so
  libstdc++
  libswscale.so
  libusb-1.0.so
  libx11
  libxi
  libxrandr
  lz4
  lzo
  mbedtls2
  pugixml
  sdl3
  speexdsp
  xxhash
  xz
  zlib
  zlib-ng
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
  sfml
  vulkan-headers
)
optdepends=('pulseaudio: PulseAudio backend')
options=(!emptydirs !lto)
source=(
  dolphin-emu::git+https://github.com/dolphin-emu/dolphin.git#tag=${_gittag}
  git+https://github.com/mutouyun/cpp-ipc.git
  git+https://github.com/weisslj/cpp-optparse.git
  git+https://github.com/mozilla/cubeb.git
  git+https://github.com/ocornut/imgui.git
  git+https://github.com/epezent/implot.git
  git+https://github.com/fmtlib/fmt.git
  git+https://github.com/mgba-emu/mgba.git
  git+https://github.com/zlib-ng/minizip-ng.git
  git+https://github.com/RetroAchievements/rcheevos.git
  git+https://github.com/arsenm/sanitizers-cmake.git
  git+https://github.com/KhronosGroup/SPIRV-Cross.git
  git+https://github.com/syoyo/tinygltf.git
  git+https://github.com/GPUOpen-LibrariesAndSDKs/VulkanMemoryAllocator.git
  git+https://github.com/e-dant/watcher.git
  git+https://github.com/zlib-ng/zlib-ng.git
)
b2sums=('8e43c8c34c355a36fea357307a01423a549ce43d2dadf4b782166fe9c62faa9de49798f59d386a599ce964cb8b063a0e5977c218124cfc2345b33776324dcfdb'
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
        'SKIP'
        'SKIP'
        'SKIP'
        'SKIP'
        'SKIP')

prepare() {
  cd dolphin-emu
  for submodule in Externals/{cpp-ipc/cpp-ipc,cpp-optparse/cpp-optparse,cubeb/cubeb,imgui/imgui,implot/implot,fmt/fmt,mGBA/mgba,minizip-ng/minizip-ng,rcheevos/rcheevos,spirv_cross/SPIRV-Cross,tinygltf/tinygltf,VulkanMemoryAllocator,watcher,zlib-ng/zlib-ng}; do
    git submodule init ${submodule}
    git config submodule.${submodule}.url ../${submodule##*/}
    git -c protocol.file.allow=always submodule update ${submodule}
  done
  cd Externals/cubeb/cubeb
  git submodule init cmake/sanitizers-cmake
  git config submodule.cmake/sanitizers-cmake.url "${srcdir}"/sanitizers-cmake
  git -c protocol.file.allow=always submodule update cmake/sanitizers-cmake
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
