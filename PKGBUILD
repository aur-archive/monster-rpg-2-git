# Maintainer: Jonas Karlsson <jonaskarlsson@fripost.org>
pkgname=monster-rpg-2-git
pkgver=20130327
pkgrel=1
epoch=
pkgdesc="A console style role playing game (perhaps most accurately, a JRPG)."
arch=('i386' 'x86_64')
url="http://www.nooskewl.com"
license=('custom')
groups=()
depends=('allegro-git' 'lua' 'openal' 'curl')
makedepends=('make' 'cmake' 'git')
checkdepends=()
optdepends=()
provides=()
conflicts=()
replaces=()
backup=()
options=()
install=
changelog=
source=('monster-rpg-2.desktop')
noextract=()
md5sums=('f4161e8b41956b513bfb2f1a6cd62ba9')


_gitroot=git://nooskewl.com/monster-rpg-2.git
_gitname=monster-rpg-2-git

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [[ -d "$_gitname" ]]; then
    cd "$_gitname" && git pull origin
    msg "The local files are updated."
  else
    git clone "$_gitroot" "$_gitname"
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_gitname-build"
  git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  # sed patching data directory to /usr/share/monster-rpg-2
  cd "$srcdir/$_gitname-build"
  sed "s/sprintf(result,\ \"%s\/data\/\", tmp)/\
       sprintf(result,\ \"\/usr\/share\/monster-rpg-2\/data\/\", tmp)/" \
       -i $srcdir/$_gitname-build/src/util.cpp 

  # sed patching lua5.2 includes to lua
  sed "s/include <lua5.2\//include </" \
      -i $srcdir/$_gitname-build/include/monster2.hpp

  sed "s/lua5.2/lua/" \
      -i $srcdir/$_gitname-build/CMakeLists.txt

  mkdir build && cd build
  cmake .. -DKCM_AUDIO=on -DUSER_INCLUDE_PATH="/usr/include" \
           -DUSER_LIBRARY_PATH="/usr/lib/"
  make
}

package() {
  cd "$srcdir/$_gitname-build"
  install -D -m755 build/monster2 "${pkgdir}/usr/bin/monster-rpg-2"

  # Install game files
  mkdir -p "${pkgdir}/usr/share/monster-rpg-2"
  cp -r data "${pkgdir}/usr/share/monster-rpg-2/data"

  # Install a desktop entry
  cd $srcdir
  install -Dm644 $_gitname-build/icon512.png "${pkgdir}/usr/share/pixmaps/monster-rpg-2.png"
  install -Dm644 monster-rpg-2.desktop "${pkgdir}/usr/share/applications/monster-rpg-2.desktop"

  # Install documentation
  install -Dm644 $_gitname-build/README.txt "${pkgdir}/usr/share/doc/$pkgname/README"
  install -Dm644 $_gitname-build/LICENSE.txt "${pkgdir}/usr/share/licenses/$pkgname/LICENSE"
}

# vim:set ts=2 sw=2 et:
