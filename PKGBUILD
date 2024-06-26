# Maintainer: groestlcoin <groestlcoin@gmail.com>

pkgbase=groestlcoin
pkgname=('groestlcoin-daemon' 'groestlcoin-cli' 'groestlcoin-qt' 'groestlcoin-tx' 'groestlcoin-wallet' 'groestlcoin-util')
pkgver=27.0
pkgrel=1
arch=('x86_64')
url="https://www.groestlcoin.org/groestlcoin-core-wallet/"
makedepends=(
  boost
  db5.3
  gcc-libs
  glibc
  gmp
  libevent
  libminiupnpc.so
  libsqlite3.so
  libzmq.so
  qrencode
  qt5-base
  qt5-tools
)
license=('MIT')
source=("$pkgbase-$pkgver.tar.gz::https://github.com/Groestlcoin/groestlcoin/releases/download/v$pkgver/groestlcoin-$pkgver.tar.gz"
        "$pkgbase-$pkgver.SHA256SUMS::https://github.com/Groestlcoin/groestlcoin/releases/download/v$pkgver/SHA256SUMS"
        "$pkgbase-$pkgver.SHA256SUMS.asc::https://github.com/Groestlcoin/groestlcoin/releases/download/v$pkgver/SHA256SUMS.asc"
        "groestlcoin.sysusers"
        "groestlcoin.tmpfiles"
        "groestlcoin-qt.desktop"
        "groestlcoin-qt.appdata.xml")
sha256sums=('cf8de03ef104e67aa7c0c1f69fd78e19ea6fa3e8187d890d7916c1c72a3be530'
            '40c9e0ee5460e363823e3f156643874d39b26f4f54bf259181918005fc61b9b2'
            'SKIP'
            '766f1732b72ee105aa4380ab9433bc6e7d957896e0f3d84eaf08202dc7c0fc85'
            '3cc8b772cd5bde500d74ec45c870168834b93b3b69197a8b1aa809d8b9a69d4f'
            '4dc7fe4ae360b2bbd2ffbebab8849417c31145adff2ecdcfbb3bb03835cd1cf7'
            '87f9a2bc6c3a91f7fd9668d84e35e69bdaed221c7d4655d39b54561845424e21')
b2sums=('46b0ef60a017d037e9d0094540e118fb7ca29d308544dfa735bacdf27113061e5e3e83d2be3eb9ad2a6545c5e743ffc8a173cdb0c7ed11e874662dbff4bddf88'
        '919f2fc3952df8c5c1408d2ce9233d5f47dff76790bd81ffca750eaef40096e61ecc01e996bb4de27938ed557e8926eb9f1b0e6bec5ee84b129fa42a8163a7a0'
        'SKIP'
        'f6bfe677aea28c40794f3c37e48d908215543736c558ef9f3f7ada6cf1d9016200821903c6c676f4841092170cfa64ee8f03f697aea19ea82b78877f9167526b'
        'ebf2151e205daeb14ab5260f204040dcb2bf9969d3e6be8c166abdb74f86ef92a05174cc97f2360c8044c81e8bdfd68a74bf1f114dce8b75e421b4184165a54f'
        '3d0fc6a8f970f4415e577c687d154e08763b8ad9bab9018af2687ee940cf423b25624e3581b49e90112ba2a02e385747740f9b30b8dceb0f4c8d5f8f82096ab9'
        'f08bc357fa34653a8d7588b6ce06fb4ff686d4f701bdbe15ca12648ed62bb6afbaf1680f77c340a291735a2f56086a061a2d671ab19b95d95803917c40ce6f9a')
validpgpkeys=(287AE4CA1187C68C08B49CB2D11BD4F33F1DB499)
changelog=Changelog

prepare() {
  sha256sum -c --ignore-missing "$pkgbase-$pkgver.SHA256SUMS"
  cd "$pkgbase-$pkgver"
  autoreconf -fi
}

build() {
  cd $pkgbase-$pkgver
  #remove _FORTIFY_SOURCE from CXXFLAGS to prevent a duplicate definition warning as configure adds _FORTIFY_SOURCE itself
  CXXFLAGS=${CXXFLAGS/-Wp,-D_FORTIFY_SOURCE=?/}
  ./configure --prefix=/usr --with-gui=qt5 BDB_LIBS="-ldb_cxx-5.3" BDB_CFLAGS="-I/usr/include/db5.3"
  make
}

package_groestlcoin-qt() {
  pkgdesc="Groestlcoin is a peer-to-peer network based digital currency - Qt"
  depends=(
    db5.3
    gcc-libs
    glibc
    gmp
    hicolor-icon-theme
    libevent
    libminiupnpc.so
    libsqlite3.so
    libzmq.so
    qrencode
    qt5-base
  )

  cd $pkgbase-$pkgver
  install -Dm755 src/qt/groestlcoin-qt "$pkgdir"/usr/bin/groestlcoin-qt
  install -Dm644 contrib/completions/fish/groestlcoin-qt.fish \
    -t "$pkgdir/usr/share/fish/vendor_completions.d/"
  install -Dm644 doc/man/groestlcoin-qt.1 \
    "$pkgdir"/usr/share/man/man1/groestlcoin-qt.1
  install -Dm644 ../groestlcoin-qt.desktop \
    "$pkgdir"/usr/share/applications/groestlcoin-qt.desktop
  install -Dm644 ../groestlcoin-qt.appdata.xml \
    "$pkgdir"/usr/share/metainfo/groestlcoin-qt.appdata.xml
  install -Dm644 src/qt/res/src/groestlcoin.svg \
    "$pkgdir"/usr/share/icons/hicolor/scalable/apps/groestlcoin-qt.svg

  for i in 16 32 64 128 256; do
    install -Dm644 share/pixmaps/groestlcoin${i}.png \
      "$pkgdir"/usr/share/icons/hicolor/${i}x${i}/apps/groestlcoin-qt.png
  done

  install -Dm644 COPYING "$pkgdir/usr/share/licenses/$pkgname/COPYING"
}

package_groestlcoin-daemon() {
  pkgdesc="Groestlcoin is a peer-to-peer network based digital currency - daemon"
  depends=(
    db5.3
    gcc-libs
    glibc
    gmp
    libevent
    libminiupnpc.so
    libsqlite3.so
    libzmq.so
  )
  backup=('etc/groestlcoin/groestlcoin.conf')

  cd $pkgbase-$pkgver
  install -Dm755 src/groestlcoind "$pkgdir"/usr/bin/groestlcoind
  install -Dm644 contrib/completions/bash/groestlcoind.bash \
    "$pkgdir/usr/share/bash-completion/completions/groestlcoind"
  install -Dm644 contrib/completions/fish/groestlcoind.fish \
    -t "$pkgdir/usr/share/fish/vendor_completions.d/"
  install -Dm644 doc/man/groestlcoind.1 \
    "$pkgdir"/usr/share/man/man1/groestlcoind.1
  install -Dm644 contrib/init/groestlcoind.service \
    "$pkgdir/usr/lib/systemd/system/groestlcoind.service"
  install -Dm644 "$srcdir/groestlcoin.sysusers" \
    "$pkgdir/usr/lib/sysusers.d/groestlcoin.conf"
  install -Dm644 "$srcdir/groestlcoin.tmpfiles" \
    "$pkgdir/usr/lib/tmpfiles.d/groestlcoin.conf"
  install -Dm644 share/examples/groestlcoin.conf \
    "$pkgdir/etc/groestlcoin/groestlcoin.conf"

  install -Dm644 COPYING "$pkgdir/usr/share/licenses/$pkgname/COPYING"
}

package_groestlcoin-cli() {
  pkgdesc="Groestlcoin is a peer-to-peer network based digital currency - RPC client"
  depends=(boost-libs libevent)

  cd $pkgbase-$pkgver
  install -Dm755 src/groestlcoin-cli "$pkgdir"/usr/bin/groestlcoin-cli
  install -Dm644 contrib/completions/bash/groestlcoin-cli.bash \
    "$pkgdir/usr/share/bash-completion/completions/groestlcoin-cli"
  install -Dm644 contrib/completions/fish/groestlcoin-cli.fish \
    -t "$pkgdir/usr/share/fish/vendor_completions.d/"
  install -Dm644 doc/man/groestlcoin-cli.1 \
    "$pkgdir"/usr/share/man/man1/groestlcoin-cli.1

  install -Dm644 COPYING "$pkgdir/usr/share/licenses/$pkgname/COPYING"
}

package_groestlcoin-tx() {
  pkgdesc="Groestlcoin is a peer-to-peer network based digital currency - Transaction tool"
  depends=(
    db5.3
    gcc-libs
    glibc
    libsqlite3.so
  )

  cd $pkgbase-$pkgver
  install -Dm755 src/groestlcoin-tx "$pkgdir"/usr/bin/groestlcoin-tx
  install -Dm644 contrib/completions/bash/groestlcoin-tx.bash \
    "$pkgdir/usr/share/bash-completion/completions/groestlcoin-tx"
  install -Dm644 contrib/completions/fish/groestlcoin-tx.fish \
    -t "$pkgdir/usr/share/fish/vendor_completions.d/"
  install -Dm644 doc/man/groestlcoin-tx.1 \
    "$pkgdir"/usr/share/man/man1/groestlcoin-tx.1

  install -Dm644 COPYING "$pkgdir/usr/share/licenses/$pkgname/COPYING"
}

package_groestlcoin-wallet() {
  pkgdesc="Groestlcoin is a peer-to-peer network based digital currency - Wallet tool"
  depends=(
    db5.3
    gcc-libs
    glibc
    libsqlite3.so
  )

  cd $pkgbase-$pkgver
  install -Dm755 src/groestlcoin-wallet "$pkgdir"/usr/bin/groestlcoin-wallet
  install -Dm644 contrib/completions/fish/groestlcoin-wallet.fish \
    -t "$pkgdir/usr/share/fish/vendor_completions.d/"
  install -Dm644 doc/man/groestlcoin-wallet.1 \
    "$pkgdir"/usr/share/man/man1/groestlcoin-wallet.1

  install -Dm644 COPYING "$pkgdir/usr/share/licenses/$pkgname/COPYING"
}

package_groestlcoin-util() {
  pkgdesc="Groestlcoin is a peer-to-peer network based digital currency - Util tool"
  depends=(
    db5.3
    gcc-libs
    glibc
    libsqlite3.so
  )

  cd $pkgbase-$pkgver
  install -Dm755 src/groestlcoin-util "$pkgdir"/usr/bin/groestlcoin-util
  install -Dm644 contrib/completions/fish/groestlcoin-util.fish \
    -t "$pkgdir/usr/share/fish/vendor_completions.d/"
  install -Dm644 doc/man/groestlcoin-util.1 \
    "$pkgdir"/usr/share/man/man1/groestlcoin-util.1

  install -Dm644 COPYING "$pkgdir/usr/share/licenses/$pkgname/COPYING"
}
