ARG BUILD_FROM=ghcr.io/linuxserver/baseimage-alpine-nginx:3.12
FROM ${BUILD_FROM} as app

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ENV SCRIPT_ROOT=/opt/tt-rss

RUN \
    apk add --no-cache \
        php7=7.3.27-r0 \
        php7-fpm=7.3.27-r0 \
        php7-pdo=7.3.27-r0 \
        php7-gd=7.3.27-r0 \
        php7-pgsql=7.3.27-r0 \
        php7-pdo_pgsql=7.3.27-r0 \
        php7-mbstring=7.3.27-r0 \
        php7-intl=7.3.27-r0 \
        php7-xml=7.3.27-r0 \
        php7-curl=7.3.27-r0 \
        php7-session=7.3.27-r0 \
        php7-tokenizer=7.3.27-r0 \
        php7-dom=7.3.27-r0 \
        php7-fileinfo=7.3.27-r0 \
        php7-json=7.3.27-r0 \
        php7-iconv=7.3.27-r0 \
        php7-pcntl=7.3.27-r0 \
        php7-posix=7.3.27-r0 \
        php7-zip=7.3.27-r0 \
        php7-exif=7.3.27-r0 \
        php7-openssl=7.3.27-r0 \
        git=2.26.3-r0 \
        postgresql-client=12.6-r0 \
        sudo=1.9.5p2-r0 \
        php7-pecl-xdebug=2.9.8-r0 \
        rsync=3.1.3-r3 \
    && sed -i 's/\(memory_limit =\) 128M/\1 256M/' /etc/php7/php.ini \
    && sed -i -e 's/^listen = 127.0.0.1:9000/listen = 9000/' \
        -e 's/;\(clear_env\) = .*/\1 = no/i' \
        -e 's/^\(user\|group\) = .*/\1 = app/i' \
        -e 's/;\(php_admin_value\[error_log\]\) = .*/\1 = \/tmp\/error.log/' \
        -e 's/;\(php_admin_flag\[log_errors\]\) = .*/\1 = on/' \
        /etc/php7/php-fpm.d/www.conf \
        &&  sed -i 's/^;clear_env/clear_env/i' /etc/php7/php-fpm.d/www.conf

# Copy root filesystem
COPY rootfs /
 
ENV OWNER_UID=1000
ENV OWNER_GID=1000
ENV TTRSS_MYSQL_CHARSET="UTF8"
ENV TTRSS_PHP_EXECUTABLE="/usr/bin/php8"
ENV TTRSS_PLUGINS="auth_internal, note, nginx_xaccel"

# CMD ${SCRIPT_ROOT}/startup.sh

FROM app

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Laurent Bouriez <laurent@bouriez.org>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Home Assistant lbouriez Add-ons" \
    org.opencontainers.image.authors="Laurent Bouriez <laurent@bouriez.org>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://addons.community" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
