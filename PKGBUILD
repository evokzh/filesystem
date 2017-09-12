# Maintainer: Sébastien Luttringer
# Contributor: Tom Gundersen <teg@jklm.no>

pkgname=filesystem
pkgver=2017.09
pkgrel=3.1
pkgdesc='Base Arch Linux files'
arch=('i686' 'x86_64')
license=('GPL')
url='https://www.archlinux.org'
groups=('base')
depends=('iana-etc')
backup=('etc/crypttab' 'etc/fstab' 'etc/group' 'etc/gshadow' 'etc/host.conf'
        'etc/hosts' 'etc/issue' 'etc/ld.so.conf' 'etc/motd' 'etc/nsswitch.conf'
        'etc/passwd' 'etc/profile' 'etc/resolv.conf' 'etc/securetty'
        'etc/shadow' 'etc/shells')
source=('crypttab' 'env-generator' 'fstab' 'group' 'gshadow' 'host.conf' 'hosts'
        'issue' 'ld.so.conf' 'locale.sh' 'motd' 'nsswitch.conf' 'os-release'
        'passwd' 'profile' 'resolv.conf' 'securetty' 'shadow' 'shells'
        'sysusers' 'tmpfiles')
md5sums=('5fa6674df7645d7f5895f2d12b4ef4e9'
         '2b0344e9639f35f3c0d5637a23556089'
         'e33f6dfdd61978fcb3ddf1431286e05a'
         '803da7c3c9df9b47a78b52fe9ddf02b1'
         '822b75f0faca19a9c4cee334c63ab1b3'
         '7d119a9cce152aa182fb3392ddeecea7'
         'a1315ea3e2b64d197b6efaf9c14ff778'
         '7813c481156f6b280a3ba91fc6236368'
         '5deb9f890a4d08a245e9752ede77271e'
         '71ed98c52e11ada1f936ac8cb14eecd9'
         'd41d8cd98f00b204e9800998ecf8427e'
         '44851ecc062ba34a4c024b6f3246c48f'
         '0a0fbb8e64faabb40023bd180d7190a1'
         'cffabcce564fa9e47981da780059a621'
         '13feaea89d404729ad2f7cf0bcc41d85'
         '0ee015fad07732676d9488ae498eed41'
         'f04bcb2803afc4dcb95670fe87343b4d'
         '7cc0d3e777ccb03f91e979c3aab296a0'
         'a78cd8d7f8240a8448edee82f503c34e'
         '6ec767b80e0df5c4450078363a31bca0'
         '0267a3a463f35eec8a31f40a720dfd86')

package() {
  cd "$pkgdir"

  # setup root filesystem
  for d in boot dev etc home mnt usr var opt srv/http run; do
    install -d -m755 $d
  done
  install -d -m555 proc
  install -d -m555 sys
  install -d -m0750 root
  install -d -m1777 tmp
  # vsftpd won't run with write perms on /srv/ftp
  # ftp (uid 14/gid 11)
  install -d -m555 -g 11 srv/ftp

  # setup /etc and /usr/share/factory/etc
  install -d etc/{ld.so.conf.d,skel,profile.d} usr/share/factory/etc
  for f in fstab group host.conf hosts issue ld.so.conf motd nsswitch.conf passwd resolv.conf securetty shells profile; do
    install -m644 "$srcdir"/$f etc/
    install -m644 "$srcdir"/$f usr/share/factory/etc/
  done
  ln -s ../proc/self/mounts etc/mtab
  for f in gshadow shadow crypttab; do
    install -m600 "$srcdir"/$f etc/
    install -m600 "$srcdir"/$f usr/share/factory/etc/
  done
  touch etc/arch-release
  install -m755 "$srcdir"/locale.sh etc/profile.d/locale.sh
  install -Dm644 "$srcdir"/os-release usr/lib/os-release

  # setup /var
  for d in cache local opt log/old lib/misc empty; do
    install -d -m755 var/$d
  done
  install -d -m1777 var/{tmp,spool/mail}

  # allow setgid games (gid 50) to write scores
  install -d -m775 -g 50 var/games
  ln -s spool/mail var/mail
  ln -s ../run var/run
  ln -s ../run/lock var/lock

  # setup /usr hierarchy
  for d in bin include lib share/misc src; do
    install -d -m755 usr/$d
  done
  for d in {1..8}; do
    install -d -m755 usr/share/man/man$d
  done

  # add lib symlinks
  ln -s usr/lib lib
  [[ $CARCH = 'x86_64' ]] && {
    ln -s usr/lib lib64
    ln -s lib usr/lib64
  }

  # add bin symlinks
  ln -s usr/bin bin
  ln -s usr/bin sbin
  ln -s bin usr/sbin

  # setup /usr/local hierarchy
  for d in bin etc games include lib man sbin share src; do
    install -d -m755 usr/local/$d
  done
  ln -s ../man usr/local/share/man

  # setup systemd-sysusers
  install -D -m644 "$srcdir"/sysusers usr/lib/sysusers.d/arch.conf

  # setup systemd-tmpfiles
  install -D -m644 "$srcdir"/tmpfiles usr/lib/tmpfiles.d/arch.conf

  # setup systemd.environment-generator
  install -D -m755 "$srcdir"/env-generator usr/lib/systemd/system-environment-generators/10-arch
}

# vim:set ts=2 sw=2 et:
