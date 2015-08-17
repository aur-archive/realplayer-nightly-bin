# Maintainer: Limao Luo <luolimao+AUR@gmail.com>
# Contributor: Jonathan Wiersma <arch aur@jonw.org>

pkgname=realplayer-nightly-bin
_pkgname=realplay
pkgver=11.1.1.2953
_pkgdate=20100721
_pkgdlhash=297b549c4d0c10586916
pkgrel=2
pkgdesc="Real Media Player produced by RealNetworks"
arch=(i686 x86_64)
_arch=linux-2.2-libc6-gcc32-i586
url=http://forms.helixcommunity.org/helix/builds/
license=(custom)
depends=(desktop-file-utils hicolor-icon-theme lib32-{alsa-lib,gtk2,libxv,libstdc++5})
[[ $CARCH == "i686" ]] && depends=(${depends[@]/lib32-/})
provides=(realplayer=$pkgver)
conflicts=({bin32-,}realplayer helixplayer{,-nightly-bin})
options=(!strip)
install=$_pkgname.install
source=($pkgname.tar.bz2::http://software-dl.real.com/$_pkgdlhash/helix/$_pkgdate/player_all-${_pkgname}_gtk_current-$_pkgdate-$_arch/$_pkgname-$pkgver-$_arch.tar.bz2 $_pkgname.desktop)
noextract=($pkgname.tar.bz2)
sha256sums=('7f223bf283db92c9990bdcc0503fd7008662af53955ce3cf8609b1975ce257e0'
    'fe640fb6ba5920514a39e3adcb90cb5fd44a019aafe4c67e8dcdabd6b4ee540a')
sha512sums=('4a838183e617649a014695e8fac297b87d94f60abf294f47ab32f3cafaef5507786e85fe9cdc94d358996c91a07c6bd3a249e503e74b41a14da539da03afd296'
    'c93a50215bebe0b63948bd18d6cc16462bdcaa996a471d1f21be668f1e9075e9eae7096eb9eb97a3473c88f350c3c30303da76087e2da12b05509767ea2b226f')

package() {
    install -d "$srcdir"/$pkgname/
    cd "$srcdir"/$pkgname/
    bsdtar -xf ../$pkgname.tar.bz2

    # Install shared libs
    for _file in {codecs,common,plugins}/*.so; do
        install -Dm755 $_file "$pkgdir"/opt/$pkgname/$_file
    done

    # Install executables
    install -Dm755 $_pkgname $_pkgname.bin "$pkgdir"/opt/$pkgname/
    install -Dm755 Bin/setup $"pkgdir"/opt/$pkgname/bin/setup
    install -d "$pkgdir"/usr/bin/
    ln -s /opt/$pkgname/$_pkgname "$pkgdir"/usr/bin/$_pkgname

    # Install mozilla plugins
    install -d "$pkgdir"/usr/lib/mozilla/plugins/
    install -Dm755 mozilla/*.{so,xpt} "$pkgdir"/usr/lib/mozilla/plugins/

    # Install License and extras
    install -Dm644 LICENSE "$pkgdir"/usr/share/licenses/$pkgname/LICENSE
    install -m644 LICENSE README "$pkgdir"/opt/$pkgname/

    ## Time to SHARE
    cd share/
    # Install selected files from share directory
    find -type f -execdir install -Dm644 '{}' "$pkgdir"/opt/$pkgname/'{}' \;

    for _file in locale/*/{LICENSE,README}; do
        install -Dm644 $_file "$pkgdir"/opt/$pkgname/$_file
    done

    # Install Icons
    for _res in 16 192 32 48; do
        install -Dm644 icons/${_pkgname}_${_res}x$_res.png "$pkgdir"/usr/share/icons/hicolor/${_res}x$_res/apps/$_pkgname.png
    done

    # Install desktop extras
    install -Dm644 $_pkgname.png "$pkgdir"/usr/share/pixmaps/$_pkgname.png
    install -Dm644 $_pkgname.applications "$pkgdir"/usr/share/application-registry/$_pkgname.applications
    desktop-file-install ../../$_pkgname.desktop --dir "$pkgdir"/usr/share/applications/

    install -d "$pkgdir"/usr/share/mime-info/
    install -m644 $_pkgname.{keys,mime} "$pkgdir"/usr/share/mime-info/

    # Install Locales
    cd locale/
    for _locale in *; do
        _loc="$pkgdir"/usr/share/locale/$_locale/LC_MESSAGES/
        install -d "$_loc"
        install -m644 $_locale/*.mo "$_loc"
    done
}
