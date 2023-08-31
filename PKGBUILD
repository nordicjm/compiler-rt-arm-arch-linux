# Copyright 2023 Nordic Semiconductor ASA

pkgname=compiler-rt-armv7
pkgver=16.0.6
pkgrel=1
pkgdesc="compiler-rt library for ARMv7 targets"
arch=('x86_64')
url="https://clang.llvm.org/"
license=('custom:Apache 2.0 with LLVM Exception')
depends=('llvm' 'clang')
makedepends=('cmake' 'ninja')
options=('staticlibs' '!strip')
_source_base=https://github.com/llvm/llvm-project/releases/download/llvmorg-$pkgver
source=($_source_base/llvm-project-$pkgver.src.tar.xz)
sha256sums=('ce5e71081d17ce9e86d7cbcfa28c4b04b9300f8fb7e78422b1feb6bc52c3028e')

build() {
  mkdir -p llvm-project-$pkgver.src/build
  cd llvm-project-$pkgver.src/build

  cmake ../compiler-rt \
    -GNinja \
    -DCMAKE_TRY_COMPILE_TARGET_TYPE=STATIC_LIBRARY \
    -DCOMPILER_RT_OS_DIR="baremetal" \
    -DCOMPILER_RT_BUILD_BUILTINS=ON \
    -DCOMPILER_RT_BUILD_SANITIZERS=OFF \
    -DCOMPILER_RT_BUILD_XRAY=OFF \
    -DCOMPILER_RT_BUILD_LIBFUZZER=OFF \
    -DCOMPILER_RT_BUILD_PROFILE=OFF \
    -DCMAKE_C_COMPILER_TARGET="armv7m-none-eabi" \
    -DCMAKE_ASM_COMPILER_TARGET="armv7m-none-eabi" \
    -DCOMPILER_RT_BAREMETAL_BUILD=ON \
    -DCOMPILER_RT_DEFAULT_TARGET_ONLY=ON \
    -DLLVM_CONFIG_PATH=/usr/bin/llvm-config \
    -DCMAKE_C_FLAGS="-mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16" \
    -DCMAKE_ASM_FLAGS="-mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16" \
    -DCMAKE_C_COMPILER=/usr/bin/clang \
    -DCMAKE_AR=/usr/bin/llvm-ar \
    -DCMAKE_NM=/usr/bin/llvm-nm \
    -DCMAKE_RANLIB=/usr/bin/llvm-ranlib \
    -DCMAKE_BUILD_TYPE=Release

  ninja
}

package() {
  cd llvm-project-$pkgver.src/build

  mkdir -p "$pkgdir/usr/lib/clang/16/lib/baremetal"
  install lib/baremetal/libclang_rt.builtins-armv7m.a "$pkgdir/usr/lib/clang/16/lib/baremetal/libclang_rt.builtins-armv7m.a"
}
