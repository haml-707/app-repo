FROM swr.cn-north-4.myhuaweicloud.com/opensourceway/openeuler/nginx:1.24.0-22.03-lts-sp1-fancy as NginxBuilder

FROM openeuler/openeuler:22.03-lts-sp1

ENV PATH /usr/share/nginx/sbin:$PATH
ENV NGINX_CONFIG_FILE /etc/nginx/nginx.conf
ENV NGINX_CONFIG_PATH /etc/nginx/
ENV NGINX_PID /var/run/nginx.pid
ENV NGINX_USER nginx
ENV NGINX_GROUP nginx
ENV NGINX_BIN /usr/share/nginx/sbin/
ENV NGINX_HOME /usr/share/nginx/
ENV NGINX_EXE_FILE /usr/share/nginx/sbin/nginx
ENV DST_PATH /etc/nginx/cert
COPY --from=NginxBuilder /usr/share/nginx /usr/share/nginx
COPY --from=NginxBuilder /usr/share/nginx/sbin/nginx /usr/share/nginx/sbin/nginx
COPY --from=NginxBuilder /etc/nginx/modules /etc/nginx/modules
COPY --from=NginxBuilder /etc/nginx/geoip  /etc/nginx/geoip
COPY --from=NginxBuilder /etc/nginx/mime.types  /etc/nginx/mime.types
ADD ./fancySource/openEuler /etc/nginx/fancySource

RUN yum update -y \
    && yum install -y findutils passwd shadow
COPY ./nginx/nginx.conf /etc/nginx/nginx.conf

RUN touch /var/run/nginx.pid \
    && groupadd -g 1000 nginx \
    && useradd -u 1000 -g nginx -s /sbin/nologin nginx \
    && chown -R nginx:nginx /usr/share/nginx \
    && find /usr/share/nginx -type d -print0 | xargs -0 chmod 500 \
    && chmod 500 /usr/share/nginx/sbin/nginx \
    && mkdir -p /var/log/nginx \
    && chown -R nginx:nginx /var/log/nginx \
    && chmod -R 640 /var/log/nginx \
    && mkdir -p /var/lib/nginx/tmp/client_body \
    && chown -R nginx:nginx /var/lib/nginx/tmp/client_body \
    && mkdir -p /var/lib/nginx/tmp/fastcgi \
    && chown -R nginx:nginx /var/lib/nginx/tmp/fastcgi \
    && mkdir -p /var/lib/nginx/tmp/proxy \
    && chown -R nginx:nginx /var/lib/nginx/tmp/proxy \
    && mkdir -p /var/lib/nginx/tmp/scgi \
    && chown -R nginx:nginx /var/lib/nginx/tmp/scgi \
    && mkdir -p /var/lib/nginx/tmp/uwsgi \
    && chown -R nginx:nginx /var/lib/nginx/tmp/uwsgi \
    && chmod -R 500 /var/lib/nginx/ \
    && chown -R nginx:nginx /var/lib/nginx/ \
    && chown -R nginx:nginx /var/run/nginx.pid \
    && chmod 640 /var/run/nginx.pid \
    && chown -R nginx:nginx /etc/nginx \
    && chmod 550 /etc/nginx \
    && chmod 550 /etc/nginx/geoip/ \
    && chmod 440 /etc/nginx/geoip/* \
    && chmod 550 /etc/nginx/modules \
    && chmod 440 /etc/nginx/modules/* \
    && chmod 550 /etc/nginx/fancySource \
    && chmod 550 /etc/nginx/fancySource/css \
    && chmod 440 /etc/nginx/fancySource/css/* \
    && chmod 550 /etc/nginx/fancySource/img \
    && chmod 440 /etc/nginx/fancySource/img/* \
    && chmod 550 /etc/nginx/fancySource/img/footer \
    && chmod 440 /etc/nginx/fancySource/img/footer/* \
    && chmod 440 /etc/nginx/fancySource/footer.html \
    && chmod 440 /etc/nginx/fancySource/header.html \
    && chmod 440 /etc/nginx/nginx.conf \
    && chmod 440 /etc/nginx/mime.types \
    && rm -rf /usr/share/nginx/html/ \
    && rm -rf /usr/share/nginx/logs/ \
    && echo "umask 0027" >> /etc/bashrc \
    && echo "set +o history" >> /etc/bashrc \
    && sed -i "s|HISTSIZE=1000|HISTSIZE=0|" /etc/profile \
    && sed -i "s|PASS_MAX_DAYS[ \t]*99999|PASS_MAX_DAYS 30|" /etc/login.defs \
    && passwd -l $NGINX_USER \
    && usermod -s /sbin/nologin sync \
    && usermod -s /sbin/nologin shutdown \
    && usermod -s /sbin/nologin halt \
    && echo "export TMOUT=1800 readonly TMOUT" >> /etc/profile \
    && rm -rf /usr/bin/gdb* \
    && rm -rf /usr/share/gdb \
    && rm -rf /usr/share/gcc-10.3.1

RUN yum remove gdb-gdbserver findutils passwd shadow -y \
    && yum clean all

EXPOSE 8080

USER nginx

ENTRYPOINT ["nginx", "-g", "daemon off;"]