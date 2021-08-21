pkgname="aitrack"
pkgver="0.6.4_alpha"
pkgrel="1"
pkgdesc="A 6-Degree of Freedom headtracker designed to work alongside Opentrack for its use in simulators/games."
arch=("x86_64")
url="https://github.com/mdk97/aitrack-linux"
source=("git+https://github.com/mdk97/aitrack-linux")
license=("MIT")
sha256sums=('SKIP')
depends=("opencv" "fmt" "spdlog" "qt5-base" "qt5-x11extras" "openmp" "vtk" "glew" "hdf5")
makedepends=("curl" "gzip" "tar" "make" "gcc" "opencv" "fmt" "spdlog" "qt5-base" "qt5-x11extras" "openmp" "vtk")
install=aitrack.install
prepare() {
    cd "$srcdir/${pkgname}-linux"
    curl -L https://github.com/microsoft/onnxruntime/releases/download/v1.4.0/onnxruntime-linux-x64-1.4.0.tgz --output onnxruntime-linux-x64-1.4.0.tgz
    gunzip onnxruntime-linux-x64-1.4.0.tgz
    tar --extract -f onnxruntime-linux-x64-1.4.0.tar

    export number_of_threads=$(getconf _NPROCESSORS_ONLN)
    qmake -makefile
}
build() {
    cd "$srcdir/${pkgname}-linux"
    make -j$number_of_threads
}
package() {
    install -d "$pkgdir/usr/lib"
    install -d "$pkgdir/usr/bin"
    install -d "$pkgdir/usr/share/aitrack/"
    install -d "$pkgdir/usr/share/aitrack/models"
    cd "$srcdir/${pkgname}-linux"
    install onnxruntime-linux-x64-1.4.0/lib/libonnxruntime.so.1.4.0 "$pkgdir/usr/lib/libonnxruntime.so.1.4.0"
    install aitrack "$pkgdir/usr/bin/aitrack"
    install models/* "$pkgdir/usr/share/aitrack/models/"
}
