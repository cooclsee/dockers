FROM ubuntu:18.04 AS build

RUN apt-get update && \
         DEBIAN_FRONTEND="noninteractive" \
         apt-get install -y --no-install-recommends \
         build-essential \
         cmake \
         git \
         curl \
         vim \
         ca-certificates \
         tzdata \
         libssl-dev \
         libmysqlclient-dev \
         libx264-dev \
         libfaac-dev \
         libmp4v2-dev && \
         apt-get autoremove -y && \
         apt-get clean -y && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir -p /opt/media

WORKDIR /opt/media
RUN git clone --depth=1 https://github.com/xia-chu/ZLMediaKit && \
    cd ZLMediaKit && git submodule update --init --recursive && \
    mkdir -p build release/linux/Release/

WORKDIR /opt/media/ZLMediaKit/build
RUN cmake -DCMAKE_BUILD_TYPE=Release .. && \
    make

FROM ubuntu:18.04
#shell,rtmp,rtsp,rtsps,http,https,rtp
EXPOSE 9000/tcp
EXPOSE 1935/tcp
EXPOSE 554/tcp
EXPOSE 322/tcp
EXPOSE 80/tcp
EXPOSE 443/tcp
EXPOSE 10000/udp
EXPOSE 10000/tcp
RUN apt-get update && \
    DEBIAN_FRONTEND="noninteractive" \
    apt-get install -y --no-install-recommends \
         vim \
         ca-certificates \
         tzdata \
         libssl-dev \
         libx264-dev \
         libfaac-dev \
         ffmpeg \
         libmp4v2-dev && \
         apt-get autoremove -y && \
         apt-get clean -y && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /opt/media/bin/
COPY --from=build /opt/media/ZLMediaKit/release/linux/Release/MediaServer /opt/media/bin/MediaServer
ENV PATH /opt/media/bin:$PATH
CMD MediaServer
