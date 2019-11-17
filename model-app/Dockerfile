FROM energyplus

MAINTAINER Mark Moloney m4rkmo@gmail.com

ENV PYTHONPATH=/usr/local/src/rl-energy/
ENV ENERGYPLUS_WEATHER=/root/USA_CA_San.Francisco.Intl.AP.724940_TMY3.epw
ENV ENERGYPLUS_LOGBASE=/var/simdata/eplog
ENV ENERGYPLUS_MODEL=/root/2ZoneDataCenterHVAC_wEconomizer_Temp_Fan.idf
ENV ENERGYPLUS=/var/simdata/energyplus

COPY USA_CA_San.Francisco.Intl.AP.724940_TMY3.epw /root/
COPY 2ZoneDataCenterHVAC_wEconomizer_Temp_Fan.idf /root/
COPY src /usr/local/src/rl-energy

RUN apt-get update \
    && apt-get install -y build-essential libopenmpi-dev libsm6 libxext6 libxrender-dev

RUN apt-get update \
    && apt-get install -y python3-dev python3-pip

RUN python3 -m pip install --upgrade --quiet setuptools
RUN python3 -m pip install --quiet mpi4py gym baselines tensorflow==1.14.0 numpy pandas matplotlib

VOLUME /var/simdata
WORKDIR /var/simdata

CMD [ "python3", "-m", "baselines_energyplus.trpo_mpi.run_energyplus", "--num-timesteps", "1000000000" ]