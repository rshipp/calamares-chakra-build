
pkgname=calamares
pkgver=1.0.1
pkgrel=1
pkgdesc='Distribution-independent installer framework'
arch=('x86_64')
url='https://github.com/calamares/calamares'
license=('LGPL')
depends=('qt5-svg' 'kconfig' 'ki18n' 'kcoreaddons' 'solid' 'yaml-cpp'
         'parted' 'libatasmart' 'udisks2' 'polkit-qt5')
makedepends=('extra-cmake-modules' 'git' 'qt5-tools')
source=("git://github.com/calamares/calamares"
#source=("calamares.tar.xz"
        'kdm_sddm.conf'
        'locale.conf'
        'prepare.conf'
        'settings.conf'
        'launch-calamares.sh'
        'installer.svg'
        'calamares.desktop')
#        'GreetingPage.diff'
#        'CalamaresStyle.diff'
#        'UEFI.diff'
#        'JobQueue.diff'
#        'along_UEFI.diff')
md5sums=('SKIP'
         '68a774a6bddcfdf83c9734dfffeab6c2'
         '6b793a81d051d5d0b00a6031ceb08308'
         '76cf16c8e4347d369330ed64ff28083b'
         '9189f089281bda06b2ca3220554d9497'
         '2437e44479a54376ad9244d120369f6c'
         'd5c65f43e057054e9728810530c4a030'
         '31a21df45f1f6a9fb0aaf0d5418895f2')

prepare () {
  cd ${srcdir}/${pkgname}
  
  git submodule init
  git submodule update
  sed -i 's|Ext4|Xfs|' ${srcdir}/${pkgname}/src/modules/partition/tests/PartitionJobTests.cpp
  sed -i 's|Ext4|Xfs|' ${srcdir}/${pkgname}/src/modules/partition/gui/EraseDiskPage.cpp
  sed -i 's|Ext4|Xfs|' ${srcdir}/${pkgname}/src/modules/partition/gui/ReplacePage.cpp
  #patch -p1 -i ${srcdir}/GreetingPage.diff
  #patch -p1 -i ${srcdir}/CalamaresStyle.diff
  #patch -p1 -i ${srcdir}/UEFI.diff
  #patch -p1 -i ${srcdir}/JobQueue.diff
  #patch -p1 -i ${srcdir}/along_UEFI.diff
}

build() {
  mkdir -p build
  
  cd build
  
  cmake ../${pkgname} \
    -DCMAKE_BUILD_TYPE=Debug \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DWITH_PARTITIONMANAGER=1 \
    -DCMAKE_INSTALL_LIBDIR=lib 
  make
}

package() {
  cd build 
  make DESTDIR="${pkgdir}" install
  
  rm -rf "${pkgdir}/usr/share/calamares/settings.conf"
  install -D -m644 "${srcdir}/settings.conf" "${pkgdir}/usr/share/calamares/settings.conf"
  install -D -m644 "${srcdir}/kdm_sddm.conf" "${pkgdir}/etc/calamares/modules/kdm_sddm.conf"
  install -D -m644 "${srcdir}/locale.conf" "${pkgdir}/etc/calamares/modules/locale.conf"
  install -D -m644 "${srcdir}/prepare.conf" "${pkgdir}/etc/calamares/modules/prepare.conf"
  
  sed 's|linux312|linux|' -i "${pkgdir}/usr/share/calamares/modules/initcpio.conf"
  # toggle for kf5/kde 4 ISO
  #sed 's|KaOS-kf5|KaOS|' -i "${pkgdir}/usr/share/calamares/modules/bootloader.conf"
  
  install -Dm755 "${srcdir}/launch-calamares.sh" "${pkgdir}/usr/bin/launch-calamares.sh"
  install -Dm644 "$srcdir/$pkgname.desktop" "$pkgdir/usr/share/applications/$pkgname.desktop"
  install -Dm644 "${srcdir}/installer.svg" "${pkgdir}/usr/share/pixmaps/installer.svg"
}
