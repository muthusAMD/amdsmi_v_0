# AMD SMI CLI Tool

This tool acts as a command line interface for manipulating
and monitoring the amdgpu kernel, and is intended to replace
and deprecate the existing rocm_smi CLI tool & gpuv-smi tool.
It uses Ctypes to call the amd_smi_lib API.
Recommended: At least one AMD GPU with AMD driver installed

## Install CLI Tool and Python Library

### Requirements

* python 3.7+ 64-bit
* amdgpu driver must be loaded for amdsmi_init() to pass

### CLI Installation

Before amd-smi install, ensure previous versions of amdsmi library are uninstalled using pip:

```bash
python3 -m pip list | grep amd
python3 -m pip uninstall amdsmi
```

* Install amdgpu driver
* Install amd-smi-lib package through package manager
* amd-smi --help

### Install Example for Ubuntu 22.04

``` bash
python3 -m pip list | grep amd
python3 -m pip uninstall amdsmi
apt install amd-smi-lib
amd-smi --help
```

### Python Development Library Installation

This option is for users who want to develop their own scripts using amd-smi's python library

Verify that your python version is 3.7+ to install the python library

* Install amdgpu driver
* Install amd-smi-lib package through package manager
* cd /opt/rocm/share/amd_smi
* python3 -m pip install --upgrade pip
* python3 -m pip install --user .
* import amdsmi in python to start development

Warning: this will take precedence over the cli tool's library install, to avoid issues run these steps after every amd-smi-lib update.

#### Older RPM Packaged OS's

The default python versions in older RPM based OS's are not gauranteed to have the minium version.

For example RHEL 8 and SLES 15 are 3.6.8 and 3.6.15 . You will need to ensure the latest yaml package is installed ( pyyaml >= 5.1) pyyaml is installed to your pip instance:

``` bash
python3 -m pip install pyyaml
amd-smi list
```

While the CLI will work with these older python versions, to install the python development library you need to upgrade to python 3.7+

#### Python Library Install Example for Ubuntu 22.04

``` bash
apt install amd-smi-lib
amd-smi --help
cd /opt/rocm/share/amd_smi
python3 -m pip install --upgrade pip
python3 -m pip install --user .
```

``` bash
~$ python3
Python 3.8.10 (default, May 26 2023, 14:05:08)
[GCC 9.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import amdsmi
>>>
```

## Usage

amd-smi will report the version and current platform detected when running the command without arguments:

``` bash
~$ amd-smi
usage: amd-smi [-h]  ...

AMD System Management Interface | Version: 23.4.0.0 | Platform: Linux Baremetal

optional arguments:
  -h, --help          show this help message and exit

AMD-SMI Commands:
                      Descriptions:
    version           Display version information
    list              List GPU information
    static            Gets static information about the specified GPU
    firmware (ucode)  Gets firmware information about the specified GPU
    bad-pages         Gets bad page information about the specified GPU
    metric            Gets metric/performance information about the specified GPU
    process           Lists general process information running on the specified GPU
    event             Displays event information for the given GPU
    topology          Displays topology information of the devices
    set               Set options for devices
    reset             Reset options for devices
```

More detailed verison information is available from `amd-smi version`

Each command will have detailed information via `amd-smi [command] --help`

## Commands

For convenience, here is the help output for each command

``` bash
~$ amd-smi list --help
usage: amd-smi list [-h] [--json | --csv] [--file FILE] [--loglevel LEVEL]
                    [-g GPU [GPU ...]]

Lists all the devices on the system and the links between devices.
Lists all the sockets and for each socket, GPUs and/or CPUs associated to
that socket alongside some basic information for each device.
In virtualization environments, it can also list VFs associated to each
GPU with some basic information for each VF.

optional arguments:
  -h, --help               show this help message and exit
  -g, --gpu GPU [GPU ...]  Select a GPU ID, BDF, or UUID from the possible choices:
                              ID:0 | BDF:0000:23:00.0 | UUID:ffff73bf-0000-1000-80ff-ffffffffffff
                                all | Selects all devices

Command Modifiers:
  --json                   Displays output in JSON format (human readable by default).
  --csv                    Displays output in CSV format (human readable by default).
  --file FILE              Saves output into a file on the provided path (stdout by default).
  --loglevel LEVEL         Set the logging level from the possible choices:
                              DEBUG, INFO, WARNING, ERROR, CRITICAL
```

```bash
~$ amd-smi static --help
usage: amd-smi static [-h] [--json | --csv] [--file FILE] [--loglevel LEVEL]
                      [-g GPU [GPU ...]] [-a] [-b] [-V] [-d] [-v] [-c] [-B] [-r] [-p] [-l]
                      [-u]

If no GPU is specified, returns static information for all GPUs on the system.
If no static argument is provided, all static information will be displayed.

Static Arguments:
  -h, --help               show this help message and exit
  -g, --gpu GPU [GPU ...]  Select a GPU ID, BDF, or UUID from the possible choices:
                              ID:0 | BDF:0000:23:00.0 | UUID:ffff73bf-0000-1000-80ff-ffffffffffff
                                all | Selects all devices
  -a, --asic               All asic information
  -b, --bus                All bus information
  -V, --vbios              All video bios information (if available)
  -d, --driver             Displays driver version
  -v, --vram               All vram information
  -c, --cache              All cache information
  -B, --board              All board information
  -r, --ras                Displays RAS features information
  -p, --partition          Partition information
  -l, --limit              All limit metric values (i.e. power and thermal limits)
  -u, --numa               All numa node information

Command Modifiers:
  --json                   Displays output in JSON format (human readable by default).
  --csv                    Displays output in CSV format (human readable by default).
  --file FILE              Saves output into a file on the provided path (stdout by default).
  --loglevel LEVEL         Set the logging level from the possible choices:
                              DEBUG, INFO, WARNING, ERROR, CRITICAL
```

``` bash
~$ amd-smi firmware --help
usage: amd-smi firmware [-h] [--json | --csv] [--file FILE] [--loglevel LEVEL]
                        [-g GPU [GPU ...]] [-f]

If no GPU is specified, return firmware information for all GPUs on the system.

Firmware Arguments:
  -h, --help                   show this help message and exit
  -g, --gpu GPU [GPU ...]      Select a GPU ID, BDF, or UUID from the possible choices:
                               ID:0 | BDF:0000:23:00.0 | UUID:ffff73bf-0000-1000-80ff-ffffffffffff
                                all | Selects all devices
  -f, --ucode-list, --fw-list  All FW list information

Command Modifiers:
  --json                       Displays output in JSON format (human readable by default).
  --csv                        Displays output in CSV format (human readable by default).
  --file FILE                  Saves output into a file on the provided path (stdout by default).
  --loglevel LEVEL             Set the logging level from the possible choices:
                                  DEBUG, INFO, WARNING, ERROR, CRITICAL
```

```bash
~$ amd-smi bad-pages --help
usage: amd-smi bad-pages [-h] [--json | --csv] [--file FILE] [--loglevel LEVEL]
                         [-g GPU [GPU ...]] [-p] [-r] [-u]

If no GPU is specified, return bad page information for all GPUs on the system.

Bad Pages Arguments:
  -h, --help               show this help message and exit
  -g, --gpu GPU [GPU ...]  Select a GPU ID, BDF, or UUID from the possible choices:
                              ID:0 | BDF:0000:23:00.0 | UUID:ffff73bf-0000-1000-80ff-ffffffffffff
                                all | Selects all devices
  -p, --pending            Displays all pending retired pages
  -r, --retired            Displays retired pages
  -u, --un-res             Displays unreservable pages

Command Modifiers:
  --json                   Displays output in JSON format (human readable by default).
  --csv                    Displays output in CSV format (human readable by default).
  --file FILE              Saves output into a file on the provided path (stdout by default).
  --loglevel LEVEL         Set the logging level from the possible choices:
                              DEBUG, INFO, WARNING, ERROR, CRITICAL
```

```bash
~$ amd-smi metric --help
usage: amd-smi metric [-h] [--json | --csv] [--file FILE] [--loglevel LEVEL]
                      [-g GPU [GPU ...]] [-w INTERVAL] [-W TIME] [-i ITERATIONS] [-m] [-u]
                      [-p] [-c] [-t] [-e] [-k] [-P] [-f] [-C] [-o] [-l] [-x] [-E]

If no GPU is specified, returns metric information for all GPUs on the system.
If no metric argument is provided all metric information will be displayed.

Metric arguments:
  -h, --help                   show this help message and exit
  -g, --gpu GPU [GPU ...]      Select a GPU ID, BDF, or UUID from the possible choices:
                               ID:0 | BDF:0000:23:00.0 | UUID:ffff73bf-0000-1000-80ff-ffffffffffff
                                all | Selects all devices
  -w, --watch INTERVAL         Reprint the command in a loop of INTERVAL seconds
  -W, --watch_time TIME        The total TIME to watch the given command
  -i, --iterations ITERATIONS  Total number of ITERATIONS to loop on the given command
  -m, --mem-usage              Memory usage per block
  -u, --usage                  Displays engine usage information
  -p, --power                  Current power usage
  -c, --clock                  Average, max, and current clock frequencies
  -t, --temperature            Current temperatures
  -e, --ecc                    Total number of ECC errors
  -k, --ecc-block              Number of ECC errors per block
  -P, --pcie                   Current PCIe speed, width, and replay count
  -f, --fan                    Current fan speed
  -C, --voltage-curve          Display voltage curve
  -o, --overdrive              Current GPU clock overdrive level
  -l, --perf-level             Current DPM performance level
  -x, --xgmi-err               XGMI error information since last read
  -E, --energy                 Amount of energy consumed

Command Modifiers:
  --json                       Displays output in JSON format (human readable by default).
  --csv                        Displays output in CSV format (human readable by default).
  --file FILE                  Saves output into a file on the provided path (stdout by default).
  --loglevel LEVEL             Set the logging level from the possible choices:
                                  DEBUG, INFO, WARNING, ERROR, CRITICAL
```

```bash
~$ amd-smi process --help
usage: amd-smi process [-h] [--json | --csv] [--file FILE] [--loglevel LEVEL]
                       [-g GPU [GPU ...]] [-w INTERVAL] [-W TIME] [-i ITERATIONS] [-G]
                       [-e] [-p PID] [-n NAME]

If no GPU is specified, returns information for all GPUs on the system.
If no process argument is provided all process information will be displayed.

Process arguments:
  -h, --help                   show this help message and exit
  -g, --gpu GPU [GPU ...]      Select a GPU ID, BDF, or UUID from the possible choices:
                                ID: 0 | BDF: 0000:23:00.0 | UUID: c4ff73bf-0000-1000-802e-0812b504ed69
                                  all | Selects all devices
  -w, --watch INTERVAL         Reprint the command in a loop of INTERVAL seconds
  -W, --watch_time TIME        The total TIME to watch the given command
  -i, --iterations ITERATIONS  Total number of ITERATIONS to loop on the given command
  -G, --general                pid, process name, memory usage
  -e, --engine                 All engine usages
  -p, --pid PID                Gets all process information about the specified process based on Process ID
  -n, --name NAME              Gets all process information about the specified process based on Process Name.
                               If multiple processes have the same name information is returned for all of them.

Command Modifiers:
  --json                       Displays output in JSON format (human readable by default).
  --csv                        Displays output in CSV format (human readable by default).
  --file FILE                  Saves output into a file on the provided path (stdout by default).
  --loglevel LEVEL             Set the logging level from the possible choices:
                                DEBUG, INFO, WARNING, ERROR, CRITICAL
```

```bash
~$ amd-smi event --help
usage: amd-smi event [-h] [--json | --csv] [--file FILE] [--loglevel LEVEL]
                     [-g GPU [GPU ...]]

If no GPU is specified, returns event information for all GPUs on the system.

Event Arguments:
  -h, --help               show this help message and exit
  -g, --gpu GPU [GPU ...]  Select a GPU ID, BDF, or UUID from the possible choices:
                              ID:0 | BDF:0000:23:00.0 | UUID:ffff73bf-0000-1000-80ff-ffffffffffff
                                all | Selects all devices

Command Modifiers:
  --json                   Displays output in JSON format (human readable by default).
  --csv                    Displays output in CSV format (human readable by default).
  --file FILE              Saves output into a file on the provided path (stdout by default).
  --loglevel LEVEL         Set the logging level from the possible choices:
                              DEBUG, INFO, WARNING, ERROR, CRITICAL
```

```bash
~$ amd-smi topology --help
usage: amd-smi topology [-h] [--json | --csv] [--file FILE] [--loglevel LEVEL]
                        [-g GPU [GPU ...]] [-a] [-w] [-o] [-t] [-b]

If no GPU is specified, returns information for all GPUs on the system.
If no topology argument is provided all topology information will be displayed.

Topology arguments:
  -h, --help               show this help message and exit
  -g, --gpu GPU [GPU ...]  Select a GPU ID, BDF, or UUID from the possible choices:
                              ID:0 | BDF:0000:23:00.0 | UUID:ffff73bf-0000-1000-80ff-ffffffffffff
                                all | Selects all devices
  -a, --access             Displays link accessibility between GPUs
  -w, --weight             Displays relative weight between GPUs
  -o, --hops               Displays the number of hops between GPUs
  -t, --link-type          Displays the link type between GPUs
  -b, --numa-bw            Display max and min bandwidth between nodes

Command Modifiers:
  --json                   Displays output in JSON format (human readable by default).
  --csv                    Displays output in CSV format (human readable by default).
  --file FILE              Saves output into a file on the provided path (stdout by default).
  --loglevel LEVEL         Set the logging level from the possible choices:
                              DEBUG, INFO, WARNING, ERROR, CRITICAL

```

```bash
~$ amd-smi set --help
usage: amd-smi set [-h] [--json | --csv] [--file FILE] [--loglevel LEVEL] -g GPU [GPU ...]
                   [-f %] [-l LEVEL] [-P SETPROFILE] [-d SCLKMAX] [-C PARTITION]
                   [-M PARTITION] [-o WATTS]

A GPU must be specified to set a configuration.
A set argument must be provided; Multiple set arguments are accepted

Set Arguments:
  -h, --help                         show this help message and exit
  -g, --gpu GPU [GPU ...]            Select a GPU ID, BDF, or UUID from the possible choices:
                                        ID: 0 | BDF: 0000:23:00.0 | UUID: c4ff73bf-0000-1000-802e-0812b504ed69
                                          all | Selects all devices
  -f, --fan %                        Set GPU fan speed (0-255 or 0-100%)
  -l, --perf-level LEVEL             Set performance level
  -P, --profile SETPROFILE           Set power profile level (#) or a quoted string of custom profile attributes
  -d, --perf-determinism SCLKMAX     Set GPU clock frequency limit and performance level to determinism to get minimal performance variation
  -C, --compute-partition PARTITION  Set one of the following the compute partition modes:
                                        CPX, SPX, DPX, TPX, QPX
  -M, --memory-partition PARTITION   Set one of the following the memory partition modes:
                                        NPS1, NPS2, NPS4, NPS8
  -o, --power-cap WATTS              Set power capacity limit

Command Modifiers:
  --json                             Displays output in JSON format (human readable by default).
  --csv                              Displays output in CSV format (human readable by default).
  --file FILE                        Saves output into a file on the provided path (stdout by default).
  --loglevel LEVEL                   Set the logging level from the possible choices:
                                        DEBUG, INFO, WARNING, ERROR, CRITICAL
```

```bash
~$ amd-smi reset --help
usage: amd-smi reset [-h] [--json | --csv] [--file FILE] [--loglevel LEVEL] -g GPU
                     [GPU ...] [-G] [-c] [-f] [-p] [-x] [-d] [-C] [-M] [-o]

A GPU must be specified to reset a configuration.
A reset argument must be provided; Multiple reset arguments are accepted

Reset Arguments:
  -h, --help               show this help message and exit
  -g, --gpu GPU [GPU ...]  Select a GPU ID, BDF, or UUID from the possible choices:
                                ID: 0 | BDF: 0000:23:00.0 | UUID: c4ff73bf-0000-1000-802e-0812b504ed69
                                  all | Selects all devices
  -G, --gpureset           Reset the specified GPU
  -c, --clocks             Reset clocks and overdrive to default
  -f, --fans               Reset fans to automatic (driver) control
  -p, --profile            Reset power profile back to default
  -x, --xgmierr            Reset XGMI error counts
  -d, --perf-determinism   Disable performance determinism
  -C, --compute-partition  Reset compute partitions on the specified GPU
  -M, --memory-partition   Reset memory partitions on the specified GPU
  -o, --power-cap          Reset power capacity limit to max capable

Command Modifiers:
  --json                   Displays output in JSON format (human readable by default).
  --csv                    Displays output in CSV format (human readable by default).
  --file FILE              Saves output into a file on the provided path (stdout by default).
  --loglevel LEVEL         Set the logging level from the possible choices:
                                DEBUG, INFO, WARNING, ERROR, CRITICAL
```

### Example output from amd-smi static

Here is some example output from the tool:

```bash
~$ amd-smi static
GPU: 0
ASIC:
    MARKET_NAME: 0x73bf
    VENDOR_ID: 0x1002
    VENDOR_NAME: Advanced Micro Devices Inc. [AMD/ATI]
    SUBVENDOR_ID: 0
    DEVICE_ID: 0x73bf
    REV_ID: 0xc3
    ASIC_SERIAL: 0xffffffffffffffff
    XGMI_PHYSICAL_ID: N/A
BUS:
    BDF: 0000:23:00.0
    MAX_PCIE_SPEED: 16 GT/s
    MAX_PCIE_LANES: 16
    PCIE_INTERFACE_VERSION: Gen 4
    SLOT_TYPE: PCIE
VBIOS:
    NAME: NAVI21 Gaming XL D41209
    BUILD_DATE: 2020/10/29 13:30
    PART_NUMBER: 113-D4120900-101
    VERSION: 020.001.000.038.015720
BOARD:
    MODEL_NUMBER: N/A
    PRODUCT_SERIAL: 0
    FRU_ID: N/A
    MANUFACTURER_NAME: N/A
    PRODUCT_NAME: N/A
LIMIT:
    MAX_POWER: 203 W
    CURRENT_POWER: 203 W
    SLOWDOWN_EDGE_TEMPERATURE: 100 °C
    SLOWDOWN_HOTSPOT_TEMPERATURE: 110 °C
    SLOWDOWN_VRAM_TEMPERATURE: 100 °C
    SHUTDOWN_EDGE_TEMPERATURE: 105 °C
    SHUTDOWN_HOTSPOT_TEMPERATURE: 115 °C
    SHUTDOWN_VRAM_TEMPERATURE: 105 °C
DRIVER:
    DRIVER_NAME: amdgpu
    DRIVER_VERSION: 6.1.10
    DRIVER_DATE: 2015/01/01 00:00
VRAM:
    VRAM_TYPE: GDDR6
    VRAM_VENDOR: SAMSUNG
    VRAM_SIZE_MB: 16368 MB
CACHE:
    CACHE 0:
        CACHE_SIZE: 16 KB
        CACHE_LEVEL: 1
RAS:
    EEPROM_VERSION: N/A
    PARITY_SCHEMA: N/A
    SINGLE_BIT_SCHEMA: N/A
    DOUBLE_BIT_SCHEMA: N/A
    POISON_SCHEMA: N/A
    ECC_BLOCK_STATE: N/A
PARTITION:
    COMPUTE_PARTITION: N/A
    MEMORY_PARTITION: N/A
NUMA:
    NODE: 0
    AFFINITY: NONE
```

## Disclaimer

The information contained herein is for informational purposes only, and is subject to change without notice. While every precaution has been taken in the preparation of this document, it may contain technical inaccuracies, omissions and typographical errors, and AMD is under no obligation to update or otherwise correct this information. Advanced Micro Devices, Inc. makes no representations or warranties with respect to the accuracy or completeness of the contents of this document, and assumes no liability of any kind, including the implied warranties of noninfringement, merchantability or fitness for particular purposes, with respect to the operation or use of AMD hardware, software or other products described herein.

AMD, the AMD Arrow logo, and combinations thereof are trademarks of Advanced Micro Devices, Inc. Other product names used in this publication are for identification purposes only and may be trademarks of their respective companies.

Copyright (c) 2014-2023 Advanced Micro Devices, Inc. All rights reserved.
