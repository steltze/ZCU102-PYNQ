This repository contains the installation scripts needed to add PYNQ to the official Ubuntu SDCard Image of your ZCU102, based on the installation for Kria SOMs.

Versions:
- [Ubuntu 22.04 LTS from Canonical]()
- [PYNQ 3.0.1](https://github.com/Xilinx/PYNQ)
- [PYNQ-DPU 2.5](https://github.com/Xilinx/DPU-PYNQ)

A complete Python and Jupyter environment is installed on the ZCU102 development board.

## Installation

#### 1. Get the Ubuntu SD Card Image 

#### Users of Ubuntu 22.04 LTS
If you wish to run Ubuntu 22.04 LTS, you may need to update the boot firmware to the latest version. For example, 2022.1 boot firmware is recommended for Ubuntu 22.04 LTS user. Ubuntu may not boot with mismatched firmware.
The update methods can be found in [Kria Wiki](https://xilinx-wiki.atlassian.net/wiki/spaces/A/pages/1641152513/Kria+K26+SOM#Ubuntu-LTS).

The Ubuntu 22.04 image can be found [here](https://ubuntu.com/download/amd), go to `Zynq UltraScale+ MPSoC Development boards`->`Download 22.04 LTS`.

[Here](https://xilinx-wiki.atlassian.net/wiki/spaces/A/pages/2363129857/Getting+Started+with+Certified+Ubuntu+22.04+LTS+for+Xilinx+Devices) there are instructions for the installation of the image, basically just burn the image into an SD Card, compatible with the ZCU102 Development board.

#### 2. Install PYNQ
- Connect the SD Card to the board
- For the communication between the host and the target board, use either the UART-USB connector and a program like `Minicom` to read from the serial port. Alternatively, connect the board to an Ethernet port and connect to it via *ssh*

#### 3. Install PYNQ
While connected to the ZCU102 board, clone this repository and run the `install.sh` script.

```bash
git clone https://github.com/Xilinx/Kria-PYNQ.git
cd ZCU102-PYNQ/
sudo bash install.sh 
```

This script will install the required debian packages, create Python virtual environment and configure a Jupyter portal.  This process takes around 25 minutes.

The script is a modification of the script for Kria boards, according to [this discussion](https://discuss.pynq.io/t/an-attempt-to-install-pynq-on-ubuntu-for-zcu102/3809) in the PYNQ forum.

#### 4. Open Jupyter

JupyterLab can now be accessed via a web browser `<ip_address>:9090/lab`. The password is **xilinx**

## DPU Overlay 
THe `install.sh` script includes the installation of the DPU Overlay.

Clone the [PYNQ-DPU repo](https://github.com/Xilinx/DPU-PYNQ/tree/master)

Manually install the ZCU102 dpu modules from the links provided in
- pynq_dpu/dpu.bit.link
- pynq_dpu/dpu.hwh.link
- pynq_dpu/dpu.xclbin.link

Search for the `ZCU102` board links and use `wget + link` to download them.

The modules need to be renamed to `dpu.bit, dpu.hwh and dpu.xclbin` and be in the same folder with the notebook or added to the path. 

The DPU Overlay is pre-compiled with a specific configuration found [here](https://github.com/Xilinx/DPU-PYNQ/tree/208a3d5175e19c0d7e2cc342aacb19b36b7738a6/boards/zcu102). If there is a need to change configurations, the DPU has to be re-built on a host machine with Vitis AI 2.5.0

After including the dpu modules in the installation files manually, the tests execute succesfuly with `python3 -m pytest --pyargs pynq_dpu`.
