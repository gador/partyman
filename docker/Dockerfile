FROM ubuntu:24.04

ARG CONTAINER_TIMEZONE=UTC
ENV PARTICL_DIR=/root/particlcore
ENV PARTICL_DATADIR=/root/.particl
ENV PARTYMAN_DIR=/root/partyman

ENV PATH=${PARTYMAN_DIR}/bin:${PARTICL_DIR}:$PATH
ENV DEBIAN_FRONTEND=noninteractive
ENV TERM=xterm

RUN apt-get update -y \
    && apt-get install -y software-properties-common apt-utils wget curl net-tools iputils-ping ntp ntpdate python3 git \
       unzip pv jq dnsutils netcat-openbsd locales cron php8.3 nano \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN echo "export TERM=${TERM}" >> ~/.bashrc

RUN locale-gen en_US.UTF-8
RUN dpkg-reconfigure locales

RUN echo ${CONTAINER_TIMEZONE} >/etc/timezone && \
    ln -sf /usr/share/zoneinfo/${CONTAINER_TIMEZONE} /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata && \
    echo "Container timezone set to: ${CONTAINER_TIMEZONE}"
RUN ntpdate -q ntp.ubuntu.com

WORKDIR /root
RUN git clone https://github.com/dasource/partyman.git

WORKDIR /root/partyman
COPY bin/entrypoint.sh bin/entrypoint.sh
COPY bin/cron.sh bin/cron.sh
COPY conf/crontab crontab

RUN ./partyman install unattended

