FROM codementality/centos7-core

ENV SUMMARY="Base image with essential libraries and tools used as a base for \
builder images like perl, python, ruby, etc." \
    NODEJS_SCL=rh-nodejs10

LABEL summary="$SUMMARY" \
      name="centos/centos7-base" \
      version="1" \
      maintainer="Lisa Ridley <lisa@codementality>"

# This is the list of basic dependencies that all derivative container images can
# consume.
RUN yum install -y centos-release-scl && \
  INSTALL_PKGS="autoconf \
  automake \
  bzip2 \
  gcc-c++ \
  gd-devel \
  gdb \
  git \
  libcurl-devel \
  libxml2-devel \
  libxslt-devel \
  lsof \
  make \
  mariadb-devel \
  mariadb-libs \
  openssl-devel \
  patch \
  postgresql-devel \
  procps-ng \
  ${NODEJS_SCL}-npm \
  sqlite-devel \
  unzip \
  wget \
  which \
  zlib-devel" && \
  yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && \
  rpm -V $INSTALL_PKGS && \
  yum -y clean all --enablerepo='*'
