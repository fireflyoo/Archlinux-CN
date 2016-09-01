# Maintainer: firef <use_my_id at gmail dot com>

pkgname=yong
pkgver=2.4.0
pkgrel=1
pkgdesc="Tiny Small Chinese Input Method"
arch=('i686')
url="https://github.com/dgod/yong"
license=('GPL')

makedepends=('git' 'nodejs' 'gcc' 'ibus' 'wayland' 'gtk2' 'gtk3' 'libxkbcommon')
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
# sed -i -e 's/build(DIRS);/build(DIRS,null,"l32");/' \
#      -e "s/'config',//"                           \
#      -e "s/'im',//"        build.txt

sed -i 's/copy_build("l64");//' install/build.txt
echo 'delete l64 ok'
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
echo 'chang sp ok'
buff=$(sed -n '/\[pinyin\]/,$ p' im/yong.ini)
echo $buff | tr ' ' "\n"
sed -i -e 's/default=0/default=6/' \
       -e 's/3=erbi/3=pinyin/' \
       -e 's/6=pinyin/6=sp/' \
       -e 's/\[pinyin\]/\[sp\]/' \
       -e 's/name=拼音/name=双拼/' \
       -e "s|overlay=mb/pinyin.ini|overlay=mb/sp.ini\nsp=zrm|" \
       -e "s/select=LSHIFT RSHIFT/select=; '/" \
       -e "s/CNen=LCTRL/CNen=LSHIFT/"   \
       -e "s/page=- =/page=, ./"   im/yong.ini

echo 'chang yong.ini ok'

echo $buff | tr ' ' "\n" >> im/yong.ini
echo 'append yong.ini ok'
sed -i 's/"自然码"/"小鹤双拼"/' config/config_ui.c
# change default skin
png="iVBORw0KGgoAAAANSUhEUgAAAB4AAAATCAIAAAAIzCorAAAAAXNSR0IArs4c6QAAAAZiS0dEAP8A
/wD/oL2nkwAAAAlwSFlzAAALEwAACxMBAJqcGAAAAAd0SU1FB9sFCwYzDrySergAAAAhSURBVDjL
Y/z8+TMDbQATA83AqNGjRo8aPWr0qNEj3mgAvhgC/8aR0LcAAAAASUVORK5CYII="
echo $png | base64 -d -i > im/skin/name1.png
sed -i -e 's/s2t=84,3/name=90,6/' \
       -e 's/s2t_s=jian1.png/name_img=name1.png/' \
       -e 's/s2t_t=fan1.png/name_font=Bold 18/' \
       -e 's/keyboard=108,3/name_color=#3975ce/' \
       -e '/keyboard_img/d' im/skin/skin{,0,1,2}.ini
}
build() {

buildjs="$srcdir/build.js/build.js"
 cd $srcdir/$pkgname
    node $buildjs  l32 
   node $buildjs -C install  copy
}

package() {
buildjs="$srcdir/build.js/build.js"
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
