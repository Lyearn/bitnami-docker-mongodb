FROM centos:7
LABEL maintainer "Bitnami <containers@bitnami.com>"

ENV HOME="/" \
    OS_ARCH="x86_64" \
    OS_FLAVOUR="centos-7" \
    OS_NAME="linux"

COPY prebuildfs /

RUN yum install -y epel-release

# Install required system packages and dependencies
RUN yum install -y acl ca-certificates curl gzip glibc libcom_err libcurl libffi libgcc libgcrypt gmp-devel gnutls libgpg-error krb5-libs nettle libidn keyutils openldap xz-libs libnghttp2 openssl-pkcs11 libpcap libpsl librtmp1 libgsasl libssh2 openssl libtasn1 libunistring numactl procps-ng tar zlib

RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "yq" "4.16.2-2" --checksum f168c946128443c4a3484975ef643ae7d74019e84d12c6643167287e7592ee85
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "wait-for-port" "1.0.1-5" --checksum 488628d237cb319add76d0e3989d18990562db323949d9968f5c2588a3ea04d1
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "render-template" "1.0.1-5" --checksum ab63e9a9646ebdfdde67e8b944ed50e52badeb6f8aa24cd70879a65a33fbe121
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "mongodb" "4.4.16-1" --checksum f9987f47e1e8f60452c4f229ab5c1c5557d83685687ee515e1bb168bca182170
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "gosu" "1.14.0-1" --checksum 5d374042b9320f3c931294d854835ffe602b499b2d71a4274769e12e6ba37eb7
RUN yum update -y && yum upgrade -y && \
    yum clean all && rm -fR /var/cache/yum
RUN chmod g+rwX /opt/bitnami

COPY rootfs /
RUN /opt/bitnami/scripts/mongodb/postunpack.sh
ENV BITNAMI_APP_NAME="mongodb" \
    BITNAMI_IMAGE_VERSION="4.4.16-centos-7" \
    PATH="/opt/bitnami/common/bin:/opt/bitnami/mongodb/bin:$PATH"

EXPOSE 27017

USER 1001
ENTRYPOINT [ "/opt/bitnami/scripts/mongodb/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/mongodb/run.sh" ]
