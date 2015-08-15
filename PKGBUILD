# Maintainer: Doug Newgard <scimmia22 at outlook dot com>
# Contributor: Ronald van Haren <ronald.archlinux.org>

pkgname=emotion-svn
pkgver=82099
pkgrel=1
pkgdesc="Video and audio codec API in EFL"
arch=('i686' 'x86_64')
url="http://www.enlightenment.org"
license=('BSD')
depends=('gstreamer0.10-base-plugins' 'efl-svn')
makedepends=('subversion')
optdepends=('gstreamer0.10-good-plugins: Access more types of video'
            'gstreamer0.10-bad-plugins: Access more types of video'
            'gstreamer0.10-ugly-plugins: Access more types of video'
            'gstreamer0.10-ffmpeg: Access video with ffmpeg')
conflicts=('emotion')
provides=('emotion')
options=(!libtool)

_svntrunk="http://svn.enlightenment.org/svn/e/trunk/emotion"
_svnmod="emotion"

build() {
  cd "$srcdir"

  msg "Connecting to SVN server...."

  if [[ -d "$_svnmod/.svn" ]]; then
    (cd "$_svnmod" && svn up -r "$pkgver")
  else
    svn co "$_svntrunk" --config-dir ./ -r "$pkgver" "$_svnmod"
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_svnmod-build"
  svn export "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
  cd "$srcdir/$_svnmod-build"

  ./autogen.sh --prefix=/usr

  make
}

package(){
  cd "$_svnmod-build"
  make DESTDIR="$pkgdir" install
  
  # install license files
  install -Dm644 "$srcdir/$_svnmod-build/COPYING" \
	"$pkgdir/usr/share/licenses/$pkgname/COPYING"

  install -Dm644 "$srcdir/$_svnmod-build/AUTHORS" \
	"$pkgdir/usr/share/licenses/$pkgname/AUTHORS"
  
  rm -r "$srcdir/$_svnmod-build"
}
