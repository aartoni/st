# Maintainer: arto <alessio@artoni.org>
pkgname=minilarbs-st
pkgver=0.9.3
pkgrel=1
pkgdesc="minilarbs build of st"
arch=(x86_64)
url=https://github.com/aartoni/st
license=(MIT)
depends=(glibc fontconfig harfbuzz libx11 libxft sh coreutils sed grep gawk util-linux)
makedepends=(freetype2)
optdepends=(
  'dmenu: URL and output pickers (Alt+l, Alt+y, Alt+o)'
  'xclip: copy a URL or command output (Alt+y, Alt+o)'
  'xdg-utils: open a URL (Alt+l)'
)
provides=(st)
conflicts=(st)
_patches=(
  https://st.suckless.org/patches/externalpipe/st-externalpipe-0.8.5.diff
  st-scrollback-0.9.2.patch
  st-externalpipe-eternal-0.8.3.patch
  https://st.suckless.org/patches/xresources/st-xresources-20260524-688f70a.diff
  https://st.suckless.org/patches/alpha/st-alpha-20240814-a0274bc.diff
  st-changealpha-20251027-0.9.3.patch
  st-boxdraw_v2-0.8.5.patch
  st-ligatures-boxdraw-20251007-0.9.3.patch
  st-font2-0.8.5.patch
  minilarbs-class.patch
)
source=("https://dl.suckless.org/st/st-$pkgver.tar.gz"
        config.h
        st-urlhandler
        st-copyout
        "${_patches[@]}")
b2sums=('95d672b8bd25f2bcda9ef7d0238fec522045e5082d5bdfda0b34ad1c67b93c1571e62f96b41f6a024ca00311d405d081a922f61604014d99708a0bdf20eb0e38'
        '63d4b9717842ad43552381280542969a5f09b25d432585f25d5107e03597efba2f3c4cd3190ed6682a800a851397f633cc93611a282c396c233a6e9081889297'
        '139c6c64bb128ddd024dd6544e178cc89b382a862eca9e1406f8f18cf017b57f644717d0d202f6acea7e0b723db51abd4e52c9ce4341aebea3af01e095553630'
        'e0a726b8d4e80047d0511dd6b04256cfdacbc61d805ce2b0ac58dd5c0ecf932a7744e368aa9ca8feb34caba4332bfe2ebbcc7384e9d1f6efb405d9e1e5c44ea1'
        'd8ff75d18cd2c657d683c653e1a4bec9c064f5eb1052a145afe9788cbd8d646a45e23c37f7e640a474ad77bd1591623b206221df76339980931128ed0db8c8e1'
        '8818beb92407d586071645bfcf9fb206010f967e29541cbf563e30107a130a5518b8cbe433f29b6ca891c4b57ddf9e070320c1f4e2b70dbc4f0a8cdd46d8ba7d'
        '16fd18614b9663556494e882ba60c133e87641f2678a984261c0f7ed6d4248a499fb009dd276db0a1c219a4449c62b236207fcc03ae055dba88d63a3e09037b7'
        '027b43661e3409a23839b51c3578f3d982357ef9731738a6547280d5e53234dd6dba8bdbc23ccda3eb2d1fe5fddc036bfd8bf37b671c3b3bf48c471e73433fcb'
        '1ebf136e4f9f0cb18f75e94cbfbadf7c3a2cb8c835ad26262f2e98cf25e88efe568946a9894430151711a3a0e7c96aa71c2e34dd1e9a9c9e69f045f8f9c8df2a'
        'def850fb10729ca0be50452f3506b86d1ba929cd37f5232a06d0a6b99cdcd4b71967e6c33a8f2b70a029c21ab3562859757df1cbdaa77bfada5b52d70947a173'
        '1852146f515637bd708b486a91c4693427a259c1390ca42203b8b2795d11b562c6959c2392d6e7bada1ddb315ae079aae78c55890baa2a20952c37c4fd1ec62c'
        'a7c0956e5f03462beb5788f4b1d2b53ab36050df763bf4274f5fa04b42161c27070cb4878432033e4ef3f307eae718e150bc8432cc8663c572c95a0ba80b73bd'
        'f6c04383bba4b9418116e08749acabc552a9c86aff5750ae9bc46c208a9989432ad2c6bc6f517cd7993f4a7c24a526c673999cdc0fd9847ec6a961cb0794cc96'
        '8c49472eba0cc21b26078b14f495154784d574d999044e853d8021b1d5ed6a82e057f99bf3ce1fe419fe83316a05d1070da485894572ba4905407d3ac5d4c105')

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
  install -Dm755 -t "$pkgdir/usr/bin" st-urlhandler st-copyout
  install -Dm644 "st-$pkgver/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
