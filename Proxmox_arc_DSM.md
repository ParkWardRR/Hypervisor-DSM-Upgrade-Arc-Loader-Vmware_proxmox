# Proxmox-DSM-Upgrade-Arc-Loader (Untested)

This is an adaptation of the ESXI-DSM-Upgrade-Arc-Loader for use on Proxmox. This guide presents rough, untested notes on the sequencing for setting up Arc on a Proxmox setup (replacing ESXi) and transitioning from DSM 6 to DSM 7.

Let us know if you encounter any type of issue, we're here to help.

## Prerequisites

- Make sure you have downloaded the `arc-*.vmdk-dyn.zip` file from the [appropriate source](https://github.com/AuxXxilium/arc/releases).

## Equipment

- Proxmox host
- Lenovo SA120
- HBA - Dell perc h330+ SAS3008

## Instruction Steps

Follow the instructions to set up Arc on Proxmox and upgrade DSM 6 to DSM 7:

1. **Import the .vmdk file**: Import the downloaded `.vmdk` file to your Proxmox host using the Proxmox web GUI.

2. **Shutdown the DSM 6 VM**: Stop the DSM 6 VM to prevent any conflict between the DSM 6 and DSM 7 instances.

3. **Convert the VMDK to a QCOW2 image**: Proxmox uses a different VM image format (QCOW2). Use `qemu-img` to convert the VMDK to a QCOW2 image.
```bash
qemu-img convert -f vmdk -O qcow2 /path_to/arc-dyn.vmdk /destination_path/arc-dyn.qcow2
```

4. **Remove the existing disk from the VM**: Remove the current drive from your VM but ensure you don't delete it from the filesystem.

5. **Attach the `arc-dyn.qcow2` as a new disk**: Using the Proxmox GUI, attach the converted `.qcow2` image to your VM.

6. **Configure the new DSM 7 VM**: Use the same MAC address from the DSM 6 VM to set your NIC on the DSM 7 VM.

7. **Power on the VM**: You can now boot the virtual machine.

8. **Configure Arc's drive mapper**: Set the Arc's drive mapper to "auto" or "active" to optimize the Arc Loader during the DSM 7 installation.

9. **Install DSM 7**: Begin the installation of DSM 7 using the Arc with the previous steps completed.

10. **Shutdown the DSM 7 VM**: Following a successful installation, power off the DSM 7 VM.

11. **Transfer PCIe passthrough**: Transfer the host bus adapter's PCIe passthrough from the DSM 6 to the DSM 7 VM to ensure the correct functioning of devices during the upgrade.

12. **Boot the DSM 7 VM to Arc**: Now, power on the DSM 7 VM and boot to the Arc loader.

13. **Adjust Arc's drive mapper**: Change the Arc's drive mapper to "active" setting to correctly map drive paths in your setup.

14. **Boot DSM 7**: With all configurations completed, power on DSM 7 and ensure its proper functioning.

15. **Restore your storage pools**: Restore the saved data sets and storage structures to ensure that your upgraded system has access to all of its previous data, according to your setup requirements.

If you encounter any issues, feel free to raise a ticket on this repository. We appreciate any feedback and will update the guide accordingly.

---

Xpenology
