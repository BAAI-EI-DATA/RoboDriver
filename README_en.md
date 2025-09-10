# RoboDriver

[English](./README.md) | [中文](./README_zh.md)

RoboDriver is an embodied data collection toolkit from the BAAI Embodied Data Team. It builds robotics pipelines on Dora dataflows with a ZeroMQ bridge, and ships reference robots (SO101, Realman, Pika, Aloha) plus reusable nodes (cameras, arms, grippers, trackers, visualization).

This README describes the project’s purpose and structure. For installation and hands‑on usage, use the Quick Start guide.

## Core Capabilities

- Multi‑robot: reference implementations for SO101, Realman, Pika, and Aloha covering connection, control, and data capture.
- Modular components: composable nodes for cameras, arms, grippers, 6DoF trackers, and visualization (Rerun).
- Dataflow orchestration: Dora dataflows for message passing; ZeroMQ bridge for Python control and debugging.
- Multimodal logging: RGB, depth, joint states, and other signals for embodied data collection and annotation.

## Repository Layout

- Robot implementations: `operating_platform/robot/robots/*` (e.g., `so101_v1/`, `realman_v1/`, `pika_v1/`, `aloha_v1/`)
- Atomic components: `operating_platform/robot/components/*` (e.g., `camera_*`, `arm_normal_*`, `gripper_pika`, `tracker_6d_vive`, `dora-rerun`)
- SO101 end‑to‑end example: `README_SO101.md`

## Quick Start

Follow the setup and usage guide in `QuickStart.md`.

It covers Conda environments, project installation, PyTorch options, optional audio libraries, and running sample dataflows.

## Roadmap

- Integrate with ROS1
- Integrate with ROS2
- Support pluggable socket transports for dataflow bridging (e.g., TCP/UDP, beyond ZeroMQ)

## Contributing

- Maintained by the BAAI Embodied Data Team. Issues and PRs are welcome.
