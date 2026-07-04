# SLURM GRES config generator
This program generates SLURM GRES config.

## Requirements
- NVIDIA Driver (nvidia-smi)

## Usage
```
Usage:
gres_conf_gen [-n|--name DeviceName [Default:gpu]] [-h|--header HeaderFileName [Default:none]] [-a|--autodetect AutoDetectOption [Default:none]] [--no-cpus] [--sockets [NumSockets] [--cores-per-socket NumCores]]
```

## Output file
Default:

```
# Contents of [HeaderFileName]

Name=[DeviceName] File=/dev/nvidia0 CPUs=0-A
Name=[DeviceName] File=/dev/nvidia1 CPUs=A-B
...
Name=[DeviceName] File=/dev/nvidiaX CPUs=Q-P
```

Socket-boundary CPU allocation:

```
gres_conf_gen --sockets
```

Socket CPU ranges are detected from `lscpu -p=CPU,SOCKET`, and GPUs are assigned to sockets by GPU number.

Socket-count auto detection with contiguous CPU ranges:

```
gres_conf_gen --sockets --cores-per-socket 64
```

Manual socket count with contiguous CPU ranges:

```
gres_conf_gen --sockets 2 --cores-per-socket 64
```

The CPU range is assigned by GPU number and always uses full socket ranges. For example, on an 8 GPU / 2 socket node:

```
Name=gpu        File=/dev/nvidia0 CPUs=0-63
Name=gpu        File=/dev/nvidia1 CPUs=0-63
Name=gpu        File=/dev/nvidia2 CPUs=0-63
Name=gpu        File=/dev/nvidia3 CPUs=0-63
Name=gpu        File=/dev/nvidia4 CPUs=64-127
Name=gpu        File=/dev/nvidia5 CPUs=64-127
Name=gpu        File=/dev/nvidia6 CPUs=64-127
Name=gpu        File=/dev/nvidia7 CPUs=64-127
```

No CPU allocation:

```
gres_conf_gen --no-cpus
```

```
Name=gpu        File=/dev/nvidia0
Name=gpu        File=/dev/nvidia1
...
```
