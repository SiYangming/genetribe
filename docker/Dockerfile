# syntax=docker/dockerfile:1
# GeneTribe 1.2.1 — apt 最小化自建镜像（无官方 biocontainer）
#
# 构建：
#   docker build -t genetribe:1.2.1 -f docker/Dockerfile .
# 运行（挂载数据目录；pipeline 子命令在 cwd 找 <prefix>.bed/.fa/.chrlist）：
#   docker run --rm -u $(id -u):$(id -g) -v $PWD:/data -w /data genetribe:1.2.1 \
#       core -l aet -f rice -n 16
#
# 注意：ENTRYPOINT 直调 /opt/genetribe/genetribe（勿再 symlink 入口——上游按 argv[0] 相对路径找 bin/）。

FROM debian:bookworm-slim

LABEL org.opencontainers.image.title="genetribe" \
      org.opencontainers.image.version="1.2.1" \
      org.opencontainers.image.source="https://github.com/SiYangming/genetribe" \
      org.opencontainers.image.description="GeneTribe apt-minimal image (blast+bedtools+jcvi)"

ENV DEBIAN_FRONTEND=noninteractive \
    TMPDIR=/tmp \
    GENETRIBE_VERSION=1.2.1

ARG GENETRIBE_REPO=https://github.com/SiYangming/genetribe.git
ARG GENETRIBE_TAG=v1.2.1

RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        ca-certificates curl git gawk \
        ncbi-blast+ bedtools \
        python3 python3-venv python3-pip; \
    python3 -m venv /opt/genetribe-venv; \
    /opt/genetribe-venv/bin/pip install --no-cache-dir 'jcvi>=1.0.6'; \
    git clone --depth 1 --branch "${GENETRIBE_TAG}" \
        "${GENETRIBE_REPO}" /opt/genetribe; \
    cd /opt/genetribe; \
    ./install.sh; \
    test -x /opt/genetribe/genetribe; \
    apt-get purge -y curl git python3-pip; \
    apt-get autoremove -y; \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/* /usr/share/doc /usr/share/man /tmp/* /var/tmp/*

ENV PATH="/opt/genetribe:/opt/genetribe-venv/bin:${PATH}"

RUN set -eux; \
    out="$(genetribe -h)"; \
    printf '%s\n' "$out" | grep -q "GeneTribe"; \
    printf '%s\n' "$out" | grep -q "1.2.1"; \
    blastp -version | head -n 1; \
    bedtools --version; \
    python3 -c "import jcvi; print('jcvi OK')"

ENTRYPOINT ["/opt/genetribe/genetribe"]
CMD ["-h"]
