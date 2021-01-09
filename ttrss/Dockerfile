ARG BUILD_FROM=hassioaddons/base:8.0.5
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base
# hadolint ignore=DL3003
RUN \
	apk add --no-cache \
		nginx \
		php7 \
		php7-fpm \
		php7-pdo \
		php7-gd \
		php7-pgsql \
		php7-pdo_pgsql \
		php7-mbstring \
		php7-intl \
		php7-xml \
		php7-curl \
		php7-session \
		php7-dom \
		php7-fileinfo \
		php7-json \
		php7-pcntl \
		php7-posix \
		php7-zip \
		php7-openssl \
	\
	&& apk add --no-cache --virtual .build-dependencies \
		git \
	\
	&& git clone --branch master --depth 1 \
		https://git.tt-rss.org/fox/tt-rss.git /var/www/ttrss \
	\
	&& apk del --no-cache --purge .build-dependencies \
	\
	&& find /var/www/ -type f -name "*.md" -depth -exec rm -f {} \; \
	&& rm -f -r \
		/tmp/* \
		/etc/nginx \
		/usr/local/share/.cache \
		/usr/lib/node_modules \
		/var/www/ttrss/.git

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION