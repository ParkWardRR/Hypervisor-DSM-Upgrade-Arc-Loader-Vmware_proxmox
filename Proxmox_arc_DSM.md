# Proxmox DSM Guide

This guide assists you in installing DSM 7 on the Proxmox hypervisor and transitioning from DSM 6 to DSM 7 on Proxmox. While this guide is untested, it is adapted from the ESXI-DSM-Upgrade-Arc-Loader method. This guide also aids in transitioning from DSM 6 to DSM 7, and sets up DSM 7 on a Proxmox hypervisor using Arc Loader. Although it's an unverified guide, it's adapted from the ESXI-DSM-Upgrade-Arc-Loader method specifically for Proxmox. 

Additional resources: [Arc Loader Wiki](https://github.com/AuxXxilium/AuxXxilium/wiki)

---

[![image.png](https://i.postimg.cc/B6y2Q23Y/image.png)](https://postimg.cc/ftcV8tJm)

---

## Prerequisites

- Prior to the start, download `arc-*.vmdk-dyn.zip` from the [appropriate source](https://github.com/AuxXxilium/arc/releases).

## Equipments

- Proxmox host

# SECTION 1: Configuring and Launching the Proxmox VM running DSM 7

Here are the steps to set up your Proxmox host:

1. **Transfer the .vmdk file**: Use `scp` (Secure Copy Protocol) to transfer the `.vmdk` file (downloaded as per prerequisites) to your Proxmox host. Replace `<Proxmox host IP>` and `destination-directory` with your actual host IP and desired directory path.
 ```bash
scp ~/Downloads/arc-*.vmdk-dyn.zip root@<Proxmox host IP>:/destination-directory/
```

2. **Convert the vmdk to a qcow2 image**: Proxmox uses qcow2 VM images, so the vmdk file must be converted. The conversion can be done using `qemu-img`, like so:
 ```bash
qemu-img convert -f vmdk -O qcow2 /path_to/arc-dyn.vmdk /destination_path/arc-dyn.qcow2
```

3. **Create and configure a new VM**: Use Proxmox's web GUI to create a new VM. Add another drive during the VM creation process, which will later serve as the DSM's OS installation location. When asked for the OS, choose "Do not use any media".

4. **Attach the converted drive**: Remove the existing SCSI0 disk from the VM's 'Disks' tab. Then, append the `.qcow2` image (created in the second step) as a new SATA drive. 
  
5. **Modify the boot order**: Within the 'Options' section, adjust 'Boot Order' to disable everything (via Edit) except the SATA0. Confirm with 'OK'. 

6. **Initiate the VM**: Launch the newly created VM.

# SECTION 2: Configuring Arc Loader and Connecting Storage Drives

After successfully initiating your VM:

1. **Optimize Arc's Drive Mapper**: In the drive mapper settings of Arc, select either "auto" or "active". This ensures Arc Loader's optimal functionality during the DSM 7 installation.

2. **Select a compatible model number**: Choose a model number based on your Arc guide. If, for example, you need a rack-mounted DAS aesthetic, the RS4021xs+ might suffice.

3. **Autogenerate MAC and Serial Number**: Opt for the auto-generation functionality for both the MAC and Serial Number.

4. **Install DSM 7 using Arc**: Begin the DSM 7 installation using Arc.

5. **Validate the DSM 7 installation**: Ensure the DSM 7 has been properly installed and is running smoothly.

6. **Hook up NAS storage drives or HBA/DAS**: Upon validating DSM 7 installation, link your NAS storage drives and/or any Hardware-Based RAID/Host Bus Adapter systems. This process might require shutting down the VM. Once completed, rerun the Arc mapper for optimal setup configuration.

# SECTION 3: Migration from DSM 6 to DSM 7

For those transitioning from DSM 6 to DSM 7, the following steps are crucial:

1. **Shutdown DSM 6 VM**: Halt the running DSM 6 VM to avoid conflicts or overlaps.

2. **Transfer PCIe passthrough**: Transfer PCIe passthrough of the Host Bus Adapter from DSM 6 to the installed DSM 7 VM.

3. **Reboot DSM 7 VM via Arc**: By using the Arc loader, start and boot the DSM 7 VM.

4. **Alter Arc's drive mapper setting**: Modify the drive mapper setting of Arc to "active" to match the current setup. 

5. **Launch DSM 7**: After all configurations are set, start up DSM 7 and confirm its proper functioning.

6. **Restore storage pools**: Recover any saved datasets and storage structures in order to provide previous data access based on your setup needs.

---

Happy Upgrading! :D
---
Xpenology
