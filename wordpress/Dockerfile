# This file creates a container that runs wordpress
#
# Author: Paul Czarkowski
# Date: 12/28/2014

FROM ubuntu:trusty
MAINTAINER Paul Czarkowski "paul@paulcz.net"

RUN \
  DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y wget && \
  wget -O - http://dl.hhvm.com/conf/hhvm.gpg.key | apt-key add - && \
  echo deb http://dl.hhvm.com/ubuntu trusty main | tee /etc/apt/sources.list.d/hhvm.list && \
  apt-get update && apt-get install -yq \
  curl \
  ca-certificates \
  net-tools \
  sudo \
  wget \
  vim \
  strace \
  lsof \
  netcat \
  lsb-release \
  locales \
  socat \
  supervisor \
  hhvm-fastcgi \
  nginx \
  --no-install-recommends && \
  locale-gen en_US.UTF-8

# install etcdctl and confd
RUN \
  curl -sSL -o /usr/local/bin/etcdctl https://s3-us-west-2.amazonaws.com/opdemand/etcdctl-v0.4.6 \
  && chmod +x /usr/local/bin/etcdctl \
  && curl -sSL -o /usr/local/bin/confd https://github.com/kelseyhightower/confd/releases/download/v0.7.1/confd-0.7.1-linux-amd64 \
  && chmod +x /usr/local/bin/confd

ADD . /app

# Define working directory.
WORKDIR /app

RUN \
  useradd -d /app -c 'application' -s '/bin/false' app && \
  chmod +x /app/bin/* && \
  chown -R app:app /app

# Define default command.
CMD ["/app/bin/boot"]

VOLUME /app/wordpress

# Expose ports.
EXPOSE 8080
