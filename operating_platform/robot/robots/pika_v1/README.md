# Pika v1 Robot

This folder contains the Pika v1 robot dataflows and Docker scripts.

## Start (Docker)

Use the helper script to start coordinator and dataflows. It supports host-controlled container mode (default) or in-container mode.

```sh
# Host controls container (default)
bash operating_platform/robot/robots/pika_v1/docker/start.sh

# In-container mode (run directly inside container)
bash operating_platform/robot/robots/pika_v1/docker/start.sh --container

# Overwrite logs instead of appending
bash operating_platform/robot/robots/pika_v1/docker/start.sh -o
```

Logs are written to `logs/` in the working directory.

Adjust `CONTAINER_NAME`, `PROJECT_DIR`, conda env names, and dataflow paths in the script if your setup differs.

## Dataflows

- VIVE tracker: `operating_platform/robot/components/tracker_6d_vive/dora_vive_dual.yml`
- Pika gripper: `operating_platform/robot/components/gripper_pika/dora_gripper_pika_dual.yml`
- Pika robot: `operating_platform/robot/robots/pika_v1/robot_dataflow.yml`
