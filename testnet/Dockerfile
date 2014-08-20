FROM ubuntu:14.04
MAINTAINER Lars Kluge <l@larskluge.com>

RUN apt-get update
RUN dpkg-reconfigure locales && \
    locale-gen en_US.UTF-8 && \
    /usr/sbin/update-locale LANG=en_US.UTF-8
ENV LC_ALL en_US.UTF-8

RUN apt-get -y install vim wget git build-essential pkg-config libtool autotools-dev autoconf libssl-dev libboost-all-dev libdb5.1-dev libdb5.1++-dev bsdmainutils

RUN adduser --disabled-password --home /dogecoin --gecos "" dogecoin

USER dogecoin
ENV HOME /dogecoin
WORKDIR /dogecoin
RUN mkdir data
RUN mkdir src
WORKDIR /dogecoin/src
RUN git clone https://github.com/dogecoin/dogecoin.git
WORKDIR /dogecoin/src/dogecoin
RUN git checkout 1.8-dev
RUN ./autogen.sh && ./configure --without-miniupnpc && make

ADD dogecoin.conf /dogecoin/.dogecoin/dogecoin.conf

USER root
RUN make install
RUN chown -R dogecoin:dogecoin /dogecoin/.dogecoin

USER dogecoin
WORKDIR /dogecoin

VOLUME /dogecoin/data

EXPOSE 44555 44556

ENV RPCUSER user
ENV RPCPASS pass

CMD ["dogecoind"]

