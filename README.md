# Image to run angent test against gocd server

## DO Link reference
https://docs.digitalocean.com/products/container-registry/quickstart/#push-to-your-registry

```bash
doctl registry login
docker tag <my-image> registry.digitalocean.com/<my-registry>/<my-image>
docker push registry.digitalocean.com/<my-registry>/<my-image>
```

## Helper to build docker image

```bash
export REGISTRY_BASE=registry.digitalocean.com/four-pm-docker
export IMAGE_NAME=4pm-gocd-agent-python
export IMAGE_TAG=0.0.1-SNAPSHOT

docker build . --tag ${REGISTRY_BASE}/${IMAGE_NAME}:${IMAGE_TAG}
docker push ${REGISTRY_BASE}/${IMAGE_NAME}:${IMAGE_TAG}

docker run -d -e GO_SERVER_URL=https://goci.4pm.ie:443/go ${REGISTRY_BASE}/${IMAGE_NAME}:${IMAGE_TAG}
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