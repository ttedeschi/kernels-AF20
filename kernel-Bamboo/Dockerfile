FROM ghcr.io/comp-dev-cms-ita/kernel-root

WORKDIR /code

USER root

RUN apt-get update && \
    apt-get install -y vim rsync && \
    apt-get install -y voms-clients && \
    apt-get -y install python3.11-venv ninja-build && \
    echo "deb http://archive.ubuntu.com/ubuntu/ jammy main universe" >> /etc/apt/sources.list.d/xrootd.list && \
    apt-get update && \
    apt-get install -y xrootd-client && \
    DEBIAN_FRONTEND=noninteractive apt install krb5-user -y libkrb5-dev libauthen-krb5-perl   

RUN wget https://go.dev/dl/go1.23.3.linux-amd64.tar.gz
RUN rm -rf /usr/local/go && tar -C /usr/local -xzf go1.23.3.linux-amd64.tar.gz
ENV PATH=$PATH:/usr/local/go/bin

RUN git clone https://github.com/dmwm/dasgoclient.git
WORKDIR dasgoclient
RUN go get github.com/dmwm/cmsauth
RUN go get github.com/dmwm/das2go
RUN go get github.com/vkuznet/x509proxy
RUN go get github.com/buger/jsonparser
RUN go get github.com/pkg/profile

RUN make build_all

RUN mv dasgoclient_amd64 /usr/local/go/bin/dasgoclient

RUN sed -i '152a\
    set(VDT_LIBRARY "/opt/conda/lib/libvdt.so" CACHE INTERNAL "set VDT_LIBRARY env var")\n\
    set(VDT_INCLUDE_DIR "/opt/conda/include" CACHE INTERNAL "set VDT_INCLUDE_DIR env var")' /opt/conda/cmake/ROOTConfig.cmake

RUN pip install git+https://gitlab.cern.ch/ttedesch/bamboo.git@allow-spool
#RUN pip install bamboo-hep

WORKDIR /code
