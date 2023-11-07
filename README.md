# DSM Upgrade Guide for Hypervisors

Welcome to the DSM Upgrade Guide for VMware ESXi and Proxmox hypervisors utilizing Arc Loader.

![Screenshot](https://i.postimg.cc/tg6dJxKT/image.png)

In this repository, we offer guides to assist you with migrating your existing DSM 6 setup to DSM 7 using Arc Loader. These guides facilitate migration in two specific virtualization environments:

- **VMware ESXi Guide:** For users operating within a VMware environment, refer to [`vmware_esxi_arc_DSM.md`](./vmware_esxi_arc_DSM.md).

- **Proxmox VE Guide:** For users working in a Proxmox setup, see [`Proxmox_arc_DSM.md`](./Proxmox_arc_DSM.md). This guide has not been verified yet.

After you have followed the relevant guide above, proceed to the Arc Loader Configuration:

- **Arc Loader Configuration:** Refer to [`arc_loader_config_dsm.md`](./arc_loader_config_dsm.md) for step-by-step guidelines on how to configure Arc Loader. This is required regardless of whether you follow the VMware ESXi or Proxmox VE guide.

## High-Level Overview

The update process involves three main stages:

### Stage 1: Preparation and Launch of Hypervisor VM running DSM 7

1. **Download Arc Loader Files**: Obtain the `arc-*.vmdk-dyn.zip` file from an appropriate source.

2. **Shutdown DSM 6 VM**: Shut down the DSM 6 VM to prevent interruption during the transition.

3. **Import and Convert Arc Loader Image**: Transfer the Arc Loader file to your host and convert the Arc Loader disk image to a suitable format. This process varies depending on your virtualization platform.

4. **Initiate DSM 7 VM**: Apply the configuration and MAC addresses of your previous setup to your DSM 7 VM, and boot up the newly created VM. At this stage, Arc Loader will connect to the internet to retrieve the DSM OS.

### Stage 2: Arc Loader Configuration and Storage Drive Connection

1. **Configure Arc's Drive Mapper**: Adjust Arc's drive mapper to "auto" or "active" for optimal DSM 7 installation.

2. **Initiate DSM 7 Installation**: Carry out the DSM 7 installation using Arc Loader.

3. **Connect Storage Drives**: Attach your storage/data drives or other hardware, such as a RAID/Host Bus Adapter (HBA/DAS) system.

### Stage 3: Migration from DSM 6 to DSM 7

1. **Shutdown DSM 7 VM**: Turn off the DSM 7 VM to ensure conflict-free drive passthrough alteration.

2. **Transfer PCIe Passthrough**: Move the host bus adapter's PCIe passthrough from the DSM 6 VM to the DSM 7 VM.

3. **Boot DSM 7 VM Using Arc**: Power on the DSM 7 VM using Arc Loader.

4. **Verify DSM 7 Setup and Restore Storage Pools**: After modifying the drive settings, start DSM 7, check its operation, and restore your storage pools to access saved datasets and storage structures as per your setup requirements.

---

Enjoy your newly upgraded Xpenology setup!
