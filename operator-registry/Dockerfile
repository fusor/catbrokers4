# To build this, from catbrokers4 project root run:
# docker build -t docker.io/automationbroker/osb-operators -f operator-registry/Dockerfile .
#
# You can also build this image based on your fork:
# docker build --build-arg ASB_REPO=djzager \
#              --build-arg ASB_BRANCH=example \
#              -t docker.io/djzager/osb-operators \
#              -f operator-registry/Dockerfile
FROM quay.io/openshift/origin-operator-registry

ARG ASB_REPO=openshift
ARG ASB_BRANCH=master
ARG TSB_REPO=openshift
ARG TSB_BRANCH=master

USER root
RUN yum -y install git

# Get ASB manifests
RUN mkdir -p /registry/automationbroker
RUN git clone --branch ${ASB_BRANCH} https://github.com/${ASB_REPO}/ansible-service-broker.git
RUN mv ansible-service-broker/operator/deploy/olm-catalog /registry/automationbroker

# Get TSB manifests
RUN mkdir -p /registry/templateservicebroker
RUN git clone --branch ${TSB_BRANCH} https://github.com/${TSB_REPO}/template-service-broker-operator.git
RUN mv template-service-broker-operator/deploy/olm-catalog /registry/templateservicebroker

# Initialize the database
RUN initializer --manifests /registry --output bundles.db

# There are multiple binaries in the origin-operator-registry
# We want the registry-server
ENTRYPOINT ["registry-server"]
CMD ["--database", "bundles.db"]
