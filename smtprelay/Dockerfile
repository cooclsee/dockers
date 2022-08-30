ARG SMTPRELAY_VERSION=1.8.0

FROM ubuntu:20.04
ARG SMTPRELAY_VERSION
WORKDIR /tmp
RUN if [ "$(uname -m)" = aarch64 ]; then export OS_ARCH=arm64; else OS_ARCH=amd64; fi \
    && apt update && apt install -y wget \
    && wget -O /tmp/smtprelay.tar.gz https://github.com/decke/smtprelay/releases/download/v${SMTPRELAY_VERSION}/smtprelay-v${SMTPRELAY_VERSION}-linux-${OS_ARCH}.tar.gz \
    && tar zxvf /tmp/smtprelay.tar.gz \
    && chmod +x /tmp/smtprelay


FROM gcr.io/distroless/base:latest
ARG SMTPRELAY_VERSION
ENV SMTPRELAY_VERSION $SMTPRELAY_VERSION
COPY --from=0 /tmp/smtprelay /usr/local/bin/smtprelay

EXPOSE 25
ENTRYPOINT [ "smtprelay" ]
CMD [ "--help" ]
