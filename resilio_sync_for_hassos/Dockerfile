ARG BUILD_FROM
FROM $BUILD_FROM

# set version label
ARG BUILD_DATE
ARG VERSION
ARG SYNC_VERSION
LABEL build_version="Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="thelamer"


RUN \
  echo "**** install packages ****" && \
  apt-get update && \
  apt-get install -y --no-install-recommends \
    gnupg && \
  echo "**** install resilio-sync ****" && \
  if [ -z ${SYNC_VERSION+x} ]; then \
    SYNC_VERSION=$(curl -sX GET https://linux-packages.resilio.com/resilio-sync/deb/dists/resilio-sync/non-free/binary-amd64/Packages |grep -A 7 -m 1 'Package: resilio-sync' | awk -F ': ' '/Version/{print $2;exit}'); \
  fi && \
  echo "deb https://linux-packages.resilio.com/resilio-sync/deb resilio-sync non-free" | tee /etc/apt/sources.list.d/resilio-sync.list && \
  curl -L https://linux-packages.resilio.com/resilio-sync/key.asc | apt-key add && \
  apt-get update && \
  apt-get install -y --no-install-recommends \
    "resilio-sync=${SYNC_VERSION}" && \
  echo "**** cleanup ****" && \
  rm -rf \
    /tmp/* \
    /var/lib/apt/lists/* \
    /var/tmp/* \
    /var/log/*

# add local files
COPY rootfs/ / 

# ports 
EXPOSE 8888 55555
