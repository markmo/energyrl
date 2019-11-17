FROM ubuntu:18.04

MAINTAINER Mark Moloney m4rkmo@gmail.com

# This is a little untidy. EnergyPlus version format is inconsistent.
ENV ENERGYPLUS_VERSION=8.8.0
ENV ENERGYPLUS_INSTALL_VERSION=8-8-0
ENV ENERGYPLUS_TAG=v$ENERGYPLUS_VERSION
ENV ENERGYPLUS_SHA=7c3bbe4830
ENV WORKING_DIR=/usr/local/src

# Download from GitHub
ENV ENERGYPLUS_DOWNLOAD_BASE_URL https://github.com/NREL/EnergyPlus/releases/download/$ENERGYPLUS_TAG
ENV ENERGYPLUS_DOWNLOAD_FILENAME EnergyPlus-$ENERGYPLUS_VERSION-$ENERGYPLUS_SHA-Linux-x86_64.sh
ENV ENERGYPLUS_DOWNLOAD_URL $ENERGYPLUS_DOWNLOAD_BASE_URL/$ENERGYPLUS_DOWNLOAD_FILENAME

COPY RL-patch-for-EnergyPlus-8-8-0.patch /usr/local/src/

# Remove auxiliary files not needed in the container
RUN apt-get update \
    && apt-get install -y ca-certificates curl git-core cmake build-essential python2.7 python-pip \
    && rm -rf /var/lib/apt/lists/* \
    && curl -SLO $ENERGYPLUS_DOWNLOAD_URL \
    && chmod +x $ENERGYPLUS_DOWNLOAD_FILENAME \
    && echo "y\r" | ./$ENERGYPLUS_DOWNLOAD_FILENAME \
    && rm $ENERGYPLUS_DOWNLOAD_FILENAME \
    && cd /usr/local/EnergyPlus-$ENERGYPLUS_INSTALL_VERSION \
    && rm -rf DataSets Documentation ExampleFiles WeatherData MacroDataSets PostProcess/convertESOMTRpgm \
        PostProcess/EP-Compare PreProcess/FMUParser PreProcess/ParametricPreProcessor PreProcess/IDFVersionUpdater

# Remove broken symlinks
RUN cd /usr/local/bin \
    && find -L . -type l -delete

# Build patched EnergyPlus
RUN cd $WORKING_DIR \
    && git clone -b v8.8.0 https://github.com/NREL/EnergyPlus.git \
    && cd $WORKING_DIR/EnergyPlus \
    && patch -p1 < /usr/local/src/RL-patch-for-EnergyPlus-8-8-0.patch \
    && mkdir build \
    && cd build \
    && cmake -DCMAKE_INSTALL_PREFIX=/usr/local/EnergyPlus-8-8-0 .. \
    && make -j4 \
    && make install

VOLUME /var/simdata
WORKDIR /var/simdata

ENV ENERGYPLUS_DIR=/usr/local/EnergyPlus-${ENERGYPLUS_VERSION}
ENV ENERGYPLUS=${ENERGYPLUS_DIR}/energyplus

CMD [ "/bin/bash" ]