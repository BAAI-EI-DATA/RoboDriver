# RoboDriver

<!-- Badges -->
<p>
  <a href=".github/workflows/ci.yml"><img alt="Build" src="https://img.shields.io/badge/CI-setup_pending-lightgrey"></a>
  <a href="LICENSE"><img alt="License" src="https://img.shields.io/badge/License-Apache_2.0-blue.svg"></a>
  <a href="QuickStart.md"><img alt="Docs" src="https://img.shields.io/badge/docs-QuickStart-green"></a>
  <a href="#"><img alt="PyPI" src="https://img.shields.io/badge/PyPI-pending-lightgrey"></a>
  
</p>

[English](./README_en.md) | [中文](./README.md)

RoboDriver 是由 BAAI 具身数据团队开发并维护的开源具身数据采集工具。它基于 Dora 数据流进行消息编排，并通过 ZeroMQ 桥接 Python 控制端。内置参考机器人（SO101、Realman、Pika、Aloha）与可复用节点（相机、机械臂、夹爪、6DoF 追踪器、可视化），可快速拼装数据采集流水线。

本文档介绍项目的目的与结构；安装与上手请查看 Quick Start 指南。

## 核心能力

- 多机器人支持：SO101、Realman、Pika、Aloha 等参考实现，覆盖连接、控制与数据采集。
- 组件化模块：可组合的相机、机械臂、夹爪、6DoF 追踪器与可视化（Rerun）节点，便于扩展与复用。
- 数据流编排：使用 Dora 数据流传递消息，配合 ZeroMQ 桥接 Python 控制端，便于调试与集成。
- 多模态记录：支持 RGB、深度、关节状态等信号，适用于采集、标注与重放等场景。

## 仓库结构

- 机器人实现：`operating_platform/robot/robots/*`（如 `so101_v1/`、`realman_v1/`、`pika_v1/`、`aloha_v1/`）
- 原子组件：`operating_platform/robot/components/*`（如 `camera_*`、`arm_normal_*`、`gripper_pika`、`tracker_6d_vive`、`dora-rerun`）
- SO101 端到端示例：`README_SO101.md`

## 快速开始

目标：帮助你参考仓库提供的范例，快速接入你自己的机器人。

请阅读 `QuickStart.md` 获取环境与使用说明，主要包括：

- Conda 环境创建与激活
- 项目与预编译 wheels 安装
- PyTorch 各平台安装选项
- 可选音频库依赖（Ubuntu）
- 运行示例数据流与联调指引
- 机器人适配指导：以 `operating_platform/robot/robots/<robot>_v1/` 为模板，按需修改 `configs.py` / `manipulator.py`

## 路线图

以下功能正在开发中（预计约 1 个月内提供）：

- 集成 ROS1
- 集成 ROS2
- 支持可插拔的套接字传输用于数据流桥接（如 TCP/UDP，扩展 ZeroMQ 之外）

## 参与贡献

- 由 BAAI 具身数据团队维护。欢迎通过 Issues/PR 提交反馈与贡献。
