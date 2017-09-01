# Maintainer: Sébastien Luttringer
# Contributor: Tom Gundersen <teg@jklm.no>

pkgname=filesystem
pkgver=2017.08
pkgrel=1
pkgdesc='Base Arch Linux files'
arch=('i686' 'x86_64')
license=('GPL')
url='https://www.archlinux.org'
groups=('base')
makedepends=('systemd')
depends=('iana-etc')
backup=('etc/fstab' 'etc/crypttab' 'etc/group' 'etc/hosts' 'etc/ld.so.conf'
        'etc/passwd' 'etc/shadow' 'etc/gshadow' 'etc/resolv.conf' 'etc/motd'
        'etc/nsswitch.conf' 'etc/shells' 'etc/host.conf' 'etc/securetty'
        'etc/profile' 'etc/issue')
source=('group' 'issue' 'nsswitch.conf' 'securetty' 'host.conf' 'ld.so.conf'
        'passwd' 'shadow' 'fstab' 'crypttab' 'hosts' 'motd' 'os-release'
        'resolv.conf' 'shells' 'gshadow' 'profile' 'locale.sh' 'sysusers'
        'tmpfiles')
md5sums=('7fed1e1fb855e41a6d64d41f8521d69a'
         '7813c481156f6b280a3ba91fc6236368'
         '44851ecc062ba34a4c024b6f3246c48f'
         'f04bcb2803afc4dcb95670fe87343b4d'
         '7d119a9cce152aa182fb3392ddeecea7'
         '5deb9f890a4d08a245e9752ede77271e'
         '5182ac38a0de85da8ade93ef71975ca4'
         'f64466dd77c7bec37a8b47681468211a'
         'e33f6dfdd61978fcb3ddf1431286e05a'
         '5fa6674df7645d7f5895f2d12b4ef4e9'
         'a1315ea3e2b64d197b6efaf9c14ff778'
         'd41d8cd98f00b204e9800998ecf8427e'
         '7756fd3b8876eee095bd6e94ddac13ca'
         '0ee015fad07732676d9488ae498eed41'
         'a78cd8d7f8240a8448edee82f503c34e'
         '1c1e3b08acfa286f4b417c49de3e4366'
         '13feaea89d404729ad2f7cf0bcc41d85'
         '71ed98c52e11ada1f936ac8cb14eecd9'
         '6ec767b80e0df5c4450078363a31bca0'
         '6723590b164b281f471e8b1cd926b633')

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
  install -d -m555 -g ftp srv/ftp

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
  install -Dm644 "$srcdir"/os-release "$pkgdir"/usr/lib/os-release

  # setup /var
  for d in cache local opt log/old lib/misc empty; do
    install -d -m755 var/$d
  done
  install -d -m1777 var/{tmp,spool/mail}

  # allow setgid games to write scores
  install -d -m775 -g games var/games
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
  ln -s usr/lib "$pkgdir"/lib
  [[ $CARCH = 'x86_64' ]] && (
    ln -s usr/lib "$pkgdir"/lib64
    ln -s lib "$pkgdir"/usr/lib64
  )

  # add bin symlinks
  ln -s usr/bin "$pkgdir"/bin
  ln -s usr/bin "$pkgdir"/sbin
  ln -s bin "$pkgdir"/usr/sbin

  # setup /usr/local hierarchy
  for d in bin etc games include lib man sbin share src; do
    install -d -m755 usr/local/$d
  done
  ln -s ../man usr/local/share/man

  # setup systemd-sysusers
  install -D -m644 "$srcdir"/sysusers "$pkgdir"/usr/lib/sysusers.d/arch.conf

  # setup systemd-tmpfiles
  install -D -m644 "$srcdir"/tmpfiles "$pkgdir"/usr/lib/tmpfiles.d/arch.conf
}

# vim:set ts=2 sw=2 et:
