FROM ghcr.io/comp-dev-cms-ita/kernel-root

WORKDIR /code

USER root

RUN apt-get update && \
    apt-get install -y vim rsync && \
    apt-get install -y voms-clients && \
    apt-get -y install python3.11-venv && \
    echo "deb http://archive.ubuntu.com/ubuntu/ jammy main universe" >> /etc/apt/sources.list.d/xrootd.list && \
    apt-get update && \
    apt-get install -y xrootd-client && \
    DEBIAN_FRONTEND=noninteractive apt install krb5-user -y libkrb5-dev libauthen-krb5-perl   

RUN git clone https://gitlab.cern.ch/cms-analysis/general/mkShapesRDF.git /code/

RUN cd /code/ && \
    ./install.sh docker
