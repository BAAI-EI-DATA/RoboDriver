# RoboDriver

[English](./README_en.md) | [中文](./README.md)

RoboDriver 是由 BAAI 具身数据团队打造的具身数据采集工具。它基于 Dora 数据流与 ZeroMQ 桥接构建机器人数据管线，内置参考机器人（SO101、Realman、Pika、Aloha）以及可复用节点（相机、机械臂、夹爪、追踪器、可视化）。

本文档介绍项目的目的与结构；安装与上手请查看 Quick Start 指南。

## 核心能力

- 多机器人：提供 SO101、Realman、Pika、Aloha 的参考实现，覆盖连接、控制与数据采集。
- 模块化组件：可组合的相机、机械臂、夹爪、6DoF 追踪器与可视化（Rerun）节点。
- 数据流编排：使用 Dora 数据流进行消息传递，通过 ZeroMQ 与 Python 控制端桥接，便于调试与集成。
- 多模态记录：支持 RGB、深度、关节状态等多种信号，适用于具身数据采集与标注。

## 仓库结构

- 机器人实现：`operating_platform/robot/robots/*`（如 `so101_v1/`、`realman_v1/`、`pika_v1/`、`aloha_v1/`）
- 原子组件：`operating_platform/robot/components/*`（如 `camera_*`、`arm_normal_*`、`gripper_pika`、`tracker_6d_vive`、`dora-rerun`）
- SO101 端到端示例：`README_SO101.md`

## 快速开始

请阅读 `QuickStart.md` 获取环境搭建与使用说明。

内容涵盖 Conda 环境、项目安装、PyTorch 选项、可选音频依赖，以及如何运行示例数据流。

## 路线图

以下功能正在开发中（预计约 1 个月内提供）：

- 集成 ROS1
- 集成 ROS2
- 支持可插拔的套接字传输用于数据流桥接（如 TCP/UDP，扩展 ZeroMQ 之外）

## 参与贡献

- 由 BAAI 具身数据团队维护。欢迎通过 Issues/PRs 参与共建。
