ARG PYTHON_VERSION="3.9"
ARG PULUMI_VERSION=latest

# --------------------------------------------------------------------------------

FROM debian:buster-slim AS builder
RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get install -y \
    curl \
    build-essential \
    git

RUN apt-get update && apt-get -y install --no-install-recommends awscli
ENV TZ=Europe/Stockholm
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN if [ "$PULUMI_VERSION" = "latest" ]; then \
    curl -fsSL https://get.pulumi.com/ | bash; \
    else \
    curl -fsSL https://get.pulumi.com/ | bash -s -- --version $PULUMI_VERSION ; \
    fi

# --------------------------------------------------------------------------------

FROM mcr.microsoft.com/vscode/devcontainers/python:0-${PYTHON_VERSION}

COPY --from=builder /root/.pulumi/bin/pulumi /pulumi/bin/pulumi
COPY --from=builder /root/.pulumi/bin/*-python* /pulumi/bin/

ENV PATH "/pulumi/bin:${PATH}"
