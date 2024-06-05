ARG HELMFILE_VERSION=0.165.0
FROM ghcr.io/helmfile/helmfile:v${HELMFILE_VERSION} as helmfile

FROM ubuntu:22.04

RUN DEBIAN_FRONTEND=noninteractive apt-get update && \
    apt-get install -y apt-transport-https ca-certificates curl gnupg && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get clean

RUN echo "Preparing kubectl..." && \
    curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | \
    gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg && \
    chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg && \
    echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | \
    tee /etc/apt/sources.list.d/kubernetes.list && \
    chmod 644 /etc/apt/sources.list.d/kubernetes.list && \
    echo "Preparing helm..." && \
    curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | tee /usr/share/keyrings/helm.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | \
    tee /etc/apt/sources.list.d/helm-stable-debian.list

ARG KUBECTL_VERSION=1.30.1-1.1
ARG HELM_VERSION=3.15.1-1

RUN DEBIAN_FRONTEND=noninteractive apt-get update && \
    apt-get install -y kubectl=${KUBECTL_VERSION} helm=${HELM_VERSION} git && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get clean

COPY --from=helmfile /usr/local/bin/helmfile /usr/local/bin/helmfile

RUN mkdir /nonexistent && \
    chown 65534:65534 /nonexistent
USER 65534:65534
WORKDIR /nonexistent

RUN helm plugin install https://github.com/databus23/helm-diff --version v3.9.7 && \
    helm plugin install https://github.com/jkroepke/helm-secrets --version v4.6.0 && \
    rm -rf /nonexistent/.cache/helm/plugins
