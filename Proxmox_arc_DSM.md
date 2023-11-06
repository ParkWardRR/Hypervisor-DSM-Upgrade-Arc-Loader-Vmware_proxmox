# Proxmox DSM Guide

This guide is designed to guide you through the process of installing DSM 7 on the Hypervisor Proxmox and subsequently migrating from DSM 6 to DSM 7. While the guide is untested, it is adapted from the ESXI-DSM-Upgrade-Arc-Loader method. 



## Prerequisites

- It is important to download the `arc-*.vmdk-dyn.zip` file from the [appropriate source](https://github.com/AuxXxilium/arc/releases) before embarking on the process.

## Equipment

- Proxmox host

# SECTION 1: Installing DSM 7 on Proxmox

Here is a step by step guide to help you go through the process:

1. **Import the .vmdk file**: Use the Proxmox web GUI to import the downloaded `.vmdk` file to your Proxmox host.

2. **Convert the VMDK to a QCOW2 image**: Proxmox leverages a different VM image format (QCOW2). Use the `qemu-img` to make a conversion from VMDK to a QCOW2 image. Follow the following script:
```bash
qemu-img convert -f vmdk -O qcow2 /path_to/arc-dyn.vmdk /destination_path/arc-dyn.qcow2
```
3. **Create DSM 7 VM**: Create a new VM on Proxmox for DSM 7.

4. **Attach the `arc-dyn.qcow2` as a new disk**: Use the Proxmox GUI to attach the converted `.qcow2` image to your DSM 7 VM.

5. **Configure the new DSM 7 VM**: Use the same MAC address from the DSM 6 VM to set your NIC on the DSM 7 VM.

6. **Power on the VM**: Boot the VM once you have made all the above steps.

7. **Install DSM 7**: Undertake the installation of DSM 7 using the Arc.

8. **Configure Arc's drive mapper**: Set "auto" or "active" with Arc's drive mapper to optimize the Arc Loader during the DSM 7 installation.

9. **Shutdown the DSM 7 VM**: Power off the DSM 7 VM once your installation is successful.

# SECTION 2: Migration from DSM 6 to DSM 7

Follow the steps below to migrate from DSM 6 to DSM 7 on Proxmox:

1. **Shutdown the DSM 6 VM**: Stop the DSM 6 VM to avoid any potential conflicts between the DSM 6 and DSM 7 instances.

2. **Remove the existing disk from the VM**: Remember not to delete it from the file system when removing the drive from your DSM 6 VM.

3. **Transfer PCIe passthrough**: Move the host bus adapter's PCIe passthrough from the DSM 6 to the DSM 7 VM.

4. **Boot the DSM 7 VM to Arc**: Boot your DSM 7 VM to the Arc loader.

5. **Adjust Arc's drive mapper**: Ensure to shift the Arc's drive mapper to the "active" setting to map drive paths correctly.

6. **Boot DSM 7**: Power on DSM 7 and check if it works perfectly.

7. **Restore your storage pools**: Restore the saved data sets and storage structures to ensure that your upgraded system has access to all of its previous data, according to your setup requirements.


---
Xpenology
