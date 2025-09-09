# Aloha v1 Robot

This folder contains the Aloha v1 robot dataflow and scripts. You can launch the full stack inside Docker using the provided script.

## Prerequisites

- Docker installed and running
- A container named `operating_platform` prepared with required environments (see script for env names)
- Cameras and devices connected to the host and passed through to the container

## Start

Run the launcher script to start dataflow, coordinator and Rerun view in the container:

```sh
bash operating_platform/robot/robots/aloha_v1/scripts/run.sh
```

Notes:
- The script restarts the container, launches Dora dataflow, coordinator, and a 3D view.
- Check logs in the working directory: `dataflow.log`, `coordinator.log`, `rerun_3d_view.log`.
- Adjust `CONTAINER_NAME`, `PROJECT_DIR`, conda env names and `DATAFLOW_PATH` in the script if your setup differs.

## Dataflow

- Main dataflow file: `operating_platform/robot/robots/aloha_v1/robot_aloha_dataflow.yml`

If you need to run manually inside the container:

```sh
# Example (inside container)
source /opt/conda/etc/profile.d/conda.sh
conda activate op-robot-aloha
dora run operating_platform/robot/robots/aloha_v1/robot_aloha_dataflow.yml
```
