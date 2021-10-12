FROM openeuler/openeuler:20.03
MAINTAINER tommylikehu@gmail.com

# install rsync and tini
COPY tini /usr/local/bin
RUN yum -y update && yum install -y rsync tzdata && chmod +x /usr/local/bin/tini

ENV TZ="Asia/Shanghai"
COPY entrypoint.sh /usr/local/bin/
ENTRYPOINT ["tini", "--"]
CMD ["entrypoint.sh"]
