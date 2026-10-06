# Maintainer: Section 9 Architecture Team
pkgname=whisper-cpp-cuda
pkgver=1.7.4
pkgrel=1
pkgdesc="Port of OpenAI's Whisper model in C/C++ with CUDA acceleration (sm_86 optimized)"
arch=('x86_64')
url="https://github.com/ggerganov/whisper.cpp"
license=('MIT')
depends=('cuda' 'gcc-libs')
makedepends=('git' 'cmake' 'ninja')
provides=('whisper-cpp')
conflicts=('whisper-cpp')
source=("git+https://github.com/ggerganov/whisper.cpp.git#tag=v${pkgver}")
sha256sums=('SKIP')

build() {
  cd whisper.cpp
  cmake -B build -G Ninja \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DGGML_CUDA=ON \
    -DCUDA_ARCHITECTURES="86" \
    -DWHISPER_BUILD_EXAMPLES=ON \
    -DWHISPER_BUILD_SERVER=ON
  cmake --build build
}

package() {
  cd whisper.cpp
  DESTDIR="${pkgdir}" cmake --install build

  # llama-cpp-cuda と衝突する共通ヘッダーおよび静的/共有ggmlライブラリの重複を排除
  rm -rf "${pkgdir}/usr/include/ggml"*
  rm -rf "${pkgdir}/usr/lib/cmake/ggml"
  rm -rf "${pkgdir}/usr/lib/pkgconfig/ggml.pc"

  # ライセンスファイルの配置
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
