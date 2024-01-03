FROM openeuler/openeuler:22.03-lts-sp1
MAINTAINER 747802914@qq.com

COPY tini /usr/local/bin
RUN yum -y update && yum install -y rsync tzdata && chmod +x /usr/local/bin/tini && \
    useradd -s /sbin/nologin -u 1000 rsync

ENV TZ="Asia/Shanghai"
COPY entrypoint.sh /usr/local/bin/
RUN chown 1000:1000 /usr/local/bin/entrypoint.sh && \
    chown 1000:1000 /usr/local/bin/tini && \
    chmod +x /usr/local/bin/entrypoint.sh

USER rsync

WORKDIR /usr/local/bin/
ENV PATH=$PATH:/usr/local/bin/
CMD ["entrypoint.sh"]
