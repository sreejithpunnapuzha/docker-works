FROM quay.io/sreejithp/centos:7-docker-latest

RUN set -eux; \
	yum -y update; \
  yum -y install \
		e2fsprogs \
		iptables \
		openssl \
		xfsprogs \
		xz \
		pigz \
    libseccomp \
    shadow-utils \
  ; \
  yum -y clean all ;

RUN set -x \
	&& groupadd -r dockremap \
	&& adduser -r -g dockremap dockremap \
	&& echo 'dockremap:165536:65536' >> /etc/subuid \
	&& echo 'dockremap:165536:65536' >> /etc/subgid

# https://github.com/docker/docker/tree/master/hack/dind
ENV DIND_COMMIT 37498f009d8bf25fbb6199e8ccd34bed84f2874b

RUN set -eux; \
	wget -O /usr/local/bin/dind "https://raw.githubusercontent.com/docker/docker/${DIND_COMMIT}/hack/dind"; \
	chmod +x /usr/local/bin/dind

COPY dockerd-entrypoint.sh /usr/local/bin/

VOLUME /var/lib/docker
EXPOSE 2375 2376

ENTRYPOINT ["dockerd-entrypoint.sh"]
CMD []

