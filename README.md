# ollama-docker

A simple docker compose file to start ollama docker container and a model along with it.

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