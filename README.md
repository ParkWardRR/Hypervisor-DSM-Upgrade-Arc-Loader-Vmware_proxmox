# ESXI-DSM-Upgrade-Arc-Loader
Upgrading from DSM 6 to DSM 7 using Arc Loader on an ESXi setup. It contains precise details, from setting up Arc on ESXi to transitioning smoothly from DSM 6 to DSM 7. Please check out the README document for detailed steps and prerequisites

![Screenshot](https://i.postimg.cc/Hn9cbNY8/image.png)

# Upgrading DSM 6 to DSM 7 using Arc Loader on ESXi (Rough Notes)

This GitHub repository presents a rough guide based on personal notes for upgrading DSM 6 to DSM 7 using Arc Loader, specifically for the DS3622xs+ model.

## Prerequisite

- Ensure you've downloaded the `arc-*.vmdk-dyn.zip` file from the [appropriate source](https://github.com/AuxXxilium/arc/releases)

## Parts
- Esxi host
- Lenovo SA120
- HBA - Dell perc h330+ SAS3008

## Instruction Steps
Follow the sequence for Arc setup on ESXi and upgrading from DSM 6 to DSM 7:

1. **Transfer the .vmdk file to your ESXi host**: Use scp (secure copy protocol) to transfer the downloaded file to your ESXi host securely. Replace `<ESXi host IP>` and `datastore-name` with your ESXi host IP and datastore name, respectively.
 ```bash
scp ~/Downloads/arc-23.10.1b.vmdk-dyn.zip root@<ESXi host IP>:/vmfs/volumes/datastore-name/
```

2. **Shutdown the DSM 6 VM**: Ensure you've safely shut down the DSM 6 VM. This prevents conflict or overlap between the DSM 6 and DSM 7 instances.

3. **Unzip the transferred file**: After transferring the file, log into your ESXi host and unzip the file in the appropriate directory.
```bash
unzip /vmfs/volumes/datastore-name/arc-23.10.1b.vmdk-dyn.zip
```

4. **Clone and reformat `.vmdk` file**: Use `vmkfstools` on ESXi host to clone and reformat the `.vmdk` file, streamlining the file structure and storage.
```bash
vmkfstools -i /vmfs/volumes/datastore-name/arc-dyn.vmdk /vmfs/volumes/datastore-name/arc-dyn-clone.vmdk -d thin
```

5. **Remove the existing disk from your VM settings**: Before attaching the cloned `.vmdk` file, detach the existing disk from your virtual machine. Make sure not to delete this disk from the file system.

6. **Add the `arc-dyn-clone.vmdk` as an existing hard disk**: Connect the cloned `.vmdk` to the VM as an existing hard disk. Make sure to attach it to `sata0:0`.

7. **Initiate DSM 7 VM**: Use the MAC addresses from the DSM 6 VM to configure the network interface card (NIC) on your DSM 7 VM.

8. **Power on the VM**: Now you can turn on the virtual machine.

9. **Set Arc's drive mapper**: Adjust Arc's drive mapper to "auto" or "active", optimizing Arc Loader's function during the DSM 7 installation.

10. **Install DSM 7**: With the earlier steps completed, start installing DSM 7 using Arc.

11. **Shutdown DSM 7 VM**: After the successful installation, power off the DSM 7 virtual machine.

12. **Transfer PCIe passthrough**: To maintain device connections and inputs during the upgrade, shift PCIe passthrough of the host bus adapter (HBA) from the DSM 6 VM to the new DSM 7 VM.

13. **Boot the DSM 7 VM to Arc**: Start the DSM 7 VM and boot to Arc loader.

14. **Adjust Arc's drive mapper**: Change Arc's drive mapper to the "active" setting, correctly mapping drive paths in your setup.

15. **Boot DSM 7**: With all configurations set, turn on DSM 7 and ensure it's working as expected.

16. **Restore your storage pools**: Recall the saved data sets and storage structures to ensure your upgraded system has access to prior data per your setup requirements.

---

Feel free to raise an issue in this repository if you need any clarifications.

