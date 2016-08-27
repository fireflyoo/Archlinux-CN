# Maintainer: firef <use_my_id at gmail dot com>

pkgname=yong
pkgver=2.4.0
pkgrel=1
pkgdesc="Tiny Small Chinese Input Method"
arch=('i686')
url="https://github.com/dgod/yong"
license=('GPL')

makedepends=('git' 'nodejs' 'gcc' 'ibus' 'wayland')
#provides=("yong=$pkgver")

depends=('pango')

#install=yong.install

source=("git+https://github.com/dgod/yong.git" "git+https://github.com/dgod/build.js.git")
md5sums=('SKIP' 'SKIP')


pkgver() {
    cd "$srcdir/$pkgname/install"
    awk '/%define +version/{print $3}' yong.spec | sed 's|-|.|g'
}
prepare(){
    cd  $srcdir/$pkgname
    mkdir -p {llib,cloud,gbk,mb,vim}/l32
    mkdir -p {im,config}/{l32-gtk3,l32-gtk2}
    mkdir -p im/gtk-im/{l32-gtk3,l32-gtk2}
    mkdir -p im/IMdkit/l32
sed -i -e 's/build(DIRS);/build(DIRS,null,"l32");/' \
       -e "s/'config',//"                           \
       -e "s/'im',//"        build.txt

sed -i 's/copy_build("l64");//' install/build.txt
# modify shuangping rules
sed -i -r  -e "s/'p'(,[1|2],\"\wh{0,1}un\")/'y'\1/g" \
-e "s/'d'(,[1|2],\"\wh{0,1}uang\")/'l'\1/g" \
-e "s/'d'(,[1|2],\"\wh{0,1}iang\")/'l'\1/g" \
-e "s/'y'(,[1|2],\"\wh{0,1}uai\")/'k'\1/g"  \
-e "s/'y'(,[1|2],\"\wh{0,1}ing\")/'k'\1/g" \
-e "s/'w'(,[1|2],\"\wh{0,1}ua\")/'x'\1/g"  \
-e "s/'w'(,[1|2],\"\wh{0,1}ia\")/'x'\1/g"  \
-e "s/'b'(,[1|2],\"\wh{0,1}ou\")/'z'\1/g"  \
-e "s/'z'(,[1|2],\"\wh{0,1}ei\")/'w'\1/g"  \
-e "s/'k'(,[1|2],\"\wh{0,1}ao\")/'c'\1/g"  \
-e "s/'l'(,[1|2],\"\wh{0,1}ai\")/'d'\1/g"  \
-e "s/'n'(,[1|2],\"\wh{0,1}in\")/'b'\1/g"  \
-e "s/'x'(,[1|2],\"\wh{0,1}ie\")/'p'\1/g"  \
-e "s/'c'(,[1|2],\"\wh{0,1}iao\")/'n'\1/g"  common/pinyin.c
cp -f common/pinyin.c cloud/pinyin.c
sed -i -e 's/default=0/default=6/' \
       -e "s|overlay=mb/pinyin.ini|overlay=mb/sp.ini\nsp=zrm|" \
       -e "s/select=LSHIFT RSHIFT/select=; '/" \
       -e "s/CNen=LCTRL/CNen=LSHIFT/"   \
       -e "s/page=- =/page=, ./"   \
im/yong.ini
sed -i 's/"自然码"/"小鹤双拼"/' config/config_ui.c
}
build() {

buildjs="$srcdir/build.js/build.js"
    cd "$srcdir/$pkgname/im/IMdkit"
    node $buildjs  l32 
    cd "$srcdir/$pkgname/"
    node $buildjs
    cd $srcdir/$pkgname/im/
    node $buildjs  {l32-gtk3,l32-gtk2}
    cd $srcdir/$pkgname/im/gtk-im/
    node $buildjs  {l32-gtk3,l32-gtk2}
    cd $srcdir/$pkgname/config/
    node $buildjs  {l32-gtk3,l32-gtk2}
    cd "$srcdir/$pkgname/install/"
   node $buildjs   copy
}

package() {
   mkdir -p $pkgdir/usr/bin
   cp -a $srcdir/$pkgname/install/yong $pkgdir/usr
   cd $pkgdir/usr/yong
   ln -sf ../yong/l32/yong-gtk3 $pkgdir/usr/bin/yong
   ln -sf ../yong/l32/yong-config-gtk3 $pkgdir/usr/bin/yong-config
   install -D locale/zh_CN.mo $pkgdir/usr/share/locale/zh_CN/LC_MESSAGES/yong.mo
   install -D l32/gtk-im/im-yong-gtk2.so $pkgdir/usr/lib/gtk-2.0/2.10.0/immodules/im-yong.so
   install -D l32/gtk-im/im-yong-gtk3.so $pkgdir/usr/lib/gtk-3.0/3.0.0/immodules/im-yong.so
    
}
post_install() {
  gtk-query-immodules-2.0 --update-cache 
  gtk-query-immodules-3.0 --update-cache 
}

post_remove() {
  gtk-query-immodules-2.0 --update-cache 
  gtk-query-immodules-3.0 --update-cache 
}
