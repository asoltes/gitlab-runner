FROM alpine:3.20
LABEL maintainer="ets-gitlab-runner team"
RUN apk add --no-cache \
    bash \
    curl \
    unzip \
    wget \
    jq \
    python3 \
    py3-pip \
    groff \
    less \
    libc6-compat

RUN python3 -m venv /opt/aws-cli && \
    /opt/aws-cli/bin/pip install --no-cache-dir awscli && \
    ln -s /opt/aws-cli/bin/aws /usr/local/bin/aws
    
ARG LIQUIBASE_VERSION=4.29.1
ENV LIQUIBASE_HOME=/opt/liquibase

RUN set -eux; \
    wget -q https://github.com/liquibase/liquibase/releases/download/v${LIQUIBASE_VERSION}/liquibase-${LIQUIBASE_VERSION}.zip -O /tmp/liquibase.zip && \
    mkdir -p ${LIQUIBASE_HOME} && \
    unzip -q /tmp/liquibase.zip -d ${LIQUIBASE_HOME} && \
    rm /tmp/liquibase.zip

ENV PATH="${PATH}:${LIQUIBASE_HOME}"

CMD ["/bin/bash"]
