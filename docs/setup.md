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

<details id="docker-container"><summary><b>Docker Container</b></summary>

Please make sure you have access to Docker on your machine and the [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) is installed.

Build the container:

```bash
# Ampere - Hopper
image_tag=$(sudo docker build -f Dockerfile -q .)
```

Run the container:

```bash
sudo docker run -it --runtime=nvidia --ipc=host --rm -v .:/workspace -v /workspace/.venv -v /root/.cache:/root/.cache -e HF_TOKEN="$HF_TOKEN" -e NVIDIA_DRIVER_CAPABILITIES=compute,video,utility --gpus 'all,"capabilities=compute,video,utility"' $image_tag
```

Optional arguments:

* `--ipc=host`: Use host system's shared memory, since parallel torchrun consumes a large amount of shared memory. If not allowed by security policy, increase `--shm-size` ([documentation](https://docs.docker.com/engine/containers/run/#runtime-constraints-on-resources)).
* `-v /root/.cache:/root/.cache`: Mount host cache to avoid re-downloading cache entries.
* `-e HF_TOKEN="$HF_TOKEN"`: Set Hugging Face token to avoid re-authenticating.

If you get `docker: Error response from daemon: unknown or invalid runtime name: nvidia`, you need to [configure docker](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#configuring-docker):

```shell
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```

</details>

## Downloading Checkpoints

Inside the container:

1. Get a [Hugging Face Access Token](https://huggingface.co/settings/tokens) with `Read` permission
2. Install [Hugging Face CLI](https://huggingface.co/docs/huggingface_hub/en/guides/cli): `uv tool install -U "huggingface_hub[cli]"`
3. Login: `hf auth login`
4. Accept the [NVIDIA Open Model License Agreement](https://huggingface.co/nvidia/Cosmos-Guardrail1).

Checkpoints are automatically downloaded during inference and post-training. To modify the checkpoint cache location, set the [`HF_HOME`](https://huggingface.co/docs/huggingface_hub/en/package_reference/environment_variables#hfhome) environment variable.
