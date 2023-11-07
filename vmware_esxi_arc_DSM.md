# ESXI-DSM-Upgrade-Arc-Loader

This guide will walk you through the process of upgrading from DSM 6 to DSM 7 using Arc Loader on a VMware ESXi setup. 

Additional resources: [Arc Loader Wiki](https://github.com/AuxXxilium/AuxXxilium/wiki)

![Screenshot](https://i.postimg.cc/Hn9cbNY8/image.png)

---

## Prerequisites

- Download the `arc-*.vmdk-dyn.zip` file from the [appropriate source](https://github.com/AuxXxilium/arc/releases).

## Equipment

- ESXi host

## SECTION 1: Creating the New Virtual Machine

1. **Create new virtual machine**: Start by creating a new VM with your preferred settings. Ensure you select "Other 5.x Linux (64-bit)" for the operating system.
   
2. **Adjust virtual machine settings**: Remove the existing disk, SCSI controller, and CD drive from the VM. Adjust the CPU, memory, and network settings as needed.

3. **Set boot options**: Modify the boot options to use BIOS. This step might not be necessary if you selected "Other 3.x Linux 64 Bit" while creating the new VM.

## SECTION 2: Configuring and Launching the VMware ESXi VM running DSM 7

After the VM setup, proceed as follows:

1. **Transfer the .vmdk file to your ESXi host**: Transfer the downloaded .vmdk file to your ESXi host using scp (secure copy protocol). Replace `<ESXi host IP>` and `<path to store the file>` with your specific details.
 ```bash
scp ~/Downloads/arc-*.vmdk-dyn.zip root@<ESXi host IP>:/vmfs/volumes/datastore-name/
```
2. **Unzip the transferred file and reformat**: Log into your ESXi host and unzip the file. Then, clone and reformat your .vmdk file using `vmkfstools` for a streamlined file structure. Replace `<path to the .zip file>` with your specified path.
 ```bash
ssh root@<ESXi host IP>
cd <path to the .zip file>
unzip arc-*.vmdk-dyn.zip
vmkfstools -i arc-dyn.vmdk arc-dyn-clone.vmdk -d thin
```
(Optional steps)
 ```bash
rm -rf arc-dyn.vmdk
rm -rf arc-*.vmdk-dyn.zip
```

3. **Attach `arc-dyn-clone.vmdk` as an existing hard disk**: Go back to your ESXi console, edit the new VM, and add the cloned .vmdk file as an existing hard disk. Ensure the "Controller location" is set to SATA (0:0), as it should be the only drive.

4. **Start VM**: Boot up your new DSM 7 VM.

## SECTION 3: Configuring Arc Loader and Connecting Storage Drives

Proceed with the following steps:

1. **Set up Arc's Drive Mapper**: Configure Arc's drive mapper settings to either "auto" or "active" to optimize its operations during the DSM 7 installation ([see Section](./arc_loader_config_dsm.md#drive-mapping)).

2. **Install DSM 7 using Arc**: Begin the DSM 7 installation process using Arc.

3. **Attach storage/data drives**: Connect your storage drives. This procedure varies based on your specific setup.

## SECTION 4: Migration from DSM 6 to DSM 7

After confirming a stable DSM 7 installation and you're ready to attach storage volumes:

1. **Shutdown DSM 7 VM**: Power down your DSM 7 VM to avoid conflicts when transferring PCIe passthrough.

2. **Move PCIe passthrough**: Transfer the host bus adapter's (HBA) PCIe passthrough from the DSM 6 VM to the new DSM 7 VM. This allows you to maintain your device connections during the upgrade process.

3. **Boot DSM 7 VM using Arc**: Restart the new DSM 7 VM using Arc.

4. **Adjust Arc's Drive Mapper**: Modify Arc's drive mapper to "active". This correctly maps your drive paths in your setup ([see Section](./arc_loader_config_dsm.md#drive-mapping)).

5. **Launch DSM 7**: Boot DSM 7, verifying that everything is functioning as intended.

6. **Restore Storage Pools**: Lastly, restore your saved datasets and recreate storage structures to access your previously stored data based on your setup preferences.

---

Enjoy navigating Xpenology! :D
