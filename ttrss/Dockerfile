ARG BUILD_FROM
FROM $BUILD_FROM

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base
# hadolint ignore=DL3003
RUN \
	apk add --no-cache \
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
		/usr/local/share/.cache \
		/usr/lib/node_modules \
		/var/www/ttrss/.git \
	\
	&& sed -i.bak 's/^listen = 127.0.0.1:9000/listen = 9000/' /etc/php7/php-fpm.d/www.conf \
	&& sed -i.bak 's/\(memory_limit =\) 128M/\1 256M/' /etc/php7/php.ini

# https://github.com/caddyserver/caddy/releases
ENV CADDY_VERSION 2.3.0

# Install Caddy
RUN set -eux; \
	APKARCH="$(apk --print-arch)"; \
	case "$APKARCH" in \
		x86_64)  BINARCH='amd64' ;; \
		armhf)   BINARCH='armv6' ;; \
		armv7)   BINARCH='armv7' ;; \
		aarch64) BINARCH='arm64' ;; \
		*) echo >&2 "error: unsupported architecture ($apkArch)"; exit 1 ;;\
	esac; \
    curl -J -L -o /tmp/caddy.tar.gz "https://github.com/caddyserver/caddy/releases/download/v${CADDY_VERSION}/caddy_${CADDY_VERSION}_linux_${BINARCH}.tar.gz"; \
	tar zxvf /tmp/caddy.tar.gz -C /usr/bin caddy; \
	chmod +x /usr/bin/caddy; \
	rm -rf /tmp/caddy; \
	caddy version

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION
