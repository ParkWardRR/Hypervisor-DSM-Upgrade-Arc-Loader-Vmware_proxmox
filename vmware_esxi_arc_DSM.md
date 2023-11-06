# ESXI-DSM-Upgrade-Arc-Loader

This guide will walk you through the process of upgrading from DSM 6 to DSM 7 using Arc Loader on a VMware ESXi setup. Adapted from the Proxmox setup, this guide remains focused on the specifics of the ESXi host environment.

Additional resources: [Arc Loader Wiki](https://github.com/AuxXxilium/AuxXxilium/wiki)
---

![Screenshot](https://i.postimg.cc/Hn9cbNY8/image.png)

---

## Prerequisites

- Make sure to download the `arc-*.vmdk-dyn.zip` file from the [appropriate source](https://github.com/AuxXxilium/arc/releases).

## Equipment

- ESXi host

## SECTION 1: Configuring and Launching the VMware ESXi VM running DSM 7

Do the prep work:

1. **Transfer the .vmdk file to your ESXi host**: Using scp (secure copy protocol), transfer the downloaded .vmdk file to your ESXi host in a secure manner. Replace `<ESXi host IP>` and `datastore-name` with your specific values.
 ```bash
scp ~/Downloads/arc-*.vmdk-dyn.zip root@<ESXi host IP>:/vmfs/volumes/datastore-name/
```

2. **Shutdown the DSM 6 VM** (If needed) : Make sure to safely shutdown the DSM 6 VM to avoid any conflict with the DSM 7 instance. 

3. **Unzip the transferred file**: After moving the `.zip` file to your ESXi host, unzip it in the appropriate directory with:
```bash
unzip /vmfs/volumes/datastore-name/arc-*.vmdk-dyn.zip
```

4. **Clone and reformat .vmdk file**: Clone and reformat your .vmdk file using `vmkfstools` for a more streamlined file structure.
```bash
vmkfstools -i arc-dyn.vmdk arc-dyn-clone.vmdk -d thin
# or 
vmkfstools -i /vmfs/volumes/datastore-name/arc-dyn.vmdk /vmfs/volumes/datastore-name/arc-dyn-clone.vmdk -d thin
```

5. **Remove the existing disk from your VM settings**: Before attaching the cloned .vmdk file, detach the existing disk from your VM. Make sure not to delete this disk from the file system.

6. **Attach `arc-dyn-clone.vmdk` as an existing hard disk**: Connect the cloned .vmdk to the VM as an existing hard disk.

7. **Apply the DSM 6 MAC addresses**: Apply the MAC addresses from the DSM 6 VM to DSM 7 VM Network Interface Card (NIC) configurations.

8. **Start VM**: Boot up your new DSM 7 VM.

## SECTION 2: Configuring Arc Loader and Connecting Storage Drives

The next steps are:

1. **Set up Arc's Drive Mapper**: Update the drive mapper settings for Arc loader to either "auto" or "active" to optimize its operations during the DSM 7 installation.

2. **Install DSM 7 using Arc**: Start the DSM 7 installation process using Arc, ensuring your previous configurations have been successfully set up before proceeding.

3. **Attach storage/data drives**: Hook up your storage drives. I do this by attaching the HBA via PCIe passthrough, which connects the DAS containing all my disks. Keep in mind your storage drive attachment process might differ significantly.

## SECTION 3: Migration from DSM 6 to DSM 7

After your DSM 7 has been successfully installed and all storage devices have been connected:

1. **Shutdown DSM 7 VM**: Securely power off your DSM 7 VM to avoid conflicts during PCIe passthrough transfer.

2. **Move PCIe passthrough**: Transfer the host bus adapter's (HBA) PCIe passthrough from the DSM 6 VM to the new DSM 7 VM. In this way, you can maintain your device connections and inputs during the upgrade process.

3. **Reboot DSM 7 VM using Arc**: Power on the new DSM 7 VM, ensuring to boot it with the Arc loader.

4. **Adjust Arc's drive mapper**: Modify Arc's drive mapper to "active", enabling the correct mapping of your drive paths in your setup.

5. **Launch DSM 7**: Boot DSM 7, confirming everything is working as intended.

6. **Restore Storage Pools**: Finally, retrieve your saved data sets and storage structures, granting access to previous data according to your setup requirements.

---

Happy Xpenology! :D

