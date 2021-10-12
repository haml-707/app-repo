FROM openeuler/openeuler:20.03
MAINTAINER tommylikehu@gmail.com

# install rsync and tini
RUN yum -y update && yum install -y rsync tzdata
COPY tini /usr/local/bin
ENV TZ="Asia/Shanghai"
COPY entrypoint.sh /usr/local/bin/
ENTRYPOINT ["tini", "--"]
CMD ["entrypoint.sh"]
