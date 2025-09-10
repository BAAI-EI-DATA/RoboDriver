# Contributing / 贡献指南

Thank you for your interest in contributing to RoboDriver! / 感谢你对 RoboDriver 的贡献意愿！

This guide covers how to set up a dev environment, code style, and how to make PRs. / 本指南涵盖开发环境、代码风格、以及提 PR 的流程。

## Dev Environment / 开发环境

- Python: 3.11 recommended for platform, some robots use 3.10.
- Use separate Conda envs for platform vs. each robot.
- Quick steps:
  - `conda create -n RoboDriver python==3.11 && conda activate RoboDriver`
  - `pip install -e . && pip install ./wheels/*.whl`
  - For a robot (e.g., SO101): `cd operating_platform/robot/robots/so101_v1 && pip install -e .`
- PyTorch: see `QuickStart.md` for platform-specific installs.

## Style / 风格

- Python style: follow existing code patterns. If configured in repo, run formatters/linters before commit (e.g., black/ruff/isort). / 遵循现有代码风格；如仓库配置了格式化或静态检查，请在提交前运行。
- Types: add type hints where practical. / 尽量补充类型注解。
- Docs: update README or component READMEs when changing behavior. / 行为变更请更新相关文档。
- Small, focused PRs preferred. / 倡导小而聚焦的 PR。

## Commits / 提交信息

- Prefer Conventional Commits (e.g., `feat:`, `fix:`, `docs:`, `refactor:`). / 推荐使用约定式提交前缀。
- Reference issues with `Fixes #123` or `Refs #123` when relevant. / 关联 issue。

## Pull Requests / 拉取请求

- Before you start, open an issue for discussion if the change is non-trivial. / 对于非小改动，请先开 issue 沟通。
- Include: motivation, summary of changes, testing steps, and screenshots/logs if UI/vis related. / 在描述中说明动机、改动摘要、测试步骤等。
- Add or update tests if applicable. / 务必补充或更新测试（如适用）。
- Ensure `QuickStart.md` or component docs remain accurate. / 保持文档同步更新。

## Code Areas / 代码区域说明

- Components: `operating_platform/robot/components/*` (atomic Dora nodes). / 原子组件。
- Robots: `operating_platform/robot/robots/*` (dataflows + Python manipulators). / 机器人实现与数据流。
- Common configs: `operating_platform/robot/robots/com_configs/*`. / 通用配置。

## Release & Changelog / 版本与变更记录

- Follow SemVer. Update `CHANGELOG.md` using “Keep a Changelog” format. / 采用 SemVer，并在 `CHANGELOG.md` 中记录。

## Getting Help / 获取帮助

- Open an issue with as much detail as possible (logs, OS, Python, GPU). / 在 issue 中提供尽可能详细的信息（日志、环境等）。

