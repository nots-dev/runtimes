ARG NOTS_WORKER_VERSION=0.1
ARG DEBIAN_VERSION=bookworm
ARG NODE_VERSION=20

ARG DENO_VERSION=1.38.1
ARG DENO_IMAGE=denoland/deno:bin-${DENO_VERSION}

FROM ${DENO_IMAGE} AS deno
FROM ghcr.io/explodingcamera/nots-worker:${NOTS_WORKER_VERSION} as nots-worker
FROM debian:${DEBIAN_VERSION}-slim
ARG EXTRA_DEPS=""

COPY --from=deno /deno /usr/local/bin/
COPY --from=nots-worker /usr/local/bin/nots-worker /usr/local/bin/

ENV USERNAME=noroot
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME --home-dir /app \
    && apt-get update && apt-get install -y --no-install-recommends ${EXTRA_DEPS} ca-certificates \
    && rm -rf /var/lib/apt/lists/*

USER noroot
ARG WORKERD_VERSION
ENTRYPOINT ["/usr/local/bin/nots-worker"]