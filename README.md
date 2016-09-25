# VestaCP - MultiPHP
## Abhängigkeiten installieren
```bash
apt-get install libssl-dev libxml2-dev pkg-config libssl-dev libsslcommon2-dev libbz2-dev libcurl3-dev libmysqlclient15-dev libgdbm-dev libjpeg62 libjpeg62-dev libpng12-0 libpng12-dev libxml2 libxml2-dev libmcrypt4 libmcrypt-dev libmhash2 libmhash-dev libmm-dev libmm14 libtidy-dev libtidy-0.99-0 libxslt1-dev libxslt1.1 libfreetype6 libfreetype6-dev libt1-dev libicu-dev libreadline-dev

mkdir /usr/include/freetype2/freetype
ln -s /usr/include/freetype2/freetype.h /usr/include/freetype2/freetype/freetype.h
```
## PHPBrew installieren
curl -L -O https://github.com/phpbrew/phpbrew/raw/master/phpbrew
chmod +x phpbrew
sudo mv phpbrew /usr/bin/phpbrew

mkdir -p /usr/local/php
phpbrew init --root=/usr/local/php
export PHPBREW_ROOT=/usr/local/php
[[ -e ~/.phpbrew/bashrc ]] && source ~/.phpbrew/bashrc

phpbrew update
phpbrew update --old

## PHP Pakete kompilieren
phpbrew install 5.3 +default +openssl=shared -- --with-openssl-dir=/usr/include/openssl --with-mysql-sock=/var/run/mysqld/mysqld.sock --with-mysql=mysqlnd --with-mysqli=mysqlnd --enable-pdo --with-pdo-mysql=mysqlnd --enable-exif --with-jpeg-dir=/usr --with-png-dir=/usr --with-freetype-dir=/usr --with-t1lib --with-zlib-dir=/usr --with-mcrypt=/usr --with-mhash --with-xsl=/usr --enable-zip --enable-cgi --with-curl --with-gd --enable-pcntl --enable-mbregex --enable-gd-native-ttf --with-libdir=lib64 --enable-dba=shared --enable-intl --with-readline=/usr --enable-simplexml \--enable-soap --enable-zip --with-mhash=yes --enable-shmop --enable-sockets --enable-wddx --enable-calendar --enable-sysvsem --enable-sysvshm --enable-sysvmsg --enable-bcmath --with-bz2 --enable-ctype --with-cdb --with-iconv --enable-exif --enable-ftp --with-gettext --with-pic

phpbrew install 5.4 +default +openssl=shared -- --with-openssl-dir=/usr/include/openssl --with-mysql-sock=/var/run/mysqld/mysqld.sock --with-mysql=mysqlnd --with-mysqli=mysqlnd --enable-pdo --with-pdo-mysql=mysqlnd --enable-exif --with-jpeg-dir=/usr --with-png-dir=/usr --with-freetype-dir=/usr --with-t1lib --with-zlib-dir=/usr --with-mcrypt=/usr --with-mhash --with-xsl=/usr --enable-zip --enable-cgi --with-curl --with-gd --enable-pcntl --enable-mbregex --enable-gd-native-ttf --with-libdir=lib64 --enable-dba=shared --enable-intl --with-readline=/usr --enable-simplexml \--enable-soap --enable-zip --with-mhash=yes --enable-shmop --enable-sockets --enable-wddx --enable-calendar --enable-sysvsem --enable-sysvshm --enable-sysvmsg --enable-bcmath --with-bz2 --enable-ctype --with-cdb --with-iconv --enable-exif --enable-ftp --with-gettext --with-pic

phpbrew install 7.0 +default +openssl=shared -- --with-openssl-dir=/usr/include/openssl --with-mysql-sock=/var/run/mysqld/mysqld.sock --with-mysql=mysqlnd --with-mysqli=mysqlnd --enable-pdo --with-pdo-mysql=mysqlnd --enable-exif --with-jpeg-dir=/usr --with-png-dir=/usr --with-freetype-dir=/usr --with-t1lib --with-zlib-dir=/usr --with-mcrypt=/usr --with-mhash --with-xsl=/usr --enable-zip --enable-cgi --with-curl --with-gd --enable-pcntl --enable-mbregex --enable-gd-native-ttf --with-libdir=lib64 --enable-dba=shared --enable-intl --with-readline=/usr --enable-simplexml \--enable-soap --enable-zip --with-mhash=yes --enable-shmop --enable-sockets --enable-wddx --enable-calendar --enable-sysvsem --enable-sysvshm --enable-sysvmsg --enable-bcmath --with-bz2 --enable-ctype --with-cdb --with-iconv --enable-exif --enable-ftp --with-gettext --with-pic

## Symlinks erstellen
ln -s /usr/local/php/php/php-5.3.29 /usr/local/php/php53
ln -s /usr/local/php/php/php-5.4.45 /usr/local/php/php54
ln -s /usr/local/php/php/php-7.0.11 /usr/local/php/php70

## Apache Module aktivieren
a2enmod actions cgi
service apache2 restart

## Templates erstellen