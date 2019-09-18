FROM centos:7

ENV SUMMARY="Base CentOS-7 image."

LABEL summary="$SUMMARY" \
      name="codementality/centos7-core" \
      version="1" \
      maintainer="Lisa Ridley <lisa@codementality.com>"

ENV \
    # Path to be used in other layers to place s2i scripts into
    STI_SCRIPTS_PATH=/usr/libexec/s2i \
    APP_ROOT=/opt/app-root \
    # The $HOME is not set by default, but some applications needs this variable
    HOME=/opt/app-root/src \
    PATH=/opt/app-root/src/bin:/opt/app-root/bin:$PATH

# When bash is started non-interactively, to run a shell script, for example it
# looks for this variable and source the content of this file. This will enable
# the SCL for all scripts without need to do 'scl enable'.
ENV BASH_ENV=${APP_ROOT}/etc/scl_enable \
    ENV=${APP_ROOT}/etc/scl_enable \
    PROMPT_COMMAND=". ${APP_ROOT}/etc/scl_enable"

# Copy update repo to image.
COPY CentOS-CR.repo /etc/yum.repos.d/CentOS-CR.repo

# This is the list of basic dependencies that all language container image can
# consume.
# Also setup the 'openshift' user that is used for the build execution and for the
# application runtime execution.
# TODO: Use better UID and GID values
RUN rpmkeys --import file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7 && \
  INSTALL_PKGS="bsdtar \
  findutils \
  gettext \
  groff-base \
  rsync \
  scl-utils \
  tar \
  unzip \
  yum-utils" && \
  mkdir -p ${HOME}/.pki/nssdb && \
  chown -R 1001:0 ${HOME}/.pki && \
  yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && \
  rpm -V $INSTALL_PKGS && \
  yum -y clean all --enablerepo='*'

# Copy extra files to the image.
COPY ./root/ /
# Copy update repo to image.
COPY CentOS-CR.repo /etc/yum.repos.d/CentOS-CR.repo

# Directory with the sources is set as the working directory so all STI scripts
# can execute relative to this path.
WORKDIR ${HOME}

ENTRYPOINT ["container-entrypoint"]
CMD ["base-usage"]

# Reset permissions of modified directories and add default user
RUN yum update -y \
    && yum install -y epel-release yum-utils \
    && yum clean all && rm -rf /var/cache/yum \
    && rpm-file-permissions \
    && useradd -u 1001 -r -g 0 -d ${HOME} -s /sbin/nologin \
      -c "Non-root Default Application User" default \
    && chown -R 1001:0 ${APP_ROOT}
