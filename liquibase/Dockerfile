FROM ubuntu:latest
LABEL maintainer="ets-gitlab-runner team"
RUN apt-get update && \
    apt-get install -y openjdk-11-jre-headless unzip curl wget && \
    rm -rf /var/lib/apt/lists/*
# The `rm -rf /var/lib/apt/lists/*` cleans up the apt cache from the first RUN block.

RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    ./aws/install && \
    # Add cleanup for the downloaded zip and extracted directory
    rm awscliv2.zip && \
    rm -rf aws/

# ===============================================
# New Block: Add Liquibase Installation
# ===============================================
ARG LIQUIBASE_VERSION=4.29.1
ENV LIQUIBASE_HOME=/opt/liquibase

RUN set -eux; \
    # Download the specified Liquibase version
    wget -q https://github.com/liquibase/liquibase/releases/download/v${LIQUIBASE_VERSION}/liquibase-${LIQUIBASE_VERSION}.zip -O /tmp/liquibase.zip && \
    # Create the installation directory
    mkdir -p ${LIQUIBASE_HOME} && \
    # Unzip the contents to the install directory
    unzip -q /tmp/liquibase.zip -d ${LIQUIBASE_HOME} && \
    # Cleanup the downloaded zip file
    rm /tmp/liquibase.zip

# Update PATH to include the liquibase executable
ENV PATH="${PATH}:${LIQUIBASE_HOME}"
# Set a default command or entry point
CMD ["/bin/bash"]
