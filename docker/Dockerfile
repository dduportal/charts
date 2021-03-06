FROM alpine

LABEL \
  maintainer="https://github.com/olblak" \
  repository="https://github.com/jenkins-infra/charts"

ENV HELM_HOME="/home/helm/.helm"

RUN \
  apk add --no-cache gnupg wget ca-certificates git bash curl

# Install kubectl
ENV HELM_VERSION=v3.2.1
ENV HELM_LOCATION="https://get.helm.sh"
ENV HELM_FILENAME="helm-${HELM_VERSION}-linux-amd64.tar.gz"
ENV HELM_SHA256="018f9908cb950701a5d59e757653a790c66d8eda288625dbb185354ca6f41f6b"
RUN wget ${HELM_LOCATION}/${HELM_FILENAME} && \
    sha256sum ${HELM_FILENAME} | grep -q "${HELM_SHA256}" && \
    tar zxf ${HELM_FILENAME} && mv /linux-amd64/helm /usr/local/bin/ && \
    rm ${HELM_FILENAME} && rm -r /linux-amd64

# Install helm
ENV KUBECTL_VERSION=v1.15.12
ENV KUBECTL_SHA256="a32b762279c33cb8d8f4198f3facdae402248c3164e9b9b664c3afbd5a27472e"
RUN curl -LO "https://storage.googleapis.com/kubernetes-release/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl" && \
    sha256sum kubectl | grep ${KUBECTL_SHA256} && \
    chmod +x kubectl && \
    mv kubectl /usr/local/bin/kubectl

# Instal sops
ENV SOPS_VERSION="v3.5.0"
RUN \
  wget https://github.com/mozilla/sops/releases/download/${SOPS_VERSION}/sops-${SOPS_VERSION}.linux -O /usr/local/bin/sops && \
  chmod +x /usr/local/bin/sops

# Instal helmfile
ENV HELMFILE_VERSION="0.116.0"
RUN \
  wget https://github.com/roboll/helmfile/releases/download/v${HELMFILE_VERSION}/helmfile_linux_amd64 -O /usr/local/bin/helmfile && \
  chmod +x /usr/local/bin/helmfile

RUN adduser -D -u 1000 helm

USER helm

WORKDIR /home/helm

RUN \
  helm plugin install https://github.com/databus23/helm-diff && \
  helm plugin install https://github.com/futuresimple/helm-secrets && \
  helm plugin install https://github.com/aslafy-z/helm-git.git

ENTRYPOINT "/usr/local/bin/helmfile"
