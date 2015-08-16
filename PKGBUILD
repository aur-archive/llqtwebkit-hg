# Maintainer: Bennett Goble <nivardus@gmail.com>

pkgname=llqtwebkit-hg
pkgver=470
pkgrel=1
pkgdesc="A static library that renders Web content into a memory buffer using an off-the-shelf version of the Qt application and UI framework. Official Linden Labs Mercurial repository."
arch=('i686' 'x86_64')
url="http://wiki.secondlife.com/wiki/LLQtWebKit"
license=('GPL')
depends=('qt')
makedepends=('mercurial')
provides=('llqtwebkit-hg')

_hgroot="https://bitbucket.org/lindenlab"
_hgrepo="llqtwebkit"

build() {
	cd $srcdir
	qtver=`pacman -Qi qt | grep Version | awk '{print $3}' | sed -e 's/-*//g'`
  
	msg "Connecting to mercurial repository and pulling changes..."
	if [ -d $_hgrepo ]; then
		cd $_hgrepo
		hg pull -u
	else
		hg clone ${_hgroot}/${_hgrepo}
	fi
	
	msg "Starting make..."
	
	rm -rf "$srcdir/$_hgrepo-build"
	cp -r "$srcdir/$_hgrepo" "$srcdir/$_hgrepo-build"
	cd "$srcdir/$_hgrepo-build"
	
	qmake -o Makefile -after "DEFINES+=VANILLA_QT" "CONFIG+=shared" "VERSION=$qtver" llqtwebkit.pro || return 1
	make || return 1

	install -D -m755 llqtwebkit.h $pkgdir/usr/include/llqtwebkit.h
	install -D -m755 libllqtwebkit.so.$qtver $pkgdir/usr/lib/libllqtwebkit.so.$qtver
	cp -P libllqtwebkit.so* $pkgdir/usr/lib
}

