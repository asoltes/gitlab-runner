FROM ubuntu:latest
LABEL maintainer="ets-gitlab-runner team"
RUN apt-get update && \
    apt-get install -y jq unzip curl wget openjdk17-jre && \
    rm -rf /var/lib/apt/lists/*

RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    ./aws/install && \
    rm awscliv2.zip && \
    rm -rf aws/

ARG LIQUIBASE_VERSION=4.29.1
ENV LIQUIBASE_HOME=/opt/liquibase

RUN set -eux; \
    wget -q https://github.com/liquibase/liquibase/releases/download/v${LIQUIBASE_VERSION}/liquibase-${LIQUIBASE_VERSION}.zip -O /tmp/liquibase.zip && \
    mkdir -p ${LIQUIBASE_HOME} && \
    unzip -q /tmp/liquibase.zip -d ${LIQUIBASE_HOME} && \
    rm /tmp/liquibase.zip
ENV PATH="${PATH}:${LIQUIBASE_HOME}"

CMD ["/bin/bash"]
