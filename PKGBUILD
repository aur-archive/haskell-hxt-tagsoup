# custom variables
_hkgname=hxt-tagsoup
_licensefile=LICENSE

# PKGBUILD options/directives
pkgname=haskell-hxt-tagsoup
pkgver=9.1.0
pkgrel=3
pkgdesc="TagSoup parser for HXT"
url="http://www.fh-wedel.de/~si/HXmlToolbox/index.html"
license=("OtherLicense")
arch=('i686' 'x86_64')
makedepends=()
depends=("ghc=7.0.3-2"
         "haskell-hxt=9.1.4-2"
         "haskell-hxt-charproperties=9.1.0-18"
         "haskell-hxt-unicode=9.0.1-18"
         "haskell-tagsoup=0.12.4-1")
options=('strip')
source=("http://hackage.haskell.org/packages/archive/${_hkgname}/${pkgver}/${_hkgname}-${pkgver}.tar.gz")
install="${pkgname}.install"
sha256sums=("10251ce362cfac477ccd63f8ad698a4ac647e6e1d1a29e3b5c38bc726fdfc27a")

# PKGBUILD functions
build() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    
    runhaskell Setup configure -O -p --enable-split-objs --enable-shared \
        --prefix=/usr --docdir=/usr/share/doc/${pkgname} \
        --libsubdir=\$compiler/site-local/\$pkgid
    runhaskell Setup build
    runhaskell Setup haddock
    runhaskell Setup register --gen-script
    runhaskell Setup unregister --gen-script
    sed -i -r -e "s|ghc-pkg.*unregister[^ ]* |&'--force' |" unregister.sh
}

package() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    install -D -m744 register.sh   ${pkgdir}/usr/share/haskell/${pkgname}/register.sh
    install    -m744 unregister.sh ${pkgdir}/usr/share/haskell/${pkgname}/unregister.sh
    install -d -m755 ${pkgdir}/usr/share/doc/ghc/html/libraries
    ln -s /usr/share/doc/${pkgname}/html ${pkgdir}/usr/share/doc/ghc/html/libraries/${_hkgname}
    runhaskell Setup copy --destdir=${pkgdir}
    install -D -m644 ${_licensefile} ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
    rm -f ${pkgdir}/usr/share/doc/${pkgname}/${_licensefile}
}
