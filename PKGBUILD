# Maintainer: arto <alessio@artoni.org>
pkgname=minilarbs-st
pkgver=0.9.3
pkgrel=1
pkgdesc="minilarbs build of st"
arch=(x86_64)
url=https://github.com/aartoni/st
license=(MIT)
depends=(glibc fontconfig libx11 libxft)
makedepends=(freetype2)
provides=(st)
conflicts=(st)
_patches=(
)
source=("https://dl.suckless.org/st/st-$pkgver.tar.gz"
        config.h
        "${_patches[@]}")
b2sums=('95d672b8bd25f2bcda9ef7d0238fec522045e5082d5bdfda0b34ad1c67b93c1571e62f96b41f6a024ca00311d405d081a922f61604014d99708a0bdf20eb0e38'
        '8d17bbbad10eb28375175ae5614fcc9ccb29364e9871a5aaf75408ee7bf383c5edf204b8789a5c88951f798421b33350941d8badfb372bf8f1ce749f89901f7e')

prepare() {
  cd "st-$pkgver"
  local p
  for p in "${_patches[@]}"; do
    patch -Np1 -i "$srcdir/${p##*/}"
  done
  cp "$srcdir/config.h" .
  # ncurses already ships st's terminfo entries.
  sed -i '/^\ttic /d' Makefile
  # LDFLAGS first so --as-needed precedes $(LIBS).
  printf '%s\n' 'STLDFLAGS = $(LDFLAGS) $(LIBS)' >> config.mk
}

build() {
  make -C "st-$pkgver" \
    X11INC=/usr/include/X11 \
    X11LIB=/usr/lib/X11
}

package() {
  make -C "st-$pkgver" PREFIX=/usr DESTDIR="$pkgdir" install
  install -Dm644 "st-$pkgver/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
