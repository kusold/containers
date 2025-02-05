FROM debian:bookworm-20250203

RUN apt-get update \
    && apt-get install -y curl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

VOLUME [ "/data" ]
WORKDIR /data
