# WanX-EI-Studio-Public

Robotics operating platform built on Dora dataflows and ZeroMQ, with sample robots (SO101, Realman, Pika, Aloha) and reusable components (cameras, arms, grippers, trackers, visualization).

## Quick Start

Create and activate Conda env:

```sh
conda create --name wanx-studio python==3.11
conda activate wanx-studio
```

Install this project and bundled wheels:

```sh
pip install -e .
pip install ./wheels/*.whl
```

Install PyTorch (choose your platform):

```sh
# ROCm 6.1 (Linux only)
pip install torch==2.6.0 torchvision==0.21.0 torchaudio==2.6.0 --index-url https://download.pytorch.org/whl/rocm6.1
# ROCm 6.2.4 (Linux only)
pip install torch==2.6.0 torchvision==0.21.0 torchaudio==2.6.0 --index-url https://download.pytorch.org/whl/rocm6.2.4
# CUDA 11.8
pip install torch==2.6.0 torchvision==0.21.0 torchaudio==2.6.0 --index-url https://download.pytorch.org/whl/cu118
# CUDA 12.4
pip install torch==2.6.0 torchvision==0.21.0 torchaudio==2.6.0 --index-url https://download.pytorch.org/whl/cu124
# CUDA 12.6
pip install torch==2.6.0 torchvision==0.21.0 torchaudio==2.6.0 --index-url https://download.pytorch.org/whl/cu126
# CPU only
pip install torch==2.6.0 torchvision==0.21.0 torchaudio==2.6.0 --index-url https://download.pytorch.org/whl/cpu
```

For audio capture (Ubuntu):

```sh
sudo apt update && sudo apt install -y libportaudio2
```

## Next Steps

- Chinese adaptation guide: [`QuickStart.md`](./QuickStart.md)
- SO101 full workflow: [`README_SO101.md`](./README_SO101.md)
