# RoboDriver

<!-- Badges -->
<p>
  <a href=".github/workflows/ci.yml"><img alt="Build" src="https://img.shields.io/badge/CI-setup_pending-lightgrey"></a>
  <a href="LICENSE"><img alt="License" src="https://img.shields.io/badge/License-Apache_2.0-blue.svg"></a>
  <a href="QuickStart.md"><img alt="Docs" src="https://img.shields.io/badge/docs-QuickStart-green"></a>
  <a href="#"><img alt="PyPI" src="https://img.shields.io/badge/PyPI-pending-lightgrey"></a>
  
</p>

[English](./README_en.md) | [中文](./README.md)

RoboDriver is an open-source embodied data collection toolkit from the BAAI Embodied Data Team. It builds robotics data pipelines on Dora dataflows, with a ZeroMQ bridge to Python controllers. The repo ships reference robots (SO101, Realman, Pika, Aloha) and reusable nodes (cameras, arms, grippers, 6DoF trackers, visualization) to compose pipelines quickly.

This README describes the project’s purpose and structure. For installation and hands‑on usage, use the Quick Start guide.

## Core Capabilities

- Multi‑robot support: reference implementations for SO101, Realman, Pika, and Aloha covering connection, control, and data capture.
- Modular components: composable nodes for cameras, arms, grippers, 6DoF trackers, and visualization (Rerun).
- Dataflow orchestration: Dora dataflows for message passing; ZeroMQ bridge to Python control for easy integration and debugging.
- Multimodal logging: RGB, depth, joint states, and other signals for collection, annotation, and replay.

## Repository Layout

- Robot implementations: `operating_platform/robot/robots/*` (e.g., `so101_v1/`, `realman_v1/`, `pika_v1/`, `aloha_v1/`)
- Atomic components: `operating_platform/robot/components/*` (e.g., `camera_*`, `arm_normal_*`, `gripper_pika`, `tracker_6d_vive`, `dora-rerun`)
- SO101 end‑to‑end example: `README_SO101.md`

## Quick Start

Goal: help you quickly integrate your own robot by following the provided examples/templates.

Follow `QuickStart.md` for environment setup and usage. It covers:

- Creating and activating Conda environments
- Installing the project and bundled wheels
- Installing PyTorch for your platform
- Optional audio libraries (Ubuntu)
- Running sample dataflows and basic debugging
- Robot adaptation: start from `operating_platform/robot/robots/<robot>_v1/` as a template and adjust `configs.py` / `manipulator.py`

## Roadmap

The following items are in development (ETA ~1 month):

- Integrate with ROS1
- Integrate with ROS2
- Support pluggable socket transports for dataflow bridging (e.g., TCP/UDP, beyond ZeroMQ)

## Contributing

- Maintained by the BAAI Embodied Data Team. Issues and PRs are welcome.
