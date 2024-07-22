# Maintainer: ukofk 2536076 ukofk@student.kit.edu
pkgname=texperiment
pkgver=0
pkgrel=0
pkgdesc="An idiot's experiments envolving the TeX typesetting engine and plain.tex format."
arch=(any)
url=https://github.com/neununddreissigzeicheigeBenutzerkennung/TeXperiment
license=(GLWTS)
depends=(texlive-core)
makedepends=(git)
source=($pkgname::git+https://github.com/neununddreissigzeicheigeBenutzerkennung/TeXperiment#branch=main)
md5sums=(SKIP)
texmf_dist=$(kpsewhich -var-value=TEXMFDIST)
prepare() {
    touch $srcdir/atexperiment.tex
    echo "\input plain.tex" >> $srcdir/atexperiment.tex
    for texfile in $srcdir/$pkgname/userpackages/*.tex; do
	local filename=$(basename $texfile)
	echo "\input $filename" >> $srcdir/atexperiment.tex
    done
    echo "\dump" >> $srcdir/atexperiment.tex
    touch $srcdir/atex
    echo "#!/bin/bash" >> $srcdir/atex
    echo 'tex -fmt=atexperiment "$@"' >> $srcdir/atex
}
build() {
    cd $srcdir/$pkgname/userpackages
    initex -output-directory=$srcdir $srcdir/atexperiment.tex
    cd ../../..
}
package() {
    mkdir -p $pkgdir/usr/share/licenses/$pkgname
    mkdir -p $pkgdir$texmf_dist/tex/generic
    mkdir -p $pkgdir$texmf_dist/web2c/tex
    mkdir -p $pkgdir/usr/bin
    cp $srcdir/$pkgname/LICENSE $pkgdir/usr/share/licenses/$pkgname/LICENSE
    cp -r $srcdir/$pkgname/userpackages $pkgdir$texmf_dist/tex/generic/$pkgname
    cp $srcdir/atexperiment.fmt $pkgdir$texmf_dist/web2c/tex/atexperiment.fmt
    cp $srcdir/atex $pkgdir/usr/bin/atex
    chmod +x $pkgdir/usr/bin/atex
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
