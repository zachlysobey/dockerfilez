FROM alpine:3

# APK packages
#   * bash - for convenience
#   * git - VCS is useful
#   * curl - for gcloud, helm installs
#   * openssl - for helm install  
#   * python3 - for gcloud
#   * py3-pip - for python

# Other libraries & tools:
#   * gcloud - this is a GCloud development image afterall
#   * kubectl - kuberenetes (k8s) for cloud-native development
#   * helm - for k8s package management
#   * terraform - for declaritive, repeatable infra

ENV TF_VERSION=1.1.8

RUN apk update \
  && apk upgrade \
  && apk add bash git curl openssl python3 py3-pip 

# setup gcloud CLI
RUN curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-cli-381.0.0-linux-x86_64.tar.gz
RUN tar -xf google-cloud-cli-381.0.0-linux-x86_64.tar.gz
RUN ./google-cloud-sdk/install.sh \
  --usage-reporting false \
  --path-update true \
  --command-completion true \
  --rc-path /root/.bashrc

# setup kubectl
RUN /google-cloud-sdk/bin/gcloud components install kubectl
RUN /google-cloud-sdk/bin/gcloud components install gke-gcloud-auth-plugin

# setup helm
RUN curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
RUN chmod 700 get_helm.sh
RUN ./get_helm.sh

# helm repos
RUN helm repo add kubeshop https://kubeshop.github.io/helm-charts \
  && helm repo add testkube https://kubeshop.github.io/helm-charts \
  && helm repo update

# terraform
# terraform_1.1.8
ENV TF_FILENAME=terraform_${TF_VERSION}_linux_amd64.zip
ENV TF_ZIP_URL=https://releases.hashicorp.com/terraform/${TF_VERSION}/${TF_FILENAME}
RUN curl -O $TF_ZIP_URL \
  && unzip $TF_FILENAME \
  && mv terraform /usr/bin/terraform \
  && rm $TF_FILENAME

# intended to be used with `docker run -v ${PWD}:/workspace [...]`
WORKDIR /workspace
