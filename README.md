Fork of https://git.scit.ch/rs/VestaCP-MultiPHP - Supports Ubuntu 18.04

# VestaCP - MultiPHP
This a little rewrite of the idea from Peter Anikin (http://anikin.pw/all/multiversionnost-php-n-aservere-s-vestacp/).
## Install Dependencies
```bash
Ubuntu 18.04
sudo apt update
sudo apt-get install wget build-essential libssl-dev libxml2-dev pkg-config libssl-dev  libbz2-dev libcurl4-openssl-dev libmysqlclient-dev libgdbm-dev libjpeg62 libjpeg62-dev libxml2 libxml2-dev libmcrypt4 libmcrypt-dev libmhash2 libmhash-dev libmm-dev libmm14 libtidy-dev libxslt1-dev libxslt1.1 libfreetype6 libfreetype6-dev libicu-dev libreadline-dev autoconf

Ubuntu Older
apt-get install build-essential libssl-dev libxml2-dev pkg-config libssl-dev libsslcommon2-dev libbz2-dev libcurl4-openssl-dev libmysqlclient-dev libgdbm-dev libjpeg62 libjpeg62-dev libpng12-0 libpng12-dev libxml2 libxml2-dev libmcrypt4 libmcrypt-dev libmhash2 libmhash-dev libmm-dev libmm14 libtidy-dev libtidy-0.99-0 libxslt1-dev libxslt1.1 libfreetype6 libfreetype6-dev libicu-dev libreadline-dev

# Small Modification for PHP 5.3/5.4 Compilation (http://stackoverflow.com/a/26342869)
mkdir /usr/include/freetype2/freetype
ln -s /usr/include/freetype2/freetype.h /usr/include/freetype2/freetype/freetype.h
```


## Install and Config of PHPBrew
```bash
curl -L -O https://github.com/phpbrew/phpbrew/raw/master/phpbrew
chmod +x phpbrew
sudo mv phpbrew /usr/bin/phpbrew

mkdir -p /usr/local/php
phpbrew init --root=/usr/local/php
export PHPBREW_ROOT=/usr/local/php
[[ -e ~/.phpbrew/bashrc ]] && source ~/.phpbrew/bashrc

phpbrew update
phpbrew update --old
```
## Ubuntu 16.04 / PHP 5.3 only!
For Ubuntu 16.04 you need to install an older OpenSSL Libary, if not it will throw out an error during compilation.

```bash
cd /usr/src
wget https://www.openssl.org/source/openssl-0.9.8zb.tar.gz
tar xfvz openssl-0.9.8zb.tar.gz
cd openssl-0.9.8zb
mkdir /usr/local/sslold
./config --prefix=/usr/local --openssldir=/usr/local/sslold
make
make install
```
Then change --with-openssl-dir=/usr/include/openssl to --with-openssl-dir=/usr/local/sslold and compile.

## Ubuntu 18.04 & PHP 5.x only!

```bash
# fix configure: error: Please reinstall the libcurl distribution - easy.h should be in <curl-dir>/include/curl/
# because of this bug https://github.com/phpbrew/phpbrew/issues/861
# we need to do this
sudo ln -s /usr/include/x86_64-linux-gnu/curl /usr/include/curl

cd /usr/src
wget https://www.openssl.org/source/openssl-1.0.2o.tar.gz
tar -xzvf openssl-1.0.2o.tar.gz
pushd openssl-1.0.2o
./config -fPIC shared --prefix=/usr/local --openssldir=/usr/local/openssl
make
make test
sudo make install
popd
```
Then change --with-openssl-dir=/usr/include/openssl to --with-openssl-dir=/usr/local/openssl and compile.

## Compiling PHP Packages with needed Modules
You don't need to install all Versions, just choose that Version you want to have installed.
```bash
# PHP 5.3
phpbrew install 5.3 +default +openssl=shared -- --with-openssl-dir=/usr/include/openssl --with-mysql-sock=/var/run/mysqld/mysqld.sock --with-mysql=mysqlnd --with-mysqli=mysqlnd --enable-pdo --with-pdo-mysql=mysqlnd --enable-exif --with-jpeg-dir=/usr --with-png-dir=/usr --with-freetype-dir=/usr --with-zlib-dir=/usr --with-mcrypt=/usr --with-mhash --with-xsl=/usr --enable-zip --enable-cgi --with-curl --with-gd --enable-pcntl --enable-mbregex --enable-gd-native-ttf --with-libdir=lib64 --enable-dba=shared --enable-intl --with-readline=/usr --enable-simplexml \--enable-soap --enable-zip --with-mhash=yes --enable-shmop --enable-sockets --enable-wddx --enable-calendar --enable-sysvsem --enable-sysvshm --enable-sysvmsg --enable-bcmath --with-bz2 --enable-ctype --with-cdb --with-iconv --enable-exif --enable-ftp --with-gettext --with-pic

# PHP 5.4
phpbrew install 5.4 +default +openssl=shared -- --with-openssl-dir=/usr/include/openssl --with-mysql-sock=/var/run/mysqld/mysqld.sock --with-mysql=mysqlnd --with-mysqli=mysqlnd --enable-pdo --with-pdo-mysql=mysqlnd --enable-exif --with-jpeg-dir=/usr --with-png-dir=/usr --with-freetype-dir=/usr --with-zlib-dir=/usr --with-mcrypt=/usr --with-mhash --with-xsl=/usr --enable-zip --enable-cgi --with-curl --with-gd --enable-pcntl --enable-mbregex --enable-gd-native-ttf --with-libdir=lib64 --enable-dba=shared --enable-intl --with-readline=/usr --enable-simplexml \--enable-soap --enable-zip --with-mhash=yes --enable-shmop --enable-sockets --enable-wddx --enable-calendar --enable-sysvsem --enable-sysvshm --enable-sysvmsg --enable-bcmath --with-bz2 --enable-ctype --with-cdb --with-iconv --enable-exif --enable-ftp --with-gettext --with-pic

# PHP 5.5
phpbrew install 5.5 +default +openssl=shared -- --with-openssl-dir=/usr/include/openssl --with-mysql-sock=/var/run/mysqld/mysqld.sock --with-mysql=mysqlnd --with-mysqli=mysqlnd --enable-pdo --with-pdo-mysql=mysqlnd --enable-exif --with-jpeg-dir=/usr --with-png-dir=/usr --with-freetype-dir=/usr --with-zlib-dir=/usr --with-mcrypt=/usr --with-mhash --with-xsl=/usr --enable-zip --enable-cgi --with-curl --with-gd --enable-pcntl --enable-mbregex --enable-gd-native-ttf --with-libdir=lib64 --enable-dba=shared --enable-intl --with-readline=/usr --enable-simplexml \--enable-soap --enable-zip --with-mhash=yes --enable-shmop --enable-sockets --enable-wddx --enable-calendar --enable-sysvsem --enable-sysvshm --enable-sysvmsg --enable-bcmath --with-bz2 --enable-ctype --with-cdb --with-iconv --enable-exif --enable-ftp --with-gettext --with-pic

# PHP 5.6
phpbrew install 5.6 +default +openssl=shared -- --with-openssl-dir=/usr/include/openssl --with-mysql-sock=/var/run/mysqld/mysqld.sock --with-mysql=mysqlnd --with-mysqli=mysqlnd --enable-pdo --with-pdo-mysql=mysqlnd --enable-exif --with-jpeg-dir=/usr --with-png-dir=/usr --with-freetype-dir=/usr --with-zlib-dir=/usr --with-mcrypt=/usr --with-mhash --with-xsl=/usr --enable-zip --enable-cgi --with-curl --with-gd --enable-pcntl --enable-mbregex --enable-gd-native-ttf --with-libdir=lib64 --enable-dba=shared --enable-intl --with-readline=/usr --enable-simplexml \--enable-soap --enable-zip --with-mhash=yes --enable-shmop --enable-sockets --enable-wddx --enable-calendar --enable-sysvsem --enable-sysvshm --enable-sysvmsg --enable-bcmath --with-bz2 --enable-ctype --with-cdb --with-iconv --enable-exif --enable-ftp --with-gettext --with-pic

# PHP 7.0
phpbrew install 7.0 +default +openssl=shared -- --with-openssl-dir=/usr/include/openssl --with-mysql-sock=/var/run/mysqld/mysqld.sock --with-mysql=mysqlnd --with-mysqli=mysqlnd --enable-pdo --with-pdo-mysql=mysqlnd --enable-exif --with-jpeg-dir=/usr --with-png-dir=/usr --with-freetype-dir=/usr --with-zlib-dir=/usr --with-mcrypt=/usr --with-mhash --with-xsl=/usr --enable-zip --enable-cgi --with-curl --with-gd --enable-pcntl --enable-mbregex --enable-gd-native-ttf --with-libdir=lib64 --enable-dba=shared --enable-intl --with-readline=/usr --enable-simplexml \--enable-soap --enable-zip --with-mhash=yes --enable-shmop --enable-sockets --enable-wddx --enable-calendar --enable-sysvsem --enable-sysvshm --enable-sysvmsg --enable-bcmath --with-bz2 --enable-ctype --with-cdb --with-iconv --enable-exif --enable-ftp --with-gettext --with-pic

# PHP 7.1
phpbrew install 7.1 +default +openssl=shared -- --with-openssl-dir=/usr/include/openssl --with-mysql-sock=/var/run/mysqld/mysqld.sock --with-mysql=mysqlnd --with-mysqli=mysqlnd --enable-pdo --with-pdo-mysql=mysqlnd --enable-exif --with-jpeg-dir=/usr --with-png-dir=/usr --with-freetype-dir=/usr --with-zlib-dir=/usr --with-mcrypt=/usr --with-mhash --with-xsl=/usr --enable-zip --enable-cgi --with-curl --with-gd --enable-pcntl --enable-mbregex --enable-gd-native-ttf --with-libdir=lib64 --enable-dba=shared --enable-intl --with-readline=/usr --enable-simplexml \--enable-soap --enable-zip --with-mhash=yes --enable-shmop --enable-sockets --enable-wddx --enable-calendar --enable-sysvsem --enable-sysvshm --enable-sysvmsg --enable-bcmath --with-bz2 --enable-ctype --with-cdb --with-iconv --enable-exif --enable-ftp --with-gettext --with-pic
```



## Create Symlinks
Symlink only needed for installed PHP Versions, PHP Version (php-x.x.xx) may be different.
```bash
# PHP 5.3
ln -s /usr/local/php/php/php-5.3.29 /usr/local/php/php53

# PHP 5.4
ln -s /usr/local/php/php/php-5.4.45 /usr/local/php/php54

# PHP 5.5
ln -s /usr/local/php/php/php-5.5.38 /usr/local/php/php55

# PHP 5.6
ln -s /usr/local/php/php/php-5.6.28 /usr/local/php/php56

# PHP 7.0
ln -s /usr/local/php/php/php-7.0.14 /usr/local/php/php70

# PHP 7.1
ln -s /usr/local/php/php/php-7.1.6 /usr/local/php/php71
```

## Enable needed Apache Modules
```bash
a2enmod actions cgi
service apache2 restart
```

## Create VestaCP Templates
You can do this also manualy, just copy the phpcgi.stpl, phpcgi.tpl and phpcgi.sh files in your VestaCP Template folder and modify the yourname.sh file to your needed configuration. Please do only download the Tempaltes for installed PHP Versions.
```bash
# PHP 5.3
wget https://github.com/icecold21/vesta-multi-php/raw/master/php53.sh -O /usr/local/vesta/data/templates/web/apache2/php53.sh
wget https://github.com/icecold21/vesta-multi-php/raw/master/php.tpl -O /usr/local/vesta/data/templates/web/apache2/php53.tpl
wget https://github.com/icecold21/vesta-multi-php/raw/master/php.stpl -O /usr/local/vesta/data/templates/web/apache2/php53.stpl

# PHP 5.4
wget https://github.com/icecold21/vesta-multi-php/raw/master/php54.sh -O /usr/local/vesta/data/templates/web/apache2/php54.sh
wget https://github.com/icecold21/vesta-multi-php/raw/master/php.tpl -O /usr/local/vesta/data/templates/web/apache2/php54.tpl
wget https://github.com/icecold21/vesta-multi-php/raw/master/php.stpl -O /usr/local/vesta/data/templates/web/apache2/php54.stpl

# PHP 5.5
wget https://github.com/icecold21/vesta-multi-php/raw/master/php55.sh -O /usr/local/vesta/data/templates/web/apache2/php55.sh
wget https://github.com/icecold21/vesta-multi-php/raw/master/php.tpl -O /usr/local/vesta/data/templates/web/apache2/php55.tpl
wget https://github.com/icecold21/vesta-multi-php/raw/master/php.stpl -O /usr/local/vesta/data/templates/web/apache2/php55.stpl

# PHP 5.6
wget https://github.com/icecold21/vesta-multi-php/raw/master/php56.sh -O /usr/local/vesta/data/templates/web/apache2/php56.sh
wget https://github.com/icecold21/vesta-multi-php/raw/master/php.tpl -O /usr/local/vesta/data/templates/web/apache2/php56.tpl
wget https://github.com/icecold21/vesta-multi-php/raw/master/php.stpl -O /usr/local/vesta/data/templates/web/apache2/php56.stpl

# PHP 7.0
wget https://github.com/icecold21/vesta-multi-php/raw/master/php70.sh -O /usr/local/vesta/data/templates/web/apache2/php70.sh
wget https://github.com/icecold21/vesta-multi-php/raw/master/php.tpl -O /usr/local/vesta/data/templates/web/apache2/php70.tpl
wget https://github.com/icecold21/vesta-multi-php/raw/master/php.stpl -O /usr/local/vesta/data/templates/web/apache2/php70.stpl

# PHP 7.0
wget https://github.com/icecold21/vesta-multi-php/raw/master/php71.sh -O /usr/local/vesta/data/templates/web/apache2/php71.sh
wget https://github.com/icecold21/vesta-multi-php/raw/master/php.tpl -O /usr/local/vesta/data/templates/web/apache2/php71.tpl
wget https://github.com/icecold21/vesta-multi-php/raw/master/php.stpl -O /usr/local/vesta/data/templates/web/apache2/php71.stpl

# Update Owner and Permissions
chmod 755 /usr/local/vesta/data/templates/web/apache2/*
```

## Use the MultiPHP Modification
```bash
-> go to WEB -> your Domain -> Apache2 -> and chose the PHP Version (PHP53, PHP54 or PHP70) -> Save

# That it creates the needed cgi-bin file, you have to run a full rebuild of the user (from Web or CommandLine).
v-user-rebuild username
```


Validating installation
1. Create website
2. Modify website to use proper template. e.g php 5.7
3. Replace index.html with index.php
```php
<?php
  $result = shell_exec('lsb_release -a');
  echo "<pre>$result</pre>";
  phpinfo();
```
4. Visit the site and verify configuration is working
