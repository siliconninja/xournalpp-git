# Maintainer : Keshav Amburay <(the ddoott ridikulus ddoott rat) (aatt) (gemmaeiil) (ddoott) (ccoomm)>
# Contributor : Mark Coolen <mark ddoott coolen aatt gmail ddoott ccoomm nniiccee!>
# Contributor : siliconninja <theninja (aattt) thesilicon (dddott) (nnniiinnnja) yaaayy!>

_pkgname="xournalpp"
pkgname="${_pkgname}-git"

pkgver=1.0.0.721.gf021e04a
pkgrel=1
pkgdesc="C++ re-write of tablet notetaking app Xournal"
arch=('i686' 'x86_64')
url="https://github.com/xournalpp/xournalpp"
license=('GPL-2.0')
makedepends=('git' 'cmake' 'gettext' 'boost' 'python')
depends=('texlive-core' 'gtk3' 'boost-libs' 'glib2' 'libglade' 'poppler-glib' 'glibmm' 'desktop-file-utils')
conflicts=('xournalpp')
install="xournalpp.install"
source=("${_pkgname}::git+https://github.com/xournalpp/xournalpp.git"
        "fixpoppler.patch")
sha256sums=('SKIP'
            '84c9a4b921d309dcd2ef22108c5aebff1f4747cca63112e2b1faa7d03ab38a80')

pkgver() {
	cd "${srcdir}/${_pkgname}/"
	git describe --tags | sed -e 's|v||g' -e 's|glib.||g' -e 's|-|.|g'
}

prepare() {
	cd "${srcdir}/${_pkgname}/"
        patch -p1 < ${srcdir}/fixpoppler.patch

	test -e "${srcdir}/${_pkgname}/build" || mkdir -p "${srcdir}/${_pkgname}/build"
	cd "${srcdir}/${_pkgname}/build"

	cmake -DENABLE_OS="OFF" -DENABLE_MATHTEX="ON" -DCMAKE_INSTALL_PREFIX="/usr/" -DBUILD_POPPLER="ON" -DPOPPLER_GIT_VER="0.69.0" ..
}

build() {
	cd "${srcdir}/${_pkgname}/build"
	make
}

package() {
	cd "${srcdir}/${_pkgname}/build"

	# sed 's|/usr/local|/usr|g' -i "${srcdir}/${_pkgname}/build/cmake_install.cmake" || true
	make DESTDIR="${pkgdir}/" install

	mkdir -p "${pkgdir}/usr/share/icons/hicolor/scalable/apps"
	mkdir -p "${pkgdir}/usr/share/icons/hicolor/scalable/mimetypes"

	install -D -m0644 "${srcdir}/${_pkgname}/ui/pixmaps/xournalpp.svg" "${pkgdir}/usr/share/icons/hicolor/scalable/apps/xournalpp.svg"
	install -D -m0644 "${srcdir}/${_pkgname}/ui/pixmaps/xoj.svg" "${pkgdir}/usr/share/icons/hicolor/scalable/mimetypes/xoj.svg"
	ln -s "/usr/share/icons/hicolor/scalable/mimetypes/xoj.svg" "${pkgdir}/usr/share/icons/hicolor/scalable/mimetypes/gnome-mime-application-x-xoj.svg"

	mkdir -p "${pkgdir}/usr/share/mime/packages"
	mkdir -p "${pkgdir}/usr/share/applications"
	mkdir -p "${pkgdir}/usr/share/mimelnk/application"

	install -D -m0644 "${srcdir}/${_pkgname}/desktop/xournal.xml" "${pkgdir}/usr/share/mime/packages"
	install -D -m0644 "${srcdir}/${_pkgname}/desktop/xournalpp.desktop" "${pkgdir}/usr/share/applications"
	install -D -m0644 "${srcdir}/${_pkgname}/desktop/x-xoj.desktop" "${pkgdir}/usr/share/mimelnk/application"
}
