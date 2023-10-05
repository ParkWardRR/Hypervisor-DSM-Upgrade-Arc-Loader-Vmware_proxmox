# ESXI-DSM-Upgrade-Arc-Loader
Guidance on upgrading DSM 6 to DSM 7 using Arc Loader on an ESXi setup. Contains precise details right from setting up Arc on ESXi to transitioning smoothly from DSM 6 to DSM 7. Please check out the README document for detailed steps and prerequisites


# Upgrading DSM 6 to DSM 7 using Arc Loader on ESXi (Rough Notes)

This GitHub repository presents a rough guide based on personal notes for upgrading DSM 6 to DSM 7 using Arc Loader, specifically for the DS3622xs+ model.

## Prerequisite

- Ensure you've downloaded the `arc-23.10.1b.vmdk-dyn.zip` file from the appropriate source.

## Instruction Steps
Follow the sequence for Arc setup on ESXi and upgrading from DSM 6 to DSM 7:

1. **Transfer the .vmdk file to your ESXi host**: Use scp (secure copy protocol) to securely transfer the downloaded file to your ESXi Host. Replace `<ESXi host IP>` and `datastore-name` with your actual ESXi host IP and datastore name respectively. Example:
 ```bash
scp ~/Downloads/arc-23.10.1b.vmdk-dyn.zip root@<ESXi host IP>:/vmfs/volumes/datastore-name/
```

2. **Unzip the transferred file**: Once the file is transferred, connect to your ESXi host and unzip the file in the appropriate directory. 
```bash
unzip /vmfs/volumes/datastore-name/arc-23.10.1b.vmdk-dyn.zip
```

3. **Clone and reformat `.vmdk` file**: Utilize `vmkfstools` on ESXi host to clone and reformat the `.vmdk` file. This step removes any unnecessary bloat and ensures an optimal file format. 
```bash
vmkfstools -i /vmfs/volumes/datastore-name/arc-dyn.vmdk /vmfs/volumes/datastore-name/arc-dyn-clone.vmdk -d thin
```

4. **Remove the existing disk from your VM settings**: This is a preparatory step for attaching the cloned `.vmdk` file. Make sure not to delete it from the filesystem.

5. **Add the `arc-dyn-clone.vmdk` as an existing hard disk**: Link this cloned `.vmdk` to the VM as an existing hard disk. Take note to attach it to `sata0:0`.

6. **Initiate DSM 7 VM**: Use the MAC addresses from the DSM 6 VM to start initiating DSM 7 VM. This allows a smoother transition between the DSM versions.

7. **Power on the VM**: At this point, you can turn on the virtual machine.

8. **Shutdown the DSM 6 VM**: Ensure you've safely shut down the DSM 6 VM. This avoids any conflict or overlap between the two DSM versions in operation.

9. **Set Arc's drive mapper**: Change the setting of Arc's drive mapper to auto. This setting assists in the optimal utilisation of Arc Loader during DSM 7's installation.

10. **Install DSM 7**: With the earlier steps completed, now initiate the actual installation of DSM 7.

11. **Shutdown DSM 7 VM**: Ensure to power off the DSM 7 virtual machine after the installation process.

12. **Transfer PCIe passthrough**: This step is important for maintaining device connections and inputs during the upgrade. Shift PCIe passthrough of HBA from the DSM 6 VM to newly installed DSM 7 VM.

13. **Boot the DSM 7 VM to Arc**: Start the DSM 7 VM on Arc.

14. **Adjust Arc's drive mapper**: Switch Arc's drive mapper to the "active" setting. This is necessary to correctly map drive paths in your setup.

15. **Boot DSM 7**: Finally, turn on DSM 7 and confirm everything is functioning as expected.

16. **Restore your storage pools**: This is to bring-back any data sets or storage structures. It’s always important to restore storage pools to ensure your system has all the data you had before.

---

If any clarifications are required, feel free to raise an issue in this repository.
