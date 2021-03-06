FROM ubuntu:18.04
RUN apt-get update && apt-get install -y git curl ca-certificates unzip xz-utils && \
    useradd rancher && \
    mkdir -p /var/lib/rancher/etcd /var/lib/cattle && \
    chown -R rancher /var/lib/rancher /var/lib/cattle /usr/local/bin
RUN curl -sLf https://github.com/rancher/machine-package/releases/download/v0.14.0-rancher4-1/docker-machine.tar.gz | tar xvzf - -C /usr/bin && \
    curl -sLf https://github.com/leodotcloud/loglevel/releases/download/v0.1/loglevel-amd64-v0.1.tar.gz | tar xvzf - -C /usr/bin && \
    curl -sLf https://github.com/rancher/helm/releases/download/v2.8.0-rancher3/helm > /usr/bin/helm && \
    chmod +x /usr/bin/helm
RUN mkdir /root/.kube && \
    ln -s /usr/bin/rancher /usr/bin/kubectl && \
    ln -s /var/lib/rancher/kube_config_cluster.yml /root/.kube/config && \
    ln -s /usr/bin/rancher /usr/bin/reset-password
WORKDIR /var/lib/rancher

ENV CATTLE_UI_PATH /usr/share/rancher/ui
ENV CATTLE_UI_VERSION 2.0.52
ENV CATTLE_CLI_VERSION v2.0.2-rc2

RUN mkdir -p /usr/share/rancher/ui && \
    cd /usr/share/rancher/ui && \
    curl -sL https://releases.rancher.com/ui/${CATTLE_UI_VERSION}.tar.gz | tar xvzf - --strip-components=1 && \
    cd /var/lib/rancher

ENV CATTLE_CLI_URL_DARWIN  https://releases.rancher.com/cli/${CATTLE_CLI_VERSION}/rancher-darwin-amd64-${CATTLE_CLI_VERSION}.tar.gz
ENV CATTLE_CLI_URL_LINUX   https://releases.rancher.com/cli/${CATTLE_CLI_VERSION}/rancher-linux-amd64-${CATTLE_CLI_VERSION}.tar.gz
ENV CATTLE_CLI_URL_WINDOWS https://releases.rancher.com/cli/${CATTLE_CLI_VERSION}/rancher-windows-386-${CATTLE_CLI_VERSION}.zip

ARG VERSION=dev
ENV CATTLE_SERVER_VERSION ${VERSION}
COPY rancher /usr/bin/

ENV CATTLE_AGENT_IMAGE rancher/rancher-agent:${VERSION}
ENV CATTLE_SERVER_IMAGE rancher/rancher
VOLUME /var/lib/rancher
ENTRYPOINT ["rancher", "--http-listen-port=80", "--https-listen-port=443"]
