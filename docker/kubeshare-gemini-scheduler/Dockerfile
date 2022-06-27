# call from repo root

FROM nvidia/cuda:11.6.1-devel-ubuntu20.04 AS build

WORKDIR /tmp/build

COPY . kubeshare

RUN apt update && \
    apt install -y git && \
    git clone https://github.com/jungyh0218/Gemini.git && cd Gemini && git checkout v1.0-branch && \
    make -C src

FROM python:3.8.1-buster
RUN pip3 install inotify 

ENV NVIDIA_VISIBLE_DEVICES      all
ENV NVIDIA_DRIVER_CAPABILITIES  utility

COPY docker/lib/libdl.so.2          /lib
COPY docker/lib/libnvidia-ml.so.1   /lib

COPY --from=build /tmp/build/kubeshare/docker/kubeshare-gemini-scheduler/launcher-multigpus.sh /launcher-multigpus.sh
COPY --from=build /tmp/build/kubeshare/docker/kubeshare-gemini-scheduler/launcher.py /launcher.py
COPY --from=build /tmp/build/Gemini/bin/gem-schd /gem-schd
COPY --from=build /tmp/build/Gemini/bin/gem-pmgr /gem-pmgr

CMD ["/launcher-multigpus.sh", "/kubeshare/scheduler/config", "/kubeshare/scheduler/podmanagerport"]
