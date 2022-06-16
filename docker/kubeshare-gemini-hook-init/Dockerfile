# call from repo root

FROM nvidia/11.6.1-devel-ubuntu20.04 AS build

WORKDIR /tmp/build

RUN apt update && \
    apt install -y git && \
    git clone https://github.com/jungyh0218/Gemini.git && cd Gemini && git checkout cuda-version-upgrade \
    make -C src

# FROM busybox:1.31.1-glibc
FROM alpine:3.9

COPY --from=build /tmp/build/Gemini/lib/libgemhook.so.1 /libgemhook.so.1

CMD ["cp", "/libgemhook.so.1", "/kubeshare/library/libgemhook.so.1"]
