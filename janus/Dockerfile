############################################################
# Dockerfile - Janus Gateway on Debian Jessie
# https://github.com/krull/docker-janus
############################################################

FROM debian:stretch-slim

LABEL maintainer="Miguel Tavares <stryng@gmail.com>"
LABEL description="Deploys an image with Janus Gateway"

WORKDIR /root

RUN apt update

RUN apt install -y git make

RUN apt install -y \
      libmicrohttpd-dev libjansson-dev \
      libssl-dev libsofia-sip-ua-dev libglib2.0-dev \
      libopus-dev libogg-dev libcurl4-openssl-dev liblua5.3-dev \
      libconfig-dev pkg-config gengetopt libtool automake \
      libnice-dev libsrtp2-dev libwebsockets-dev

RUN git clone https://github.com/sctplab/usrsctp \
&& cd usrsctp \
&& ./bootstrap \
&& ./configure \
&& make \
&& make install

RUN git clone https://github.com/meetecho/janus-gateway.git \
&& cd janus-gateway \
&& sh autogen.sh \
&& ./configure --prefix=/opt/janus --disable-rabbitmq --disable-mqtt \
&& make \
&& make install \
&& make configs

ENV LD_LIBRARY_PATH /usr/local/lib

RUN apt install -y coturn \
&& turnadmin -a -u stryng -p SystemR00t -r usa \
&& turnserver -o

CMD ["/opt/janus/bin/janus"]

