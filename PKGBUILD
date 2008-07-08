# Maintainer: Aaron Griffin <aaron@archlinux.org>
# Contributor: judd <jvinet@zeroflux.org>

pkgname=filesystem
pkgver=2008.07
pkgrel=1
pkgdesc="Base filesystem"
arch=(i686 x86_64)
license=('GPL')
url="http://www.archlinux.org"
groups=('base')
install=filesystem.install
#depends=('sh' 'coreutils')
backup=(etc/fstab etc/crypttab etc/group etc/hosts etc/ld.so.conf etc/passwd \
        etc/shadow etc/gshadow etc/resolv.conf etc/motd etc/nsswitch.conf \
        etc/shells etc/host.conf etc/securetty etc/profile etc/issue)
source=(group issue nsswitch.conf securetty host.conf ld.so.conf \
        passwd shadow fstab crypttab hosts motd resolv.conf shells \
        gshadow services protocols profile)

build()
{
  cd $startdir/pkg
  mkdir -p bin boot dev etc home lib mnt proc root sbin tmp usr var opt srv sys
  chmod 555 proc
  mkdir -p media/{fl,cd,dvd}
  mkdir -p usr/{bin,include,lib,sbin,share/misc,src,man}

  mkdir -p usr/share/man/man{1,2,3,4,5,6,7,8}
  ln -s man3 $startdir/pkg/usr/share/man/man3x

  # fhs compliance
  mkdir -p usr/local/{bin,games,include,lib,man,sbin,share,src}
  ln -s ../man $startdir/pkg/usr/local/share/man
  mkdir -p var/{cache/man,local,lock,opt,run,spool/mail,tmp,games}
  chmod 1777 var/lock
  mkdir -p var/log/old
  mkdir -p etc/{skel,profile.d}
  mkdir -p lib/modules
  (cd $startdir/pkg/usr; ln -s ../var var)
  (cd $startdir/pkg/var; ln -s spool/mail mail)

  # vsftpd won't run with write perms on /srv/ftp
  mkdir -p srv/ftp
  chown root.ftp srv/ftp
  chmod 555 srv/ftp

  install -d -o root -g root -m 755 srv/http

  chmod 1777 var/spool/mail tmp var/tmp
  chmod 0750 root

  #Allow setgid games to write scores:
  chmod 775 ${startdir}/pkg/var/games
  chown root:50 ${startdir}/pkg/var/games

  cd $startdir/src
  cp fstab crypttab group host.conf hosts issue ld.so.conf motd nsswitch.conf \
    passwd protocols resolv.conf securetty services shadow shells profile \
    $startdir/pkg/etc/
  install -m 600 $startdir/src/gshadow $startdir/pkg/etc/gshadow
  chmod 600 $startdir/pkg/etc/shadow
  chmod 600 $startdir/pkg/etc/crypttab

  # no version any more
  #cat issue | sed "s/#VERSION#/$pkgver/" >$startdir/pkg/etc/issue

  # re-add /etc/arch-release, some software uses it
  # to check whether arch is running
  touch $startdir/pkg/etc/arch-release
}
md5sums=('f64f86c4a6356961b69ead0471294145'
         '1bdc5dba66947d74866a5df8ce9ef3b1'
         '775464ba7588b4976e0c2a02b83123f4'
         '655071da46d2ac03e0fb8a071bf193ea'
         'f28150d4c0b22a017be51b9f7f9977ed'
         '2c24792d97ef3cf0d73b60d4c429730b'
         '8a9042a2cedf6b6b47eb8973f14289cb'
         '019e5c24f9befef395a28e7ef2e4e5b9'
         '4e2f238bae5cbf716ff73c9404404269'
         'e5d8323a4dbee7a6d0d2a19cbf4b819f'
         '81b3cb42a6ddabc2ed2310511ee9c859'
         'd41d8cd98f00b204e9800998ecf8427e'
         '6f48288b6fcaf0065fcb7b0e525413e0'
         '40dac0de4c6b99c8ca97effbd7527c84'
         'ab9c2a40eba287b2918589ab8e0b2fbf'
         'f436d2e0ed02b7b73bd10c6693e95ac3'
         '65d78e621ed69eed69f854c3ee2e5942'
         'f2a88eacb5c37201368c916d9e594440')
