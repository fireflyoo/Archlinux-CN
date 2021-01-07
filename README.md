# yong.PKGBUILD
yong.PKGBUILD是小小输入法的编译脚本，改名为PKGBUILD后执行makepkg一下就行了(makepkg前要安装上make依赖列表里的东西）。然后用sudo pacman -U安装生成的应用安装包。

# 50-udisk.rules
这个是用来修正Arch下用Thunar文件管理器挂载硬盘时还要root权限的问题。  
复制到/etc/polkit-1/rules.d/目录下，然后把在用的账户加入wheel用户组就可以用了(usermod -aG wheel userName)  
参考链接：https://wiki.manjaro.org/index.php/Manjaro_Polkit_Rules
