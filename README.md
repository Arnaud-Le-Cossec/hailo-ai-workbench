# HAILO MODEL COMPILER ENVIRONEMENT

This project aims at creating an environement for HAILO compiler

## Requirements

**GPU**

## Install

### Install cuda

Install cuda

```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin &&
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600 &&
wget https://developer.download.nvidia.com/compute/cuda/13.2.1/local_installers/cuda-repo-wsl-ubuntu-13-2-local_13.2.1-1_amd64.deb &&
sudo dpkg -i cuda-repo-wsl-ubuntu-13-2-local_13.2.1-1_amd64.deb &&
sudo cp /var/cuda-repo-wsl-ubuntu-13-2-local/cuda-*-keyring.gpg /usr/share/keyrings/ &&
sudo apt-get update &&
sudo apt-get -y install cuda-toolkit-13-2

sudo apt update
sudo apt upgrade
sudo apt install build-essential
wget https://developer.download.nvidia.com/compute/cuda/13.2.1/local_installers/cuda_13.2.1_595.58.03_linux.run
# This can take a while
sudo sh cuda_13.2.1_595.58.03_linux.run
```

```bash
wget https://developer.download.nvidia.com/compute/cudnn/9.22.0/local_installers/cudnn-local-repo-ubuntu2404-9.22.0_1.0-1_amd64.deb
sudo dpkg -i cudnn-local-repo-ubuntu2404-9.22.0_1.0-1_amd64.deb
sudo cp /var/cudnn-local-repo-ubuntu2404-9.22.0/cudnn-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cudnn9-cuda-13
```

### 0. Install pyenv

We will need specific versions of python. If you don't already have it; I recommand you to install **Pyenv** :

```bash
sudo apt update; sudo apt install make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev curl git \
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

```bash
curl -fsSL https://pyenv.run | bash
```

and copy those lines into your `~/.bashrc` file:

```bash
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"

export LD_LIBRARY_PATH="/usr/local/cuda-13.2/lib64:$LD_LIBRARY_PATH"
export PATH="/usr/local/cuda-13.2/bin:$PATH"
```

Install the `pyenv-virtualenv` plugin

```bash
git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
```

### 1. Install python 3.10

With **pyenv**, you can easily install python `3.10` with the following command:

```bash
pyenv install 3.10
```

In the hailo-ai-workbench directory (the root directory of this repo), set the local python version to 3.10

```bash
pyenv local 3.10
```

Check version. This should return `Python 3.10.x`

```bash
python --version
```

### 2. Download HAILO tools

On the [HAILO developper zone](https://hailo.ai/developer-zone/software-downloads/?product=ai_accelerators&device=hailo_8_8l), download the **HAILO Dataflow Compiler (DFC)** and **HAILO Model Zoo (HMZ)**. Make sure to select the correct **NPU** (`HAILO8L`, `HAILO8`, `HAILO10H`, etc.) and **Python version** (`3.10`)

> You can place the files in the `ressources` folder 

### 3. Create the virtual environment

Create a virtual environment

```bash
python -m venv hailo-venv
```

### 4. Activate virtual environement

Activate the virtual environement

```bash
source hailo-venv/bin/activate
```

### 5. Prepare the virtual environment

Install hailo packages:

```bash
pip install ressources/hailo_dataflow_compiler-*.whl ressources/hailo_model_zoo-*.whl
```

### 6. Set up the NMS configuration file:

Create the postprocess_config folder:

```bash
mkdir -p $(pyenv root)/versions/hailo-venv/lib/python3.10/site-packages/hailo_model_zoo/cfg/postprocess_config
```

To obtain `yolov8s_nms_config.json`:
- Locate the zip URL in the YAML file above
- Download and extract the archive
- Copy the JSON file to the`postprocess_config` directory

### 7. Get model configuration file

Download the YAML configuration from the [networks configuration directory](https://github.com/hailo-ai/hailo_model_zoo/tree/833ae6175c06dbd6c3fc8faeb23659c9efaa2dbe/hailo_model_zoo/cfg/networks): `yolov8s.yaml`

```bash
mkdir -p tmp &&

git clone --depth 1 https://github.com/hailo-ai/hailo_model_zoo.git tmp/hailo_model_zoo &&
cp tmp/hailo_model_zoo/hailo_model_zoo/cfg/networks/yolov8s.yaml tmp/model.conf.yaml &&

curl -o tmp/model.zip $( grep -ohE "https:\/\/hailo-model-zoo.+\.zip" tmp/model.conf.yaml ) &&
unzip -d $(pyenv root)/versions/hailo-venv/lib/python3.10/site-packages/hailo_model_zoo/cfg/postprocess_config tmp/model.zip 

rm -rf tmp
```