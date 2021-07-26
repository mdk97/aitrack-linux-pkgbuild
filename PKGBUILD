pkgname="aitrack"
pkgver="0.6.4_alpha"
pkgrel="1"
pkgdesc="A 6-Degree of Freedom headtracker designed to work alongside Opentrack for its use in simulators/games."
arch=("x86_64")
url="https://github.com/mdk97/aitrack-linux"
source=("git+https://github.com/mdk97/aitrack-linux")
license=("MIT")
sha256sums=('SKIP')
depends=("opencv" "fmt" "spdlog" "qt5-base" "qt5-x11extras" "openmp" "vtk")
makedepends=("curl" "gzip" "tar" "make" "gcc" "opencv" "fmt" "spdlog" "qt5-base" "qt5-x11extras" "openmp" "vtk")
install=aitrack.install
prepare() {
    cd "$srcdir/${pkgname}-linux"
    curl -L https://github.com/microsoft/onnxruntime/releases/download/v1.4.0/onnxruntime-linux-x64-1.4.0.tgz --output onnxruntime-linux-x64-1.4.0.tgz
    gunzip onnxruntime-linux-x64-1.4.0.tgz
    tar --extract -f onnxruntime-linux-x64-1.4.0.tar

    export LD_LIBRARY_PATH="/usr/lib/ onnxruntime-linux-x64-1.4.0/lib/"
    export number_of_threads=$(getconf _NPROCESSORS_ONLN)
    pwd
    qmake -makefile
}
build() {
    cd "$srcdir/${pkgname}-linux"
    make -j$number_of_threads
}
package() {
    mkdir "$pkgdir/usr"
    mkdir "$pkgdir/usr/lib"
    mkdir "$pkgdir/usr/bin"
    mkdir "$pkgdir/home"
    mkdir "$pkgdir/home/$USER"
    chmod 700 "$pkgdir/home/$USER"
    mkdir "$pkgdir/home/$USER/.local"
    chmod 700 "$pkgdir/home/$USER/.local"
    mkdir "$pkgdir/home/$USER/.local/share"
    chmod 700 "$pkgdir/home/$USER/.local/share"
    mkdir "$pkgdir/home/$USER/.local/share/aitrack"
    chown $USER "$pkgdir/home/$USER/.local/share/aitrack"
    chmod 770 "$pkgdir/home/$USER/.local/share/aitrack"
    mkdir "$pkgdir/home/$USER/.local/share/aitrack/models"
    chown $USER "$pkgdir/home/$USER/.local/share/aitrack/models"
    chmod 770 "$pkgdir/home/$USER/.local/share/aitrack/models" 
    cd "$srcdir/${pkgname}-linux"
    cp onnxruntime-linux-x64-1.4.0/lib/libonnxruntime.so.1.4.0 "$pkgdir/usr/lib/"
    cp aitrack "$pkgdir/usr/bin/"
    cp models/* "$pkgdir/home/$USER/.local/share/aitrack/models/"
}
