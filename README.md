# DSM Upgrade Guide for Hypervisors

Welcome to the DSM Upgrade Guide for VMware ESXi and Proxmox hypervisors using Arc Loader.

![Screenshot](https://i.postimg.cc/tg6dJxKT/image.png)

In this repository, we provide detailed guides for migrating your existing DSM 6 setup to DSM 7 using Arc Loader. The guides are tailored specifically for two virtualization environments:

- **VMware ESXi users:** For detailed migration steps in a VMware environment, please refer to the [`vmware_esxi_arc_DSM.md`](./vmware_esxi_arc_DSM.md) file.

- **Proxmox VE users:** Check out the [`Proxmox_arc_DSM.md`](./Proxmox_arc_DSM.md) file for guidance tailored to a Proxmox setup. Please note that this guide is unverified.

## High-Level Overview

The update process is divided into three significant stages:

### Stage 1: Preparing and Launching the Hypervisor VM running DSM 7

1. **Download Arc Loader Files**: Download the `arc-*.vmdk-dyn.zip` file from an appropriate source. 

2. **Shutdown DSM 6 VM**: Ensure the DSM 6 VM is powered down for a seamless transition.

3. **Import and Convert the Arc Loader Image**: Depending on the virtualization platform, safely transfer the Arc Loader file to your host and convert the Arc Loader disk image to a suitable format. 

4. **Initiate DSM 7 VM**: Apply the previous setup's configurations and MAC addresses to your DSM 7 VM, then start the newly created VM. Here, the Arc Loader will connect to the internet to fetch the DSM OS. 

### Stage 2: Setting Up Arc Loader and Connecting Storage Drives

1. **Optimize Arc's Drive Mapper**: Set Arc's drive mapper to "auto" or "active" for an optimal DSM 7 installation process.

2. **Start DSM 7 Installation**: Launch the DSM 7 installation using Arc Loader.

3. **Attach Storage Drives**: Connect your storage/data drives or other pieces of hardware, such as RAID/Host Bus Adapter (HBA/DAS) system for storing data.

### Stage 3: Migrating from DSM 6 to DSM 7

1. **Shutdown DSM 7 VM**: To prevent conflicts and safely switch the drive passthrough, power down the DSM 7 VM.

2. **Move PCIe Passthrough**: Shift the host bus adapter's PCIe passthrough from DSM 6 VM to DSM 7 VM.

3. **Boot DSM 7 VM Using Arc**: Power on the DSM 7 VM and boot using Arc Loader.

4. **Confirm DSM 7 Setup and Restore Storage Pools**: After adjusting the drive settings, start DSM 7, verify its functionality, and restore your storage pools to retrieve saved datasets and storage structures per your setup preferences.

---

Should you encounter any concerns or require clarification, don't hesitate to raise an issue in this repository. 
   
---

Xpenology
