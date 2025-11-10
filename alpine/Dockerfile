# Stage 1: Build stage
FROM public.ecr.aws/docker/library/python:3.13.3-alpine AS builder

LABEL maintainer="CTP Platform Team"

RUN apk add --no-cache \
    bash \
    openjdk17-jre-headless \
    curl \
    unzip \
    wget \
    jq \
    groff \
    less \
    libc6-compat \
    gcompat \
    zip \
    gcc \
    musl-dev \
    libffi-dev \
    openssl-dev \
    cargo \
    git

# Install AWS CLI
RUN python3 -m venv /opt/aws-cli && \
    /opt/aws-cli/bin/pip install --no-cache-dir awscli

# Install SAM CLI
RUN python3 -m venv /opt/sam-cli && \
    /opt/sam-cli/bin/pip install --no-cache-dir aws-sam-cli

# Install Liquibase
ARG LIQUIBASE_VERSION=4.29.1
ENV LIQUIBASE_HOME=/opt/liquibase

RUN set -eux; \
    wget -q https://github.com/liquibase/liquibase/releases/download/v${LIQUIBASE_VERSION}/liquibase-${LIQUIBASE_VERSION}.zip -O /tmp/liquibase.zip && \
    mkdir -p ${LIQUIBASE_HOME} && \
    unzip -q /tmp/liquibase.zip -d ${LIQUIBASE_HOME} && \
    rm /tmp/liquibase.zip

# Stage 2: Runtime stage
FROM public.ecr.aws/docker/library/python:3.13.3-alpine

# Copy only what’s needed from builder
COPY --from=builder /opt/aws-cli /opt/aws-cli
COPY --from=builder /opt/sam-cli /opt/sam-cli
COPY --from=builder /opt/liquibase /opt/liquibase

# Minimal runtime dependencies
RUN apk add --no-cache \
    bash \
    openjdk17-jre-headless \
    curl \
    unzip \
    zip \
    jq \
    gcc \
    musl-dev \
    libffi-dev \
    openssl-dev \

# Symlinks for CLI binaries
RUN ln -s /opt/aws-cli/bin/aws /usr/local/bin/aws && \
    ln -s /opt/sam-cli/bin/sam /usr/local/bin/sam

ENV LIQUIBASE_HOME=/opt/liquibase
ENV PATH="${PATH}:${LIQUIBASE_HOME}"

CMD ["/bin/bash"]
