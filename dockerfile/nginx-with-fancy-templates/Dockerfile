FROM openeuler/openeuler:20.03
MAINTAINER tommylike <tommylikehu@gmail.com>
# Set env
USER root
ENV USER root

RUN alias rm="rm" && \
    yum update && \
    yum install -y wget && \
    yum install -y tar && \
    yum install -y git && \
    yum install -y make automake autoconf libtool gcc gcc-c++ kernel-devel libmaxminddb-devel pcre-devel openssl openssl-devel tzdata && \
    yum install -y createrepo && \
    yum install -y openssh-clients && \
    rm -rf /var/lib/{apt,dpkg,cache,log}/

# prepare nginx
RUN wget http://nginx.org/download/nginx-1.20.0.tar.gz \
    && tar -xvf nginx-1.20.0.tar.gz && rm nginx-1.20.0.tar.gz

# prepare nginx fancyindex
RUN git clone https://github.com/aperezdc/ngx-fancyindex.git ngx-fancyindex

RUN cd nginx-1.20.0 \
    && ./configure --prefix=/etc/nginx/ --add-module=../ngx-fancyindex \
    && make && make install \
    && cd ../ && rm -rf nginx-1.20.0  && rm -rf ngx-fancyindex\
    && mkdir -p /var/log/nginx

# Copy templates into /etc/nginx/conf/static
ADD ./fancySource/openEuler /etc/nginx/conf/static/fancySource
COPY ./nginx/nginx.conf /etc/nginx/conf/nginx.conf
COPY ./nginx/default.conf /etc/nginx/conf/conf.d/default.conf
ENV PATH="/etc/nginx/sbin:${PATH}"
ENTRYPOINT ["nginx", "-g", "daemon off;"]
