FROM dodasts/root-in-docker:v6.30-test

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

RUN git clone https://gitlab.cern.ch/cms-analysis/general/mkShapesRDF.git /code/

RUN sed -i '152a\
    set(VDT_LIBRARY "/opt/conda/lib/libvdt.so" CACHE INTERNAL "set VDT_LIBRARY env var")\n\
    set(VDT_INCLUDE_DIR "/opt/conda/include" CACHE INTERNAL "set VDT_INCLUDE_DIR env var")' /opt/conda/cmake/ROOTConfig.cmake

RUN cat /opt/conda/cmake/ROOTConfig.cmake
RUN cd /code/ && \
    ./install.sh docker && \
    mkdir xrdfs_locallib && \
    cd xrdfs_locallib && \
    rsync -ar --exclude 'python3.9' --exclude 'libROO*' --exclude 'libRoo*' /cvmfs/sft.cern.ch/lcg/views/LCG_103/x86_64-ubuntu2004-gcc9-opt/lib ./ && \
    cd .. 