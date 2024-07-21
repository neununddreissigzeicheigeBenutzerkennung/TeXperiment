# Maintainer: ukofk 2536076 ukofk@student.kit.edu
pkgname=texperiment
pkgver=1
pkgrel=1
pkgdesc="An idiot's experiments envolving the TeX typesetting engine and plain.tex format."
arch=(any)
url='https://github.com/neununddreissigzeicheigeBenutzerkennung/TeXperiment'
license=('GLWTS')
depends=('texlive-core')
makedepends=('git')
source=("$pkgname::git+https://github.com/neununddreissigzeicheigeBenutzerkennung/TeXperiment#branch=main")
md5sums=('SKIP')
prepare() {
    mv "$srcdir/$pkgname/userpackages" "$srcdir/$pkgname/$pkgname"
    touch "$srcdir/$pkgname/atexperiment.tex"
    echo "\input plain.tex" >> "$srcdir/$pkgname/atexperiment.tex"
    for file in "$srcdir/$pkgname/$pkgname"/*.tex; do
	local package_name=$(basename "$file")
	echo "\input $package_name" >> "$srcdir/$pkgname/atexperiment.tex"
    done
    echo "\dump" >> "$srcdir/$pkgname/atexperiment.tex"
    touch "$srcdir/atex"
    echo "#!/bin/bash" >> "$srcdir/atex"
    echo "pdftex -fmt=atexperiment '$@'" >> "$srcdir/atex"
}
package() {
    texmf_dist=$(kpsewhich -var-value=TEXMFDIST)
    install -d "$pkgdir/usr/share/licenses/$pkgname"
    install -Dm644 "$srcdir/$pkgname/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
    install -d "$pkgdir$texmf_dist/tex/generic"
    cp -r "$srcdir/$pkgname/$pkgname" "$pkgdir$texmf_dist/tex/generic"
    cd "$srcdir/$pkgname/$pkgname"
    initex "../atexperiment.tex"
    cd "../../.."
    install -d "$pkgdir$texmf_dist/$pkgname"
    cp "$srcdir/$pkgname/$pkgname/atexperiment.fmt" "$pkgdir$texmf_dist/$pkgname/atexperiment.fmt"
    install -d "$pkgdir/usr/bin"
    cp "$srcdir/atex" "$pkgdir/usr/bin/atex"
    chmod +x "$pkgdir/usr/bin/atex"
}
post_install() {
    mktexlsr "$texmf_dist"
}
post_remove() {
    mktexlsr "$texmf_dist"
}
post_upgrade() {
    mktexlsr "$texmf_dist"
}
