# call from repo root

FROM nvidia/cuda:11.6.1-devel-ubuntu20.04 AS build

WORKDIR /tmp/build

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=US/Central

RUN apt update && \
    apt install -y git && \
    git clone https://github.com/jungyh0218/Gemini.git && cd Gemini && git checkout 11.3-support  && \
    make -C src

# FROM busybox:1.31.1-glibc
FROM alpine:3.9

COPY --from=build /tmp/build/Gemini/lib/libgemhook.so.1 /libgemhook.so.1

CMD ["cp", "/libgemhook.so.1", "/kubeshare/library/libgemhook.so.1"]
