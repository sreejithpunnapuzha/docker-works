FROM quay.io/sreejithp/centos:8

RUN rpm --import http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-Official; \
    dnf -y update; \
    dnf -y install dnf-plugin-ovl; \
    dnf -y update; \
    dnf -y install wget ca-certificates openssh-clients \
    && dnf clean all

ENV DOCKER_CHANNEL stable
ENV DOCKER_VERSION 19.03.9
ENV dockerArch x86_64

RUN set -eux; \
	\
	if ! wget -O docker.tgz "https://download.docker.com/linux/static/${DOCKER_CHANNEL}/${dockerArch}/docker-${DOCKER_VERSION}.tgz"; then \
		echo >&2 "error: failed to download 'docker-${DOCKER_VERSION}' from '${DOCKER_CHANNEL}' for '${dockerArch}'"; \
		exit 1; \
	fi; \
	tar --extract \
		--file docker.tgz \
		--strip-components 1 \
		--directory /usr/local/bin/ \
	; \
	rm docker.tgz; \
	dockerd --version; \
	docker --version

COPY modprobe.sh /usr/local/bin/modprobe
COPY docker-entrypoint.sh /usr/local/bin/

ENV DOCKER_TLS_CERTDIR=/certs
RUN mkdir /certs /certs/client && chmod 1777 /certs /certs/client

ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["sh"]

