# Contributor: David Andersen <archlinux@davidandersen.ws>
# Contributor: Cedric Chabanois <cchabanois@gmail.com>
pkgname=backuppc
pkgver=3.2.1
pkgrel=1
pkgdesc="System for backing up computers to a server's disk."
url="http://backuppc.sourceforge.net/"
license=('GPL')
arch=('i686' 'x86_64')
depends=('apache' 'mod_perl' 'perl' 'perl-libwww' 'perl-compress-zlib' 'perl-archive-zip' 'perl-xml-rss' 'perl-file-rsyncp' 'bzip2' 'tar' 'gzip' 'net-tools' 'par2cmdline' 'iputils' 'rsync' 'openssh')
optdepends=('smbclient')
source=(http://downloads.sourceforge.net/project/backuppc/backuppc/${pkgver}/BackupPC-${pkgver}.tar.gz
	backuppc.rc backuppc-httpd.conf backuppc-httpd.rc)
install=backuppc.install
backup=('etc/BackupPC/config.pl' 'etc/BackupPC/hosts' 'etc/httpd/conf/backuppc-httpd.conf')
md5sums=('2334fafb8e03284225a9b8a7fb230012'
         'bc7402cb4ec4b5f646e2794be67827d1'
         'dab0bb18225d5898dc5ceb9fda39b76c'
         'db7bccc7e704a2ad32fef8d3dfddf736')
build() {
  cd $srcdir/BackupPC-${pkgver}

  install -d -m 755 ${pkgdir}/var/opt

  perl configure.pl \
      --batch \
      --cgi-dir /opt/BackupPC/www/cgi-bin \
      --data-dir /var/opt/BackupPC \
      --hostname __HOSTNAME__ \
      --html-dir /opt/BackupPC/www \
      --html-dir-url /BackupPC \
      --install-dir /opt/BackupPC \
      --dest-dir $pkgdir \
      --no-set-perms \
      --uid-ignore || return 1

  # change the owner as configure.pl would do if we do not use --no-set-perms and backuppc was 91:nobody
  chown 91:nobody -R $pkgdir/etc/BackupPC
  chown 91:nobody -R $pkgdir/opt/BackupPC/{bin,doc,www}
  chown 91:nobody -R $pkgdir/var/{log/BackupPC,opt/BackupPC}
  chmod 640 $pkgdir/etc/BackupPC/config.pl
  chmod 755 $pkgdir/opt/BackupPC/{bin/*,lib/*,www/cgi-bin}
  chmod 754 $pkgdir/opt/BackupPC/www/cgi-bin/BackupPC_Admin

  install -D -m 755 $startdir/src/backuppc.rc $pkgdir/etc/rc.d/backuppc || return 1
  install -D -m 644 $startdir/src/backuppc-httpd.conf $pkgdir/etc/httpd/conf/backuppc-httpd.conf || return 1
  install -D -m 755 $startdir/src/backuppc-httpd.rc $pkgdir/etc/rc.d/backuppc-httpd || return 1

  chmod 755 $pkgdir/{etc,var/log}

}