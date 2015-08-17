# Maintainer: Wido <widowild at myopera dot com>

# a PKGBUILD is now compatible with:

# Official Store http://store.zachtronicsindustries.com/product/spacechem
# Store Gameolith http://www.gameolith.com/game/spacechem/
# The Humble Idie Bundle http://www.humblebundle.com/

pkgname=spacechem
_pkgname=SpaceChem
pkgver=1012
_pkgver=1012-1 # Just for HIB
pkgrel=2
pkgdesc="an obscenely addictive, design-based puzzle game about building machines and fighting monsters in the name of science!"
url="http://www.gameolith.com/game/spacechem/"
license=('custom: "commercial"')
arch=('i686' 'x86_64')
makedepends=()
groups=('humblefsbundle' 'humblebundles')
options=(!strip)
source=('spacechem.sh'
	'spacechem.desktop')
md5sums=('dff2d4a980e324c46198fb9193a23099'
	 'b458899c445a8baa8238b9594ebee6c1')

if [[ $CARCH == x86_64 ]]; then
    _gamepkg="${pkgname}_${pkgver}_amd64.tar.gz"
    _gamepkg2="${_pkgname}-${pkgver}.tar.gz"
    _gamepkg3="${pkgname}-linux-1345144627-amd64.deb"
else
    _gamepkg="${pkgname}_${pkgver}_i386.tar.gz"
    _gamepkg2="${_pkgname}-${pkgver}.tar.gz"
    _gamepkg3="${_pkgname}-i386.deb"
fi

depends=('mono' 'sdl' 'sdl_image' 'sdl_mixer' 'xclip')

build() {
    cd ${srcdir}
    msg "You need a full copy of this game in order to install it"
    msg "Searching for ${_gamepkg} or ${_gamepkg2} or ${_gamepkg3} in dir: \"$srcdir\""
    pkgpath=$srcdir

    if [ ! -f ${_gamepkg3} ] && [ -n "${_humblefsbundlekey}" ]; then
        rm -f ${_archive}* index.html\?key\=${_humblefsbundlekey}*
        wget http://www.humblebundle.com/?key=${_humblefsbundlekey}
        wget $(cat index.html\?key\=${_humblefsbundlekey} | grep "${_gamepkg3}" | cut -d "'" -f 10) && \
           mv ${_gamepkg3}* ${_gamepkg3} || \
           error "The _humblefsbundlekey variable is not supported for users who use the Humble Library!"
    fi

    if [[ ! -f "$srcdir/${_gamepkg}" ]] && [[ ! -f "$srcdir/${_gamepkg2}" ]] && [[ ! -f "$srcdir/${_gamepkg3}" ]]; then
        error "Game package not found, please type absolute path to ${_gamepkg} or ${_gamepkg2} or ${_gamepkg3} (/home/joe):"
        read pkgpath
        if [[ ! -f "${pkgpath}/${_gamepkg}" ]] && [[ ! -f "${pkgpath}/${_gamepkg2}" ]] && [[ ! -f "${pkgpath}/${_gamepkg3}" ]] ; then
            error "Unable to find game package." && return 1
        fi
    fi
    msg "Found game package, installing..."

    ## Store Gameolith

    if [[ -f ${pkgpath}/${_gamepkg} ]] ; then
        tar xvf ${pkgpath}/${_gamepkg} -C ${srcdir} || return 1
        mv ${srcdir}/opt/ ${pkgdir}/

        # Clean package
        rm ${pkgdir}/opt/zachtronicsindustries/${pkgname}/${_gamepkg}
        rm ${pkgdir}/opt/zachtronicsindustries/${pkgname}/${pkgname}.desktop
        rm ${pkgdir}/opt/zachtronicsindustries/${pkgname}/${pkgname}.sh

    fi

    ## Official Store

    if [[ -f ${pkgpath}/${_gamepkg2} ]] ; then
        tar xvf ${pkgpath}/${_gamepkg2} -C ${srcdir} || return 1
        ar x ${srcdir}/${_pkgname}-i386.deb
        tar -zxf ${srcdir}/data.tar.gz
        rm -R ${srcdir}/usr
        mv ${srcdir}/opt/ ${pkgdir}/
    fi

    ## Humble Indie Bundle

    if [[ -f ${pkgpath}/${_gamepkg3} ]] ; then
        ar x ${pkgpath}/${_gamepkg3}
        tar -zxf ${srcdir}/data.tar.gz
        rm -R ${srcdir}/usr
        mv ${srcdir}/opt/ ${pkgdir}/
    fi
}

package(){
    cd ${srcdir}

    # Install Launcher
    install -d ${pkgdir}/usr/bin
    ln -s /opt/zachtronicsindustries/${pkgname}/${pkgname}-launcher.sh ${pkgdir}/usr/bin/${pkgname}

    # Install Desktop
    install -Dm 644 ${pkgdir}/opt/zachtronicsindustries/${pkgname}/zachtronicsindustries-${pkgname}.desktop \
        ${pkgdir}/usr/share/applications/${pkgname}.desktop

    # Install license
    install -Dm 644 ${pkgdir}/opt/zachtronicsindustries/${pkgname}/readme/LICENSE.txt ${pkgdir}/usr/share/licenses/${pkgname}/license
}
