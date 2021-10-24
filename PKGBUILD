# Maintainer: Atom Long <atom.long@hotmail.com>

pkgname=subconverter-git
pkgver=0.7.1.r8.g3fe9725
pkgrel=2
pkgdesc='Utility to convert between various subscription format'
url='https://github.com/tindy2013/subconverter'
arch=('x86_64' 'i686' 'arm' 'armv6h' 'armv7h' 'aarch64')
license=('GPL3')
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
source=("${pkgname%-git}::git+${url}.git"
        "0001-Explicitly-link-with-libdl.patch"
        "0002-fix-buffer-size.patch"
        "subconverter.service")
sha256sums=('SKIP'
            'e0555eb211e1eac2e1dccf64c970d4d5f42952a876b6043628c6ab8b47786e50'
            'e491b58e2abb317be9dce35974dfd5e6aebb27a3fb3d2fa7667d76c803c2be30'
            'd264ff72eca2668312a70583eecd8e5c7fd674df025f3da4ba11bdf308c8c5b1')
depends=('curl'
         'libevent'
         'yaml-cpp')
makedepends=('cmake'
             'git'
             'rapidjson'
             'toml11-git'
             'quickjspp'
             'libcron')

pkgver() {
  cd ${pkgname%-git}
  git describe --long --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  cd ${pkgname%-git}
  git apply "${srcdir}/0001-Explicitly-link-with-libdl.patch"
  git apply "${srcdir}/0002-fix-buffer-size.patch"
}

build() {
  cd ${pkgname%-git}
  export PKG_CONFIG_PATH=/usr/lib/pkgconfig
  cmake -DCMAKE_INSTALL_BINDIR="/opt" -DCMAKE_BUILD_TYPE=Release .
  make
}

package() {
  cd ${pkgname%-git}
  make DESTDIR="${pkgdir}" install
  
  mkdir -p "${pkgdir}/usr/bin"
  ln -s /opt/subconverter/subconverter "${pkgdir}/usr/bin/subconverter"
  
  install -Dm644 "${srcdir}"/subconverter.service  ${pkgdir}/usr/lib/systemd/system/subconverter.service
  
  # install -Dm644 LICENSE "${pkgdir}"/usr/share/licenses/${pkgname%-git}/LICENSE
}
