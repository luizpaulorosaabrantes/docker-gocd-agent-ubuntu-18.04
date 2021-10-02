# Image to run angent test against gocd server

## Helper to build docker image

```bash
docker build . --tag luizpaulorosaabrantes/4pm-gocd-agent-debian:0.0.1-SNAPSHOT
docker run -d -e GO_SERVER_URL=https://goci.4pm.ie:443/go luizpaulorosaabrantes/4pm-gocd-agent-debian:0.0.1-SNAPSHOT
```


## THe image must me be able to run these commands

```bash
apt update
apt install -y python3 python3-distutils python3-setuptools python3-pip python3-venv
python3 -m pip install pipx 
python3 -m pipx ensurepath
export PATH=$PATH:/root/.local/bin
pipx install nox
python3 -m venv venv_root
source venv_root/bin/activate

# To test this it's necessary to have nox file
nox --error-on-missing-interpreters
nox --session unit_tests
```