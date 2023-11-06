# README.md

Welcome to the Hypervisor DSM Upgrade Arc Loader guide for VMware and Proxmox. 

![Screenshot](https://i.postimg.cc/Hn9cbNY8/image.png)


This repository contains walkthroughs for migrating from DSM 6 to DSM 7 using Arc Loader, specialized for both VMware ESXi and Proxmox VE:

- **For VMware ESXi users**: Find the guide in the [`vmware_esxi_arc_DSM.md`](./vmware_esxi_arc_DSM.md) file.

- **For Proxmox VE users**: We offer an untested guide that's available in the [`Proxmox_arc_DSM.md`](./Proxmox_arc_DSM.md) file.

Please select the guide that matches your host platform. 

## High-Level Guide
The process can be divided into two major stages:

### Stage 1: Running DSM 7 on the Hypervisor
1. Download the necessary Arc Loader files.
2. Safely shut down the DSM 6 VM to prevent any conflicts ahead of the upgrade.
3. Import (or transfer) and convert the Arc Loader image file as needed for your platform.
4. Disconnect the current drive from your VM but do not delete it from your filesystem.
5. Attach the new, converted Arc Loader image to your VM.
6. Set up the DSM 7 VM using the same MAC address from the DSM 6 VM for your NIC.
7. Power on your VM.
8. Set Arc's drive mapper to "auto" or "active", optimizing Arc Loader's function during the DSM 7 installation.
9. Begin installing DSM 7.

### Stage 2: Migrating from DSM 6
1. After successful installation, power down the DSM 7 VM.
2. Transfer the PCIe passthrough from the DSM 6 VM to the new DSM 7 VM.
3. Start the DSM 7 VM, booting to the Arc loader.
4. Adjust Arc's drive mapper to ensure the correct mapping of drive paths in your setup.
5. Power on DSM 7, ensuring its proper functioning.
6. Restore your storage pools to ensure your upgraded system accesses all previous data as per your setup requirements.

If you encounter any issues or ambiguities along the way, feel free to raise a ticket in this repository.


---

Xpenology
