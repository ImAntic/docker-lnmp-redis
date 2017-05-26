FROM centos:6.9
MAINTAINER antic <antic.zhu@gmail.com>

ENV TZ "Asia/Shanghai"
# 安装依赖
RUN rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-* && \
yum -y install epel-release && \
rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-6 && \
yum makecache && \
yum -y install  wget gcc  libxml2-devel openssl-devel gd-devel libmcrypt-devel libcurl-devel bzip2-devel gmp-devel && \
yum clean all && rm -rf /tmp/yum*
# php
RUN groupadd www && useradd -rg www www
RUN  wget http://cn2.php.net/distributions/php-7.1.3.tar.gz && \
tar zxvf php-7.1.3.tar.gz && cd php-7.1.3 && \
./configure \
--prefix=/etc/php \
--with-config-file-path=/etc/php.ini \
--enable-fpm \
--with-fpm-user=www  \
--with-fpm-group=www \
--enable-inline-optimization \
--disable-debug \
--disable-rpath \
--enable-shared  \
--enable-soap \
--with-libxml-dir \
--with-xmlrpc \
--with-openssl \
--with-mcrypt \
--with-mhash \
--with-pcre-regex \
--with-sqlite3 \
--with-zlib \
--enable-bcmath \
--with-iconv \
--with-bz2 \
--enable-calendar \
--with-curl \
--with-cdb \
--enable-dom \
--enable-exif \
--enable-fileinfo \
--enable-filter \
--with-pcre-dir \
--enable-ftp \
--with-gd \
--with-openssl-dir \
--with-jpeg-dir \
--with-png-dir \
--with-zlib-dir  \
--with-freetype-dir \
--enable-gd-native-ttf \
--enable-gd-jis-conv \
--with-gettext \
--with-gmp \
--with-mhash \
--enable-json \
--enable-mbstring \
--enable-mbregex \
--enable-mbregex-backtrack \
--with-libmbfl \
--with-onig \
--enable-pdo \
--with-mysqli=mysqlnd \
--with-pdo-mysql=mysqlnd \
--with-zlib-dir \
--with-pdo-sqlite \
--enable-session \
--enable-shmop \
--enable-simplexml \
--enable-sockets  \
--enable-sysvmsg \
--enable-sysvsem \
--enable-sysvshm \
--enable-wddx \
--with-libxml-dir \
--enable-zip \
--enable-mysqlnd-compression-support \
--with-pear \
--enable-opcache && \
make -j $(awk '/processor/{i++}END{print i}' /proc/cpuinfo) && make install
RUN cd php-7.1.3 && cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm && chmod +x /etc/init.d/php-fpm && \
cp /etc/php/etc/php-fpm.conf.default /etc/php/etc/php-fpm.conf && \
cp /etc/php/etc/php-fpm.d/www.conf.default /etc/php/etc/php-fpm.d/www.conf && \
cp php.ini-production /etc/php.ini && \
sed -i -e 's~^;date.timezone =$~date.timezone = PRC~g' /etc/php.ini && \
sed -i -e 's~^;error_log = syslog$~error_log = /var/www/html/php/error.log~g' /etc/php.ini && \
sed -i -e 's/listen = 127.0.0.1:9000/listen = 9000/' /etc/php/etc/php-fpm.d/www.conf
RUN rm -rf php*


EXPOSE 9000

#启动 php-fpm
ENTRYPOINT ["/etc/php/sbin/php-fpm","-F", "-c", "/etc/php.ini"]
