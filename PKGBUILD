# Maintainer: Michael Lass <bevan@bi-co.net>

# This PKGBUILD heavily borrows from java-openjdk in [extra] maintained by:
# Levente Polyak <anthraxx[at]archlinux[dot]org>
# Guillaume ALAUX <guillaume@archlinux.org>

# This PKGBUILD is maintained on github:
# https://github.com/michaellass/AUR

_majorver=17
_completever=17.0.9
_updatever=9
_openj9ver=0.41.0
pkgrel=2
pkgver=${_completever}.u${_updatever}
_tag_ver=${_completever}+${_updatever}
[ $_majorver != $_completever ] && _versuffix=U

pkgname=jdk17-semeru
pkgdesc="Semeru ${_majorver} (OpenJDK ${_majorver} Java binaries by IBM)"
arch=('x86_64')
url='https://developer.ibm.com/'
license=('custom')

depends=('java-runtime-common>=3' 'java-environment-common' 'ca-certificates-utils' 'desktop-file-utils' 'libxrender' 'libxtst' 'alsa-lib')
optdepends=('gtk2: for the Gtk+ 2 look and feel'
            'gtk3: for the Gtk+ 3 look and feel')
provides=("java-runtime-headless=${_majorver}"
          "java-runtime-headless-openjdk=${_majorver}"
          "jre${_majorver}-openjdk-headless=${pkgver}"
          "java-runtime=${_majorver}"
          "java-runtime-openjdk=${_majorver}"
          "jre${_majorver}-openjdk=${pkgver}"
          "java-environment=${_majorver}"
          "java-environment-openjdk=${_majorver}"
          "jdk${_majorver}-openjdk=${pkgver}"
          "jdk-openjdk=${pkgver}"
          "openjdk${_majorver}-src=${pkgver}"
          "openjdk-src=${pkgver}")
backup=(etc/java-${_majorver}-semeru/logging.properties
        etc/java-${_majorver}-semeru/management/jmxremote.access
        etc/java-${_majorver}-semeru/management/jmxremote.password.template
        etc/java-${_majorver}-semeru/management/management.properties
        etc/java-${_majorver}-semeru/net.properties
        etc/java-${_majorver}-semeru/sdp/sdp.conf.template
        etc/java-${_majorver}-semeru/security/java.policy
        etc/java-${_majorver}-semeru/security/java.security
        etc/java-${_majorver}-semeru/security/policy/limited/default_local.policy
        etc/java-${_majorver}-semeru/security/policy/limited/default_US_export.policy
        etc/java-${_majorver}-semeru/security/policy/limited/exempt_local.policy
        etc/java-${_majorver}-semeru/security/policy/README.txt
        etc/java-${_majorver}-semeru/security/policy/unlimited/default_local.policy
        etc/java-${_majorver}-semeru/security/policy/unlimited/default_US_export.policy
        etc/java-${_majorver}-semeru/sound.properties)
install=install_jdk17-semeru.sh
options=(!strip)

source=(https://github.com/ibmruntimes/semeru17-binaries/releases/download/jdk-${_completever}%2B9_openj9-${_openj9ver}/ibm-semeru-open-jdk_x64_linux_${_completever}_${_updatever}_openj9-${_openj9ver}.tar.gz
        freedesktop-java.desktop
        freedesktop-jconsole.desktop
        freedesktop-jshell.desktop)
sha256sums=('9b945e58f024108a20eb907015cca4a452332b7644e8dd8e051149a3ec62e3a3'
            '99c65b670fedd57eabfdb341b4ebbca45e8b692c95ffece97a6f88e80f9019f0'
            'a9d800cb5bbcd16c7dad7e003a8a816c961397f210f8210a5019701d8e229186'
            '5e3423e36723397700d8991287c85210420e6ef44a2543f11bf3f1040bd22cb5')

_jvmdir=/usr/lib/jvm/java-${_majorver}-semeru
_jdkdir=jdk-${_tag_ver}

package() {

  install -dm 755 "${pkgdir}${_jvmdir}"
  cp -a "${srcdir}/${_jdkdir}"/* "${pkgdir}${_jvmdir}"

  cd "${pkgdir}${_jvmdir}"

  # Conf
  install -dm 755 "${pkgdir}/etc"
  mv conf "${pkgdir}/etc/java-${_majorver}-semeru"
  ln -sf /etc/java-${_majorver}-semeru conf

  # Legal
  install -dm 755 "${pkgdir}/usr/share/licenses"
  mv legal "${pkgdir}/usr/share/licenses/${pkgname}"
  ln -sf /usr/share/licenses/${pkgname} legal

  # Man pages
  for f in man/man1/*; do
    install -Dm 644 "${f}" "${pkgdir}/usr/share/${f/\.1/-semeru${_majorver}.1}"
  done
  rm -rf man
  ln -sf /usr/share/man man

  # Link JKS keystore from ca-certificates-utils
  rm -f lib/security/cacerts
  ln -sf /etc/ssl/certs/java/cacerts lib/security/cacerts

  # Desktop files
  for f in jconsole java jshell; do
    install -Dm 644 \
      "${srcdir}/freedesktop-${f}.desktop" \
      "${pkgdir}/usr/share/applications/${f}-${pkgname}.desktop"
  done

}
