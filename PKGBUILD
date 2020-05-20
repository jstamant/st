pkgname=st
pkgver=0.8.2
pkgrel=1
pkgdesc='Simple terminal emulator for X'
url='http://st.suckless.org/'
arch=('i686' 'x86_64')
license=('MIT')
depends=('libxft' 'compton')
makedepends=('ncurses' 'libxext' 'git')
source=(https://dl.suckless.org/st/st-$pkgver.tar.gz
        config.h
        st-anysize-0.8.1.diff
        st-alpha-20190116-3be4cf1.diff
        st-xresources-20190105-9f80f67.diff
        st-bold-is-not-bright-20190127-3be4cf1.diff)
sha1sums=('1093714e46adf3f02f9ae9922c49d8ff63b83128'
          '551faecff27f9671f10bd4b91edc619d54cadb96'
          'a75f5eaee7b05b1cd960ef133a34d3aeb69d8f27'
          '523c8851c88ac87afb77fbfec0cc5983a4af7b99'
          '00d4234c5857d6067cede2a0765caabf6fa27b2d'
          'bef42114952e4fead262bb1b491112014ac7bc39')

prepare() {
  cd $pkgname-$pkgver
#  sed \
#    -e 's/CPPFLAGS =/CPPFLAGS +=/g' \
#    -e 's/CFLAGS =/CFLAGS +=/g' \
#    -e 's/LDFLAGS =/LDFLAGS +=/g' \
#    -e 's/_BSD_SOURCE/_DEFAULT_SOURCE/' \
#    -i config.mk
#  sed '/@tic/d' -i Makefile
  # Add config.h and apply any patches present in source array
  #local file
  for file in "${source[@]}"; do
    if [[ $file == config.h ]]; then
      cp $srcdir/$file .
    elif [[ $file == *.diff || $file == *.patch ]]; then
      echo "Applying $file"
      patch -Np1 -i "$srcdir/$(basename $file)"
    fi
  done
}

build() {
  cd $pkgname-$pkgver
  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  cd $pkgname-$pkgver
  make PREFIX=/usr DESTDIR=$pkgdir install
  install -Dm644 LICENSE $pkgdir/usr/share/licenses/$pkgname/LICENSE
  install -Dm644 README $pkgdir/usr/share/doc/$pkgname/README
}
