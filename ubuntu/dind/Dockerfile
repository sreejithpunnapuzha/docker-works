FROM quay.io/sreejithp/ubuntu-docker:latest

RUN set -eux; \
	apt update && apt dist-upgrade -y; \
	apt install -y \
	e2fsprogs \
	iptables \
	openssl \
	xfsprogs \
	xz-utils \
	pigz \
	libseccomp2 \
	; \
	apt autoremove -y && apt clean && rm -rf /var/lib/apt/lists/* ;

RUN set -x \
	&& addgroup --system dockremap \
	&& adduser --system --ingroup dockremap dockremap \
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

