ARG IMAGE_REPO
FROM ${IMAGE_REPO:-lagoon}/kubectl

# the kubectl image comes with an HOME=/home which is needed to run as unpriviledged, but kubectl-build-deploy-dind will run as root
RUN rm -rf /root && ln -s /home /root
ENV LAGOON=kubectl-build-deploy-dind

RUN  mkdir -p /kubectl-build-deploy/git
RUN  mkdir -p /kubectl-build-deploy/lagoon

WORKDIR /kubectl-build-deploy/git

COPY docker-entrypoint.sh /lagoon/entrypoints/100-docker-entrypoint.sh
COPY build-deploy.sh /kubectl-build-deploy/build-deploy.sh
COPY build-deploy-docker-compose.sh /kubectl-build-deploy/build-deploy-docker-compose.sh
COPY rootless.values.yaml /kubectl-build-deploy/rootless.values.yaml

COPY scripts /kubectl-build-deploy/scripts

COPY helmcharts  /kubectl-build-deploy/helmcharts

ENV IMAGECACHE_REGISTRY=imagecache.amazeeio.cloud

ENV DBAAS_OPERATOR_HTTP=dbaas.lagoon.svc:5000

RUN curl -sSL https://github.com/uselagoon/lagoon-linter/releases/download/v0.5.0/lagoon-linter_0.5.0_linux_amd64.tar.gz \
    | tar -xz -C /usr/local/bin lagoon-linter

RUN curl -sSfL https://raw.githubusercontent.com/anchore/syft/main/install.sh | sh -s -- -b /usr/local/bin > /dev/null 2>&1
#curl -sSfL https://raw.githubusercontent.com/anchore/grype/main/install.sh | sh -s -- -b /usr/local/bin > /dev/null 2>&1

CMD ["/kubectl-build-deploy/build-deploy.sh"]
