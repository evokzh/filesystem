# Maintainer: Tom Gundersen <teg@jklm.no>

pkgname=filesystem
pkgver=2012.10
pkgrel=1
pkgdesc='Base filesystem'
arch=('any')
license=('GPL')
url='http://www.archlinux.org'
groups=('base')
install='filesystem.install'
makedepends=('asciidoc')
depends=('iana-etc' 'bash' 'coreutils')
backup=('etc/fstab' 'etc/crypttab' 'etc/group' 'etc/hosts' 'etc/ld.so.conf' 'etc/passwd'
        'etc/shadow' 'etc/gshadow' 'etc/resolv.conf' 'etc/motd' 'etc/nsswitch.conf'
        'etc/shells' 'etc/host.conf' 'etc/securetty' 'etc/profile' 'etc/issue')
source=('group' 'issue' 'nsswitch.conf' 'securetty' 'host.conf' 'ld.so.conf'
        'passwd' 'shadow' 'fstab' 'crypttab' 'hosts' 'motd' 'os-release' 'resolv.conf'
        'shells' 'gshadow' 'profile' 'modprobe.d.usb-load-ehci-first' 'archlinux.7.txt'
	'locale.sh')

build() {
	cd ${srcdir}
	a2x -d manpage -f manpage archlinux.7.txt
}

package() {
	cd ${pkgdir}

	#
	# setup root filesystem
	#
	for d in boot dev etc home media mnt usr var opt srv/http run; do
		install -d -m755 ${d}
	done
	install -d -m555 proc
	install -d -m555 sys
	install -d -m0750 root
	install -d -m1777 tmp
	# vsftpd won't run with write perms on /srv/ftp
	install -d -m555 -g ftp srv/ftp

	# setup /etc
	install -d etc/{ld.so.conf.d,skel,profile.d}
	for f in fstab group host.conf hosts issue ld.so.conf motd nsswitch.conf os-release passwd resolv.conf securetty shells profile; do
		install -m644 ${srcdir}/${f} etc/
	done
	ln -s /proc/self/mounts etc/mtab
	for f in gshadow shadow crypttab; do
		install -m600 ${srcdir}/${f} etc/
	done
	touch etc/arch-release
	install -D -m644 ${srcdir}/modprobe.d.usb-load-ehci-first usr/lib/modprobe.d/usb-load-ehci-first.conf
	install -m755 ${srcdir}/locale.sh etc/profile.d/locale.sh

	# setup /var
	for d in cache/man local opt log/old lib/misc empty; do
		install -d -m755 var/${d}
	done
	install -d -m1777 var/{tmp,spool/mail}
	# allow setgid games to write scores
	install -d -m775 -g games var/games
	ln -s spool/mail var/mail
	ln -s ../run var/run
	ln -s ../run/lock var/lock

	#
	# setup /usr hierarchy
	#
	for d in bin include lib sbin share/misc src; do
		install -d -m755 usr/${d}
	done
	for d in $(seq 8); do
		install -d -m755 usr/share/man/man${d}
	done

	#
	# install archlinux(7) manpage
	#
	install -D -m644 ${srcdir}/archlinux.7 usr/share/man/man7/archlinux.7

	#
	# setup /usr/local hierarchy
	#
	for d in bin etc games include lib man sbin share src; do
		install -d -m755 usr/local/${d}
	done
	ln -s ../man usr/local/share/man
}
md5sums=('004013ac940ef3d3cdd8c596e7accfe1'
         '7813c481156f6b280a3ba91fc6236368'
         '13753e4e0964f3652b0cc60a28528bdf'
         '4c4540eeb748bf1f71d631b8c1dcf0b3'
         'f28150d4c0b22a017be51b9f7f9977ed'
         '6e488ffecc8ba142c0cf7e2d7aeb832e'
         '455b78cada80f40b6f6968f5cbd97a2e'
         'b8355d9d2782f424f4cedcf682651be0'
         'ca716f853860199c1286e7939b2f2666'
         '1745349eb24ed21b4cfaa6f423bddb76'
         '7bc65f234dfb6abf24e7c3b03e86f4ff'
         'd41d8cd98f00b204e9800998ecf8427e'
         'b16a4674ccf3a932ff34c6c8393a4f33'
         '6f48288b6fcaf0065fcb7b0e525413e0'
         '22518e922891f9359f971f4f5b4e793c'
         '677523dbe94b79299aa91b35ed8203b6'
         'f3b6ae7db8adffaaa4bffc6099dcbd50'
         'a8a962370cd0128465d514e6a1f74130'
         '640f3bb28b848cb87cfff7063ebeaf42'
         '3807d07215d9116331fe1cf8feeaa0f8')
