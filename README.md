# Reinforcement Learning using the EnergyPlus Simulator 

### Docker Setup

Machine resources:

* Tested on 4 CPU and 4G RAM

### Installation

    cd <your-src-dir>
    git clone https://github.com/markmo/energyrl.git
    cd energyrl/energyplus
    docker build -t energyplus .
    cd ../model-app
    docker build -t model-app .
    cd ..

To run the model:

    docker run --name energyrl -v simdata:/var/simdata model-app

Output files are generated under the directory energyrl/simdata/eplog/openai-YYYY-MM-DD-HH-MM-SS-mmmmmm. These include:

* log.txt Log file generated by baselines Logger.
* progress.csv Log file generated by baselines Logger.
* output/episode-NNNNNNNN/ Episode data

Epsiode data contains the following files:

* 2ZoneDataCenterHVAC_wEconomizer_Temp_Fan.idf A copy of model file used in the simulation of the episode
* USA_CA_San.Francisco.Intl.AP.724940_TMY3.epw A copy of weather file used in the simulation of the episode
* eplusout.csv.gz Simulation result in CSV format
* eplusout.err Error message. You need make sure that there are no Severe errors
* eplusout.htm Human readable report file


### Notes

The docker-compose.yml installation is not ready yet. I separated the energyplus application 
and the reinforcement learning model into separate docker images. However, the problem with
running them as separate containers is that containers typically communicate with one another
via an HTTP service. Currently, the model is calling the energyplus app binary directly.

For the moment, I am creating a single container by having the model-app image inherit from
the energyplus app image, and then just running the model in an interactive session of the
model-app container with the name 'energyrl'.

Model-app requires at least Python 3.6 (or maybe 3.5.3). Typing issues with default version 
3.5.1 on Ubuntu 16.04.
