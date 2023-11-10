ARG NOTS_WORKER_VERSION=0.1
FROM ghcr.io/explodingcamera/nots-worker:${NOTS_WORKER_VERSION} as nots-worker

ARG DEBIAN_VERSION=bookworm
ARG NODE_VERSION=20
FROM node:${DEBIAN_VERSION}-slim
ARG EXTRA_DEPS=""

COPY --from=nots-worker /usr/local/bin/nots-worker /usr/local/bin/

ENV USERNAME=noroot
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME --home-dir /app \
    && apt-get update && apt-get install -y --no-install-recommends ${EXTRA_DEPS} ca-certificates \
    && rm -rf /var/lib/apt/lists/* \
    && corepack enable \
    && corepack install --global pnpm@stable

USER noroot
ARG WORKERD_VERSION
ENTRYPOINT ["/usr/local/bin/nots-worker"]