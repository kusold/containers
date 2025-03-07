FROM ghcr.io/jmbannon/ytdl-sub:2025.02.21

# Add POT Plugin - https://github.com/coletdjnz/yt-dlp-get-pot - https://github.com/Brainicism/bgutil-ytdlp-pot-provider
RUN apk add --no-cache py3-pip py3-setuptools \
    && python3 -m pip install -U bgutil-ytdlp-pot-provider
