# Setup Guide

<!--TOC-->

- [System Requirements](#system-requirements)
- [Installation](#installation)
- [Downloading Checkpoints](#downloading-checkpoints)

<!--TOC-->

## System Requirements

* NVIDIA GPUs with Ampere architecture (RTX 30 Series, A100) or newer
* NVIDIA driver >=570.124.06 compatible with [CUDA 12.8.1](https://docs.nvidia.com/cuda/archive/12.8.1/cuda-toolkit-release-notes/index.html#cuda-toolkit-major-component-versions)
* Linux x86-64
* glibc>=2.35 (e.g Ubuntu >=22.04)

## Installation

Install [git lfs](https://git-lfs.com/):

```bash
sudo apt install git-lfs
git lfs install
```

Clone the repository:

```bash
git clone git@github.com:nvidia-cosmos/<repository_name>.git
cd <repository_name>
git lfs pull
```

### HuggingFace Token

1. Get a [Hugging Face Access Token](https://huggingface.co/settings/tokens) with `Read` permission.
2. Add to your `.bashrc`: `export HF_TOKEN=<TOKEN>`.
3. Accept the [NVIDIA Open Model License Agreement](https://huggingface.co/nvidia/Cosmos-Guardrail1).

### Docker Container

Please make sure you have access to Docker on your machine and the [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) is installed.

Build the container:

```bash
# Ampere - Hopper
docker compose build
```

Run the container:

```bash
docker compose run --rm app
```

If you get `docker: Error response from daemon: unknown or invalid runtime name: nvidia`, you need to [configure docker](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#configuring-docker):

```shell
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```

## Downloading Checkpoints

Inside the container:

1. Install [Hugging Face CLI](https://huggingface.co/docs/huggingface_hub/en/guides/cli): `uv tool install -U "huggingface_hub[cli]"`


Checkpoints are automatically downloaded during inference and post-training. To modify the checkpoint cache location, set the [`HF_HOME`](https://huggingface.co/docs/huggingface_hub/en/package_reference/environment_variables#hfhome) environment variable.
