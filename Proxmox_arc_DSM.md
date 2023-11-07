# Proxmox DSM Guide

In this guide, we walk you through the process of configuring DSM 7 on a Proxmox hypervisor and transitioning from DSM 6 to DSM 7. Although this guide is untested, it borrows from the established ESXI-DSM-Upgrade-Arc-Loader method and adapts it to a Proxmox environment.

Additional resources: [Arc Loader Wiki](https://github.com/AuxXxilium/AuxXxilium/wiki)

![Screenshot](https://i.postimg.cc/B6y2Q23Y/image.png)

---

## Prerequisites

- Ensure you've downloaded `arc-*.vmdk-dyn.zip` from the [correct source](https://github.com/AuxXxilium/arc/releases).

## Equipment

- Proxmox host

## SECTION 1: Creating the New Virtual Machine

1. **Create a new VM in Proxmox**: Use the Proxmox web UI to create a new VM. Provide all the necessary settings, and select 'Linux 5.x - 2.6 Kernel' (64-bit) for the operating system.

2. **Add another drive**: For the newly created VM, add an extra drive that will be used as the DSM OS installation destination.

3. **Choose not to use any media**: When prompted for an OS during the VM creation, select "Do not use any media".

4. **Adjust VM settings**: Proceed to the 'Hardware' tab of your new VM, and remove the existing hard disk and SCSI controller. 

5. **Change boot options**: Navigate to the 'Options' tab and modify the boot order to use BIOS instead of UEFI. 

## SECTION 2: Configuring and Launching the Proxmox VM running DSM 7

After setting up the new VM in Proxmox, follow these steps:

1. **Transfer the .vmdk file to your Proxmox host**: Securely transfer the downloaded .vmdk file to your Proxmox host using scp (secure copy protocol). Replace `<Proxmox host IP>` and `datastore-name` with your specific details.
 ```bash
scp ~/Downloads/arc-*.vmdk-dyn.zip root@<Proxmox host IP>:/vmfs/volumes/<datastore-name>/
```

2. **Unzip the transferred file and convert to qcow2**: After transferring the `.zip` file to your Proxmox host, unzip it and convert the extracted .vmdk file to the qcow2 format, using the following commands:
 ```bash
unzip /vmfs/volumes/<datastore-name>/arc-*.vmdk-dyn.zip
qemu-img convert -f vmdk -O qcow2 arc-dyn.vmdk arc-dyn.qcow2
```
   
3. **Attach the converted .qcow2 file as a hard disk**: In Proxmox's web GUI, attach the converted .qcow2 file as an existing hard disk to the new VM. It should appear as a SATA drive.

4. **Start VM**: Start your new DSM 7 VM. 

## SECTION 3: Configuring Arc Loader and Connecting Storage Drives

After starting your VM:

1. **Model Selection**: Refer to [this section](./arc_loader_config_dsm.md#model-selection) in the Arc Loader configuration guide to choose a model that aligns with your hardware. 

2. **MAC Address and Serial Configuration**: Arrange for the MAC Address and Serial number for the new VM to autopopulate.

3. **Initiate the DSM 7 installation using Arc**: With Arc Loader, start your DSM 7 installation.

4. **Install DSM 7 and validate the installation**: Upon the successful installation of DSM 7, ensure it's functioning smoothly.

5. **Attach storage/data drives**: Connect your storage drives. This step may require you to shut down the VM. Once all drives have been attached, restart the Arc Mapper for an optimal setup configuration.

## SECTION 4: Migration from DSM 6 to DSM 7

In case you're migrating from DSM 6 to DSM 7:

1. **Shutdown DSM 6 VM**: Safely halt the DSM 6 VM to avoid any conflicts that may arise.

2. **Transfer PCIe passthrough**: Transfer the PCIe passthrough of the Host Bus Adapter from DSM 6 to the DSM 7 VM.

3. **Reboot DSM 7 VM using Arc**: Start the DSM 7 VM once again using Arc Loader.

4. **Adjust Arc's Drive Mapper**: Modify Arc's Drive Mapper to "active" to accurately map the drive paths in your setup.  ([see Section](./arc_loader_config_dsm.md#drive-mapping)).

5. **Start DSM 7**: Run DSM 7 and ensure it's functioning as expected.

6. **Restore Storage Pools**: Finally, recover your stored datasets and recreate your storage structures to regain access to your previous data based on your setup preferences.

---

Enjoy your Xpenology!
