ARG NOTS_WORKER_VERSION=0.1
FROM ghcr.io/explodingcamera/nots-worker:${NOTS_WORKER_VERSION} as nots-worker

ARG BUN_VERSION=1.0
FROM oven/bun:${BUN_VERSION}-distroless as bun

ARG DEBIAN_VERSION=bookworm
FROM debian:${DEBIAN_VERSION}-slim

ARG EXTRA_DEPS=""

COPY --from=bun /usr/local/bin/bun /usr/local/bin/
COPY --from=nots-worker /usr/local/bin/nots-worker /usr/local/bin/

ENV USERNAME=noroot
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME --home-dir /app \
    && apt-get update && apt-get install -y --no-install-recommends ${EXTRA_DEPS} ca-certificates \
    && rm -rf /var/lib/apt/lists/*

USER noroot
ENTRYPOINT ["/usr/local/bin/nots-worker"]