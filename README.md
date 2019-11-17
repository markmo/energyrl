# rl-energy

### Docker Setup

Machine resources:

* Tested on 4 CPU and 4G RAM

### Installation

    cd <your-src-dir>
    git clone
    cd <>/energyplus
    docker build -t energyplus .
    cd ../model-app
    docker build -t model-app .
    cd ..

To run the model:

    docker run --name energyrl -v simdata:/var/simdata model-app


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
