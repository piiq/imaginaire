<img src="imaginaire_logo.svg" alt="imaginaire_logo.svg" height="360"/>

# Imaginaire

## [Docs](http://imaginaire.cc/docs) | [License](LICENSE.md) | [Installation](INSTALL.md) | [Model Zoo](MODELZOO.md)

## Installation Guide

Our library is developed using an Ubuntu 16.04 machine. We have not yet test our
  library on other operation systems.

### Prerequisite

    This is a log of the intallation on an Ubuntu 18.04 machine with a very ond GPU (CUDA capability 5.0)

- Ubuntu 18.04 with the latest GPU driver

Download the installer (Runfile) for the latest driver from [nvidia.com](https://www.nvidia.com/Download/index.aspx).

- [Anaconda3](https://www.anaconda.com/products/individual)

Install miniconda using a bash script from [conda.io](https://docs.conda.io/en/latest/miniconda.html#installing)

- [cuda10.2](https://developer.nvidia.com/cuda-toolkit)

    Install correct version of CUDA (the one that came with the driver might be higher). **Download** the required toolkit version from [developer.nvidia.com](https://developer.nvidia.com/cuda-toolkit-archive)

    Clean up cuda installation if required.

    ```bash
    sudo apt-get --purge -y remove 'cuda*'
    sudo apt-get --purge -y remove 'nvidia*'
    sudo apt autoremove -y
    sudo apt-get clean
    ```

    **Reboot.**

    Install cuda using the downloaded Runfile.

    ```bash
    wget http://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda_10.2.89_440.33.01_linux.run
    sudo sh cuda_10.2.89_440.33.01_linux.run
    ```

    When prompted - deselect the instalation of the driver. Select to install only the toolkit.


    In your `/usr/local/` folder a `cuda-10-2` was created by the installer
    Link it to `/usr/local/cuda` with

    ```bash
    sudo ln -s /usr/local/cuda-10-2 /usr/local/cuda
```

- [cudnn](https://developer.nvidia.com/cudnn)

    Download the required cudnn toolkit from [developer.nvidia.com](https://developer.nvidia.com/rdp/cudnn-download)

    Follow the installation guide from [docs.nvidia.com](https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html)

    ```bash
    tar -xzvf cudnn-x.x-linux-x64-v8.x.x.x.tgz
    sudo cp cuda/include/cudnn*.h /usr/local/cuda/include
    sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64

    sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*
    ```

### Installation

```bash
git clone https://github.com/nvlabs/imaginaire
cd imaginaire
bash scripts/install.sh
bash scripts/test_training.sh
```

If installation is not successful, error message will be prompted.

### Instalation on a machine with an old GPU

Old GPUs with CUDA capability 5.0 and lower require pytorch built from source.

Build pytorch locally with Cuda 5.0 arch support

```bash
git clone --recursive https://github.com/pytorch/pytorch  # new clone
# Install dependencies
conda install numpy pyyaml mkl mkl-include setuptools cmake cffi typing
conda install -c pytorch magma-cuda102
# Set environment variables
export NO_MKLDNN=1
export NO_SYSTEM_NCCL=1
export CUDNN_LIB_DIR="/usr/local/cuda/lib64"
export CUDNN_INCLUDE_DIR="/usr/local/cuda/include"
export CMAKE_PREFIX_PATH="$(dirname $(which conda))/../"
# Install
python setup.py install     # build and install
python setup.py clean --all # clean the build
```

Alternatively instead of running the install script a wheel can be made that can be installed manually

```bash
python setup.py bdist_wheel
```

### Docker Installation

We use NVIDIA docker image. We provide two ways to build the docker image.

1. Build a target docker image

    ```bash
    bash scripts/build_docker.sh 20.05
    ```

2. Build many docker images in one command

    ```bash
    bash scripts/build_all_dockers.sh
    ```

    If installation is not successful, error message will be prompted.

3. Launch an interactive docker container and test the imaginaire repo.

    ```bash
    cd docker
    bash start_local_docker.sh 20.05
    cd ${IMAGINAIRE_ROOT}
    bash scripts/test_training.sh
    ```

## Flake8

We follow the PEP8 style using flake8. To follow our practice, please do

```bash
pip install flake8
flake8 --install-hook git
git config --bool flake8.strict true
```

We set the maximum line length to 80. To avoid error messages due to different line length, create a file `~/.config/flake8` with the following content:

```
[flake8]
max-line-length = 80
```

## Windows Installation

- Install [git for windows](https://gitforwindows.org/)
- Install [Microsoft C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/)
- Install [Anaconda3](https://repo.anaconda.com/archive/Anaconda3-2020.02-Windows-x86_64.exe)
- Install [CUDA10.2](https://developer.nvidia.com/cuda-10.2-download-archive)
- Install [cudnn7.6.5](https://developer.nvidia.com/cudnn)
- Open a gitbash prompt
    - `cd YOUR_ROOT_DIR`
    - `git clone HTTP_PATH_TO_IMAGINAIRE`
    - `git clone https://github.com/NVIDIA/apex`
- Open an anaconda prompt.
    - `cd YOUR_ROOT_DIR\imaginaire2020`
    - `conda install -y pytorch==1.5.1 torchvision==0.6.1 cudatoolkit=10.2 -c pytorch`
    - `conda install -y -c anaconda pyqt`
    - `scripts\install.bat`
    - `cd YOUR_ROOT_DIR\apex`
    - `pip install -v --no-cache-dir .`
- Run the code
    - Remember to set up the python path in the windows prompt before running any demo code from the repo.
        - `cd YOUR_ROOT_DIR\imaginaire2020`
        - `set PYTHONPATH=%PYTHONPATH%;%cd%`
