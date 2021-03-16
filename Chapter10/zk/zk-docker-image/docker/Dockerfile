FROM alpine-jre8  
ENV ZK_USER=zookeeper \
ZK_DATA_DIR=/var/lib/zookeeper/data \
ZK_DATA_LOG_DIR=/var/lib/zookeeper/log \
ZK_LOG_DIR=/var/log/zookeeper \

ZK_DIST=zookeeper-3.5.9-bin
RUN set -x \
    && wget -q "http://www.apache.org/dist/zookeeper/zookeeper-3.5.9/apache-$ZK_DIST.tar.gz" \
    && export GNUPGHOME="$(mktemp -d)" \
    && tar -xzf "apache-$ZK_DIST.tar.gz" -C /opt \
    && rm -r "$GNUPGHOME" "apache-$ZK_DIST.tar.gz" \
    && ln -s /opt/apache-$ZK_DIST /opt/zookeeper \
    && rm -rf /opt/zookeeper/CHANGES.txt \
    /opt/zookeeper/README.txt \
    /opt/zookeeper/NOTICE.txt \
    /opt/zookeeper/CHANGES.txt \
    /opt/zookeeper/README_packaging.txt \
    /opt/zookeeper/build.xml \
    /opt/zookeeper/config \
    /opt/zookeeper/contrib \
    /opt/zookeeper/dist-maven \
    /opt/zookeeper/docs \
    /opt/zookeeper/ivy.xml \
    /opt/zookeeper/ivysettings.xml \
    /opt/zookeeper/recipes \
    /opt/zookeeper/src \
    /opt/zookeeper/apache-$ZK_DIST.jar.asc \
    /opt/zookeeper/apache-$ZK_DIST.jar.md5 \
    /opt/zookeeper/apache-$ZK_DIST.jar.sha1 \
	&& rm -rf /var/lib/apt/lists/*


RUN apk add --no-cache bash
RUN apk add --no-cache netcat-openbsd

#Copy configuration generator script to bin
COPY scripts /opt/zookeeper/bin/

# Create a user for the zookeeper process and configure file system ownership 
# for nessecary directories and symlink the distribution as a user executable
RUN set -x \
	&& adduser  --disabled-password --gecos "" --no-create-home  --uid 1000 $ZK_USER  \
        && [ `id -u $ZK_USER` -eq 1000 ] \
        && mkdir -p $ZK_DATA_DIR $ZK_DATA_LOG_DIR $ZK_LOG_DIR /usr/share/zookeeper /tmp/zookeeper /usr/etc/ \
        && chown -R "$ZK_USER:$ZK_USER" /opt/apache-$ZK_DIST $ZK_DATA_DIR $ZK_LOG_DIR $ZK_DATA_LOG_DIR /tmp/zookeeper \
        && ln -s /opt/zookeeper/conf/ /usr/etc/zookeeper \
        && ln -s /opt/zookeeper/bin/* /usr/bin \
        && ln -s /opt/zookeeper/apache-$ZK_DIST.jar /usr/share/zookeeper/ \
        && ln -s /opt/zookeeper/lib/* /usr/share/zookeeper
