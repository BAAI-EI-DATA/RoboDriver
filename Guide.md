# WanX-EI‑Studio 使用与机器人适配指南

本仓库提供一套可组装的机器人数据采集系统，基于 Dora 数据流与 ZeroMQ 通讯，已经内置多种机器人/部件的示例实现（SO101、Realman、Pika、Aloha 等）与常用组件（摄像头、机械臂、夹爪、追踪器、可视化等）。本文档旨在帮助你：

- 快速搭建运行环境并运行内置示例
- 了解目录结构与关键概念
- 以现有机器人为模板，适配你自己的机器人


## 1. 环境与安装

建议按“平台环境 + 机器人环境”的方式分别安装，互不干扰，便于升级与调试。

### 1.1 平台环境（Python 3.11）

1) 创建并激活 Conda 环境

```bash
conda create -n wanx-studio python==3.11
conda activate wanx-studio
```

2) 安装本项目与内置轮子

```bash
pip install -e .
pip install ./wheels/*.whl
```

3) 安装 PyTorch（根据你的平台选择）

```bash
# ROCm 6.1 (Linux)
pip install torch==2.6.0 torchvision==0.21.0 torchaudio==2.6.0 --index-url https://download.pytorch.org/whl/rocm6.1
# ROCm 6.2.4 (Linux)
pip install torch==2.6.0 torchvision==0.21.0 torchaudio==2.6.0 --index-url https://download.pytorch.org/whl/rocm6.2.4
# CUDA 11.8
pip install torch==2.6.0 torchvision==0.21.0 torchaudio==2.6.0 --index-url https://download.pytorch.org/whl/cu118
# CUDA 12.4
pip install torch==2.6.0 torchvision==0.21.0 torchaudio==2.6.0 --index-url https://download.pytorch.org/whl/cu124
# CUDA 12.6
pip install torch==2.6.0 torchvision==0.21.0 torchaudio==2.6.0 --index-url https://download.pytorch.org/whl/cu126
# 仅 CPU
pip install torch==2.6.0 torchvision==0.21.0 torchaudio==2.6.0 --index-url https://download.pytorch.org/whl/cpu
```

如需语音采集，请安装系统库（Ubuntu）：

```bash
sudo apt update && sudo apt install -y libportaudio2
```

### 1.2 机器人环境（示例：SO101，Python 3.10）

机器人/数据流依赖会放在对应机器人的子包中（含 Dora CLI、OpenCV、ZMQ、驱动 SDK 等）。以 SO101 为例：

```bash
conda create -n dr-robot-so101 python==3.10
conda activate dr-robot-so101

cd operating_platform/robot/robots/so101_v1
pip install -e .
```

说明：
- 各机器人子包的 `pyproject.toml` 会列出依赖（如 `dora-rs-cli`、`opencv-python`、`pyzmq`、具体电机/传感器 SDK 等），`pip install -e .` 会自动安装。
- 其他机器人类似：`operating_platform/robot/robots/<robot>_v1/pyproject.toml`。


## 2. 目录结构与关键概念

- `operating_platform/robot/components/*`: 原子组件节点（Dora 节点），如 `camera_opencv`、`camera_rgbd_*`、`arm_normal_*`、`gripper_pika`、`tracker_6d_vive`、`dora-rerun` 可视化等。
- `operating_platform/robot/robots/<name>_v1/`: 具体机器人实现与数据流：
  - `manipulator.py`: 机器人 Python 控制端，实现连接、特征、动作发送等；通常通过 ZeroMQ 与 Dora 数据流桥接。
  - `dora_*.yml`: Dora 数据流编排文件（摄像头、机械臂、桥接等节点）。
  - `dora_zeromq.py`: 与 Python 端通信的 ZeroMQ 桥接节点（把 Dora 里的图像/关节消息发到 ZMQ，把动作从 ZMQ发回 Dora）。
  - `scripts/*.sh`: 启动脚本示例。
- `operating_platform/robot/robots/com_configs/`: 通用的配置类型定义（相机/电机总线），用于在 `configs.py` 中组合成具体机器人的配置。
- `operating_platform/robot/robots/configs.py`: 所有机器人配置的集中注册处（`@RobotConfig.register_subclass`），并提供 `make_robot_config` / `make_robot_from_config` 工厂方法。
- `operating_platform/core/`: 核心协同程序（可能以二进制形式存在，如 `coordinator.bin`）。
- `wheels/`: 预打包的 Python 轮子。

关键技术要点：
- Dora 数据流：`dora run <dataflow.yml>` 运行图形化的数据流，组件间通过端口消息通信。
- ZeroMQ 桥：数据流内使用 `dora_zeromq.py` 与外部 Python 控制端互通（图像/关节 → ZMQ；动作 ← ZMQ）。
- 机器人配置：`configs.py` 中为每种机器人定义了电机总线/相机等参数；`manipulator.py` 使用这些配置。


## 3. 运行 SO101（示例）

以下步骤与 `README_SO101.md` 一致，简化汇总如下。

### 3.1 标定

```bash
conda activate dr-robot-so101
cd operating_platform/robot/components/arm_normal_so101_v1/

# 标定主臂
dora run dora_calibrate_leader.yml

# 标定从臂
dora run dora_calibrate_follower.yml
```

### 3.2 远程操作/联动

1) 连接设备（先相机，后机械臂），确认设备索引：

```bash
ls /dev/video*     # 摄像头索引
ls /dev/ttyACM*    # 串口设备（机械臂）
```

如索引与 YAML 不一致，修改 `operating_platform/robot/robots/so101_v1/dora_teleoperate_dataflow.yml` 中对应 `env` 的 `CAPTURE_PATH` / `PORT`。

2) 运行 Dora 数据流：

```bash
cd operating_platform/robot/robots/so101_v1
conda activate dr-robot-so101
dora run dora_teleoperate_dataflow.yml
```

3) 另开一个终端，运行平台侧（根据你的脚本/入口）：

```bash
# 示例脚本（可按需修改记录参数）
bash operating_platform/robot/robots/so101_v1/scripts/run_so101_cli.sh
```

注：如使用自定义入口，请确保 Python 侧 `SO101Manipulator` 已连接 ZeroMQ，并能向 `so101_zeromq` 节点发送动作。


## 4. 适配你自己的机器人

建议基于一个最接近的现有机器人目录复制改造，例如拷贝 `operating_platform/robot/robots/so101_v1` 到 `myrobot_v1`，再按下述步骤修改。

### 4.1 定义机器人配置（configs）

打开并编辑 `operating_platform/robot/robots/configs.py`：

1) 新增一个配置类并注册：

```python
@RobotConfig.register_subclass("myrobot")
@dataclass
class MyRobotConfig(ManipulatorRobotConfig):
    leader_arms: dict[str, MotorsBusConfig] = field(default_factory=lambda: {
        "main": FeetechMotorsBusConfig(
            port="/dev/ttyACM0",
            motors={
                "joint_1": [1, "sts3215"],
                # ... 按你的电机映射填写
            },
        ),
    })

    follower_arms: dict[str, MotorsBusConfig] = field(default_factory=lambda: {
        "main": FeetechMotorsBusConfig(
            port="/dev/ttyACM1",
            motors={
                "joint_1": [1, "sts3215"],
                # ...
            },
        ),
    })

    cameras: dict[str, CameraConfig] = field(default_factory=lambda: {
        "image_top": OpenCVCameraConfig(camera_index=0, fps=30, width=640, height=480),
        # ... 其他相机
    })
```

2) 在工厂方法中加入分支：

```python
def make_robot_config(robot_type: str, **kwargs) -> RobotConfig:
    if robot_type == "myrobot":
        return MyRobotConfig(**kwargs)
    # 其他分支保持不变

def make_robot_from_config(config: RobotConfig):
    if isinstance(config, MyRobotConfig):
        from operating_platform.robot.robots.myrobot_v1.manipulator import MyRobotManipulator
        return MyRobotManipulator(config)
    # 其他分支保持不变
```

电机总线与相机类型在 `operating_platform/robot/robots/com_configs/` 中定义：

- 电机总线：`motors.py`（`FeetechMotorsBusConfig`、`DynamixelMotorsBusConfig`、`PiperMotorsBusConfig`、`PikaMotorsBusConfig` 等）
- 相机：`cameras.py`（`OpenCVCameraConfig`、`IntelRealSenseCameraConfig`）

### 4.2 编写 Manipulator

在 `operating_platform/robot/robots/myrobot_v1/manipulator.py` 中实现与 `SO101Manipulator` 接口一致的方法（可直接参考复制并精简）：

- `connect()`：等待相机与关节数据就绪（通常由 Dora → ZeroMQ 提供）
- `camera_features`/`motor_features`：声明观测与动作向量的结构
- `send_action(action: dict | Tensor)`：把动作打包后经 ZeroMQ 发回数据流
- `disconnect()`：清理线程、套接字

若使用 ZeroMQ 方式桥接，请为你的机器人定义独立的 IPC 地址前缀，避免与其他机器人冲突。

### 4.3 编排 Dora 数据流

在 `operating_platform/robot/robots/myrobot_v1/` 新建数据流 YAML，例如：

```yaml
nodes:
  - id: camera_top
    path: ../../components/camera_opencv/main.py
    inputs:
      tick: dora/timer/millis/33
    outputs: [image]
    env:
      CAPTURE_PATH: 0
      IMAGE_WIDTH: 640
      IMAGE_HEIGHT: 480

  - id: arm_myrobot_leader
    path: ../../components/arm_normal_so101_v1/main.py   # 按你的电机驱动更换
    inputs: { get_joint: dora/timer/millis/33 }
    outputs: [joint]
    env:
      GET_DEVICE_FROM: PORT
      PORT: /dev/ttyACM0
      ARM_NAME: MYROBOT-leader
      ARM_ROLE: leader

  - id: arm_myrobot_follower
    path: ../../components/arm_normal_so101_v1/main.py   # 按你的电机驱动更换
    inputs:
      get_joint: dora/timer/millis/33
      action_joint: arm_myrobot_leader/joint
      action_joint_ctrl: myrobot_zeromq/action_joint
    outputs: [joint]
    env:
      GET_DEVICE_FROM: PORT
      PORT: /dev/ttyACM1
      ARM_NAME: MYROBOT-follower
      ARM_ROLE: follower

  - id: myrobot_zeromq
    path: dora_zeromq.py
    inputs:
      image_top:   camera_top/image
      main_leader_joint:   arm_myrobot_leader/joint
      main_follower_joint: arm_myrobot_follower/joint
    outputs: [action_joint]
```

确保 `dora_zeromq.py` 与 `manipulator.py` 中的 IPC 地址匹配，事件名一致（如 `image_*`、`*_joint`、`action_joint`）。

### 4.4 启动与调试

1) 进入机器人环境，运行数据流：

```bash
conda activate dr-robot-myrobot
cd operating_platform/robot/robots/myrobot_v1
dora run dora_teleoperate_dataflow.yml
```

2) 另开终端，运行 Python 控制端（你的入口/脚本）：

```bash
python your_entry.py --robot.type=myrobot
```

3) 若需记录数据，参考 `scripts/` 中的示例参数（如 `--record.repo_id`、`--record.single_task` 等），或在你的入口中接入相应逻辑。


## 5. 常见问题

- 摄像头找不到/图像黑屏：检查 `CAPTURE_PATH` 与实际 `/dev/video*` 是否一致；UVC 相机可能被占用，关闭其它占用进程。
- 串口无权限：在 Linux 下将当前用户加入 `dialout` 组或 `udev` 规则，或用 `sudo`（不推荐长期使用）。
- Dora 未安装或找不到命令：确认在对应机器人环境下已通过 `pip install -e .` 安装了 `dora-rs-cli`（各机器人子包依赖中已声明）。
- ZMQ 超时：确认 `dora_zeromq.py` 正在运行、IPC 地址一致，且 Manipulator 端与之匹配。
- 依赖冲突：平台环境（3.11）与机器人环境（3.10/3.11）分离安装；若需统一版本，请自行在 `pyproject.toml` 中调整并测试。


## 6. 参考入口与文档

- 根目录快速安装：`README.md`
- SO101 全流程：`README_SO101.md`
- 组件说明：
  - OpenCV 相机：`operating_platform/robot/components/camera_opencv/README.md`
  - Orbbec/Realsense 相机：`operating_platform/robot/components/camera_rgbd_orbbec_v1/README.md`、`operating_platform/robot/components/camera_rgbd_orbbec_v2/README.md`、`operating_platform/robot/components/camera_rgbd_realsense/pyproject.toml`
  - 机械臂驱动：`operating_platform/robot/components/arm_normal_*/*`
  - Pika 夹爪：`operating_platform/robot/components/gripper_pika/README.md`
  - Rerun 可视化：`operating_platform/robot/components/dora-rerun/README.md`
- 机器人示例：
  - SO101：`operating_platform/robot/robots/so101_v1/README.md`
  - Realman：`operating_platform/robot/robots/realman_v1/README.md`
  - Pika：`operating_platform/robot/robots/pika_v1/README.md`
  - Aloha：`operating_platform/robot/robots/aloha_v1/README.md`


## 7. 小结

本仓库通过“Dora 数据流 + ZeroMQ 桥接 + Python Manipulator”的模式，将相机/机械臂/夹爪等组件解耦为可组合节点。你可以快速复用现有示例，按需扩展配置与数据流，完成你自己的机器人适配与联动/采集任务。

