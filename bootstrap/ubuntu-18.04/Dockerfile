# build image
FROM ubuntu:18.04

LABEL description="Bootstrap image to use for dev with Ubuntu 18.04" \
      maintainer="MoniTauro < twitter @m0n1t4ur0 >" \
      vendor=Monitauro \
      name="monitauro.devbase.bootstrap.ubuntu-18.04"

RUN echo "monitauro.devbase.bootstrap.ubuntu-18.04" > /image.txt

# TODO: Mudar TIMEZONE=UTC
ENV TIMEZONE=America/Sao_Paulo \
    LANGUAGE=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8 \
    DEBIAN_FRONTEND=noninteractive \
    TERM=xterm

# bootstrap
RUN apt clean -y \
    && apt update -y \
    && apt upgrade -y \
    && apt install -y locales \
    && export LANGUAGE=${LANGUAGE} \
    && export LANG=${LANG} \
    && export LC_ALL=${LC_ALL} \
    && locale-gen ${LANG} \
    && dpkg-reconfigure --frontend noninteractive locales \
    && apt install --no-install-recommends -yq \
        apt-transport-https \
        apt-utils \
        ca-certificates \
        lsb-release \
        software-properties-common \
        tzdata \
    && echo "#!/bin/sh\nexit 0" > /usr/sbin/policy-rc.d \
    && ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
    && echo "${TIMEZONE}" | tee /etc/timezone \
    && dpkg-reconfigure --frontend noninteractive tzdata \
    && apt-add-repository -y multiverse \
    && apt update -y \
    && apt upgrade -y 

# cleanup
RUN apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /tmp/* /var/tmp/* \
    && find /var/cache/apt/archives /var/lib/apt/lists \
       -not -name lock \
       -type f \
       -delete \
    && find /var/cache -type f -delete \
    && find /var/log -type f | while read f; do echo -n '' > ${f}; done