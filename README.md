# ollama-docker

A simple docker compose file to start ollama docker container and a model along with it.

## Setup docker

Follow official guide [here](https://docs.docker.com/engine/install/ubuntu/) or following

Remove unoffical packages

```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

Configure docker repository

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

Install Docker packages

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Allow docker to run without sudo

```bash
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
```

Test docker
```bash
docker run hello-world
```

## Setup for GPU

Configure nvidia-container-toolkit repository

```bash
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

Install container-toolkit

```bash
sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit
```

Configure container runtime
```bash
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```

## Clone the repository

```bash
git clone https://github.com/CollaborativeRoboticsLab/ollama-docker.git
```

## Start the container

```bash
cd ollama-docker
docker compose -f ollama-compose-gpu.yaml pull
docker compose -f ollama-compose-gpu.yaml up
```

## Select a model

Open a terminal and run the following command

```bash
cd ollama-docker
docker compose -f ollama-compose-gpu.yaml exec ollama bash
```

## Terminal to the container

Open a terminal and run the following command

```bash
cd ollama-docker
docker compose -f ollama-compose-gpu.yaml exec ollama ollama run llama3.2
```