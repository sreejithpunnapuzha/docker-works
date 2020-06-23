FROM quay.io/sreejithp/centos:8

RUN set -eux; \
	rpm --import http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-Official; \
	dnf update -y ; \
	dnf -y install dnf-plugin-ovl; \
	dnf -y update; \
	dnf install -y wget ca-certificates openssh-clients e2fsprogs iptables openssl \
	xfsprogs xz pigz libseccomp; \
	dnf -y clean all ;

ENV DOCKER_CHANNEL stable
ENV DOCKER_VERSION 19.03.9
ENV dockerArch x86_64
# https://github.com/docker/docker/tree/master/hack/dind
ENV DIND_COMMIT 37498f009d8bf25fbb6199e8ccd34bed84f2874b

RUN set -eux; \
	if ! wget -O docker.tgz "https://download.docker.com/linux/static/${DOCKER_CHANNEL}/${dockerArch}/docker-${DOCKER_VERSION}.tgz"; then \
	echo >&2 "error: failed to download 'docker-${DOCKER_VERSION}' from '${DOCKER_CHANNEL}' for '${dockerArch}'"; \
	exit 1; \
	fi; \
	tar --extract \
	--file docker.tgz \
	--strip-components 1 \
	--directory /usr/local/bin/ ; \
	rm docker.tgz; \
	dockerd --version; \
	docker --version; \
	groupadd -r dockremap; \
	adduser -r -g dockremap dockremap; \
	echo 'dockremap:165536:65536' >> /etc/subuid; \
	echo 'dockremap:165536:65536' >> /etc/subgid; \
	wget -O /usr/local/bin/dind "https://raw.githubusercontent.com/docker/docker/${DIND_COMMIT}/hack/dind"; \
	chmod +x /usr/local/bin/dind

COPY modprobe.sh /usr/local/bin/modprobe
COPY docker-entrypoint.sh /usr/local/bin/
COPY dockerd-entrypoint.sh /usr/local/bin/

ENV DOCKER_TLS_CERTDIR=/certs
RUN mkdir /certs /certs/client && chmod 1777 /certs /certs/client

VOLUME /var/lib/docker
EXPOSE 2375 2376

ENTRYPOINT ["dockerd-entrypoint.sh"]
CMD []
