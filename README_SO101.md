# RoboDriver SO101

> GOSIM 2025

## 0. Start (with Docker) coming soon

## 1. Start (without Docker)

Get this project

```sh
git clone https://github.com/BAAI-EI-DATA/RoboDriver.git
cd RoboDriver
```

### 1.1. Initialize Platform Environment

Create Conda env

```sh
conda create --name RoboDriver python==3.11
```

Activate Conda env

```sh
conda activate RoboDriver
```

Install this project

```sh
pip install -e .
```

```sh
pip install ./wheels/*.whl
```

Install PyTorch (choose your platform)

```sh
# ROCM 6.1 (Linux only)
pip install torch==2.6.0 torchvision==0.21.0 torchaudio==2.6.0 --index-url https://download.pytorch.org/whl/rocm6.1
# ROCM 6.2.4 (Linux only)
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

Install libportaudio2 (Ubuntu)

```
sudo apt install libportaudio2
```

### 1.2. Initialize SO101 Environment

Open a new terminal and switch to the project directory.

Create Conda env

```sh
conda create --name dr-robot-so101 python==3.10
```

Activate Conda env

```sh
conda activate dr-robot-so101
```

Install robot environment

```sh
cd operating_platform/robot/robots/so101_v1
pip install -e .
```

### 1.3. Calibrate SO101 Arm

Calibrate leader arm

```
cd operating_platform/robot/components/arm_normal_so101_v1/
dora run dora_calibrate_leader.yml
```

Calibrate follower arm

```
cd operating_platform/robot/components/arm_normal_so101_v1/
dora run dora_calibrate_follower.yml
```

## 2. Teleoperate SO101 Arm

```
cd operating_platform/robot/components/arm_normal_so101_v1/
dora run dora_teleoperate_arm.yml
```

## 3. Record Data

First unplug all camera and arm USB connections. Then plug in the head camera.

```
ls /dev/video*
```

You should see:

```
/dev/video0 /dev/video1
```

If you see other indices, make sure all other cameras are disconnected. If you cannot remove them, update the camera index in the YAML file.

Then plug in the wrist camera.

```
ls /dev/video*
```

You should see:

```
/dev/video0 /dev/video1 /dev/video2 /dev/video3
```

Now camera connections are ready.

Next, connect the robotic arm by first plugging in the leader arm's USB interface.

```
ls /dev/ttyACM*
```

you can see:

```
/dev/ttyACM0
```

Then plug in the follower arm's USB interface.

```
ls /dev/ttyACM*
```

you can see:

```
/dev/ttyACM0 /dev/ttyACM1
```

Run Dora dataflow

```
cd operating_platform/robot/robots/so101_v1
conda activate dr-robot-so101
dora run dora_teleoperate_dataflow.yml
```

Open a new terminal, then run:

```
bash operating_platform/robot/robots/so101_v1/scripts/run_so101_cli.sh
```

You can modify task name/description by editing parameters in `scripts/run_so101_cli.sh`.


# Acknowledgment
 - LeRobot 🤗: [https://github.com/huggingface/lerobot](https://github.com/huggingface/lerobot)
