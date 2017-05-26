FROM centos:6.9
MAINTAINER antic <antic.zhu@gmail.com>

ENV TZ "Asia/Shanghai"
# 安装依赖
RUN rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-* && \
yum -y install epel-release && \
rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-6 && \
yum makecache && \
yum -y install  wget openssl openssl-devel pcre-devel gcc gcc-c++ && \
yum clean all && rm -rf /tmp/yum*
# nginx
RUN groupadd www && useradd -rg www www
RUN wget http://nginx.org/download/nginx-1.11.9.tar.gz && \
tar zxvf nginx-1.11.9.tar.gz && cd nginx-1.11.9 && \
mkdir -p /usr/local/nginx && \
./configure \
--user=www \
--group=www \
--prefix=/usr/local/nginx \
--with-http_realip_module \
--with-http_sub_module \
--with-http_gzip_static_module \
--with-http_stub_status_module  \
--with-pcre  \
--with-http_ssl_module && \
make -j $(awk '/processor/{i++}END{print i}' /proc/cpuinfo) && \
make install && cd / && \
rm -rf nginx-1.11.9* && cd /usr/local/sbin && ln -s /usr/local/nginx/sbin/nginx

ADD nginx/config/nginx.conf /usr/local/nginx/conf/nginx.conf

EXPOSE 80 443
#启动nginx
ENTRYPOINT ["nginx", "-g", "daemon off;"]
