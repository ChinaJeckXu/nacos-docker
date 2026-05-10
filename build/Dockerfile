FROM alpine:latest
LABEL maintainer="chinajeckxu <chinajeckxu@163.com>"

# 安装依赖
RUN apk add --no-cache openjdk8-jre-base curl iputils ncurses vim libcurl bash

# 设置环境变量
ENV MODE="cluster" \
    PREFER_HOST_MODE="ip"\
    BASE_DIR="/home/nacos" \
    CLASSPATH=".:/home/nacos/conf:$CLASSPATH" \
    CLUSTER_CONF="/home/nacos/conf/cluster.conf" \
    FUNCTION_MODE="all" \
    JAVA_HOME="/usr/lib/jvm/java-1.8-openjdk" \
    NACOS_USER="nacos" \
    JAVA="/usr/lib/jvm/java-1.8-openjdk/bin/java" \
    JVM_XMS="1g" \
    JVM_XMX="1g" \
    JVM_XMN="512m" \
    JVM_MS="128m" \
    JVM_MMS="320m" \
    NACOS_DEBUG="n" \
    TOMCAT_ACCESSLOG_ENABLED="false" \
    TIME_ZONE="Asia/Shanghai"

ARG NACOS_VERSION=2.5.2_TW
ARG HOT_FIX_FLAG=""

WORKDIR $BASE_DIR

# 复制本地 nacos 安装包并解压（不再从 GitHub 下载）
COPY nacos-server-${NACOS_VERSION}.tar.gz /tmp/nacos-server.tar.gz
RUN set -x \
    && tar -xzvf /tmp/nacos-server.tar.gz -C /home \
    && rm -f /tmp/nacos-server.tar.gz \
    && rm -rf /home/nacos/bin/* /home/nacos/conf/*.properties /home/nacos/conf/*.example /home/nacos/conf/nacos-mysql.sql \
    && ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && echo $TIME_ZONE > /etc/timezone

ADD bin/docker-startup.sh bin/docker-startup.sh
ADD conf/application.properties conf/application.properties

# 设置启动日志目录
RUN mkdir -p logs \
	&& touch logs/start.out \
	&& ln -sf /dev/stdout logs/start.out \
	&& ln -sf /dev/stderr logs/start.out \
    && chmod +x bin/docker-startup.sh

EXPOSE 8848
ENTRYPOINT ["sh","bin/docker-startup.sh"]