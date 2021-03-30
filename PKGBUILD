_pkgname="ofono"
pkgname="$_pkgname-git"
pkgver=r8870.59d47a28
pkgrel=1
pkgdesc="Infrastructure for building mobile telephony (GSM/UMTS) applications"
url="https://01.org/ofono"
arch=("x86_64" "aarch64")
license=("GPL2")
depends=("bluez" "dbus" "modemmanager" "glib2" "udev" "mobile-broadband-provider-info")
provides=("$_pkgname")
source=(
	"git+https://github.com/msm8953-mainline/ofono.git"
	"git+https://git.kernel.org/pub/scm/libs/ell/ell.git"
)
sha256sums=(
	"SKIP"
	"SKIP"
)

pkgver() {
  cd "$_pkgname"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
	cd "$_pkgname"
	autoreconf -fi
	./configure \
		--prefix=/usr \
		--sysconfdir=/etc \
		--sbindir=/usr/bin \
		--disable-bluez4 \
		--enable-tools \
		--host=aarch64-linux-gnu
	make CFLAGS=-DHAVE_RAWMEMCHR
}

package() {
	cd "$_pkgname"
	make DESTDIR="$pkgdir" install
	install -Dm644 "$srcdir/$_pkgname/src/ofono.conf" "$pkgdir/etc/dbus-1/system.d/ofono.conf"
	install -Dm644 "$srcdir/$_pkgname/src/ofono.service" "$pkgdir/usr/lib/systemd/system/ofono.service"
	install -Dm755 "$srcdir/$_pkgname/tools/auto-enable" "$pkgdir/usr/bin/ofono-auto-enable"
	mkdir -p "$pkgdir/usr/lib/ofono/test"
	install -Dm755 "$srcdir/$_pkgname/test/"* "$pkgdir/usr/lib/ofono/test"
	install -Dm644 "$srcdir/$_pkgname/plugins/ofono.rules" "$pkgdir/usr/lib/udev/rules.d/97-ofono.rules"
}
