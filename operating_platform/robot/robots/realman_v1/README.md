# Realman v1 Robot

This folder contains the Realman v1 robot dataflow and a simple startup script.

## Start (local)

Use the script to launch the dataflow and coordinator sequentially (requires prepared Conda envs in your system):

```sh
bash operating_platform/robot/robots/realman_v1/scripts/start_realman.sh
```

Notes:
- Ensure the expected Conda environments exist: `op-robot-realman` (dataflow) and `op` (coordinator).
- The script runs `dora run operating_platform/robot/robots/realman_v1/robot_realman_dataflow.yml` and then `python coordinator.py --robot.type=realman`.
- Update IP/ports/camera indices in the config/dataflow if your hardware setup differs.
