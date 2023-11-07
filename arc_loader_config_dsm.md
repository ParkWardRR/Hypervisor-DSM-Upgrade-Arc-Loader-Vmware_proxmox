# Arc Loader Configuration Guide for DSM

This guide walks you through the process of configuring Arc Loader, which is the bootloader that facilitates booting and installing Synology DSM.

---

## Model Selection

Once you've booted into Arc Loader, you are ready to configure your Xpenology NAS. 

1. Start by selecting the Synology NAS model you wish to emulate using the model configurator. For rack-mounted setups, RS3621xs+ or RS4021xs+ are commonly used choices.

![Model Configurator](https://i.postimg.cc/7hGHgZ7H/Screenshot-2023-11-07-at-12-26-31-PM.png)

For more information, visit the Arc guide's [model selector section](https://github.com/AuxXxilium/AuxXxilium/wiki/Arc:-Choose-a-Model).

---

## MAC Address and Serial Configuration

Next, set your MAC Address and Serial number:

1. If you're migrating, input your existing MAC Address and Serial number.

2. If you're setting up a new configuration, you can auto-generate these values.

![MAC Address & Serial Configuration](https://i.postimg.cc/B6wZLKj7/Screenshot-2023-11-07-at-12-26-59-PM.png)

Note:

- The MAC Address spoofed in Arc Loader will override the ESXi NIC's MAC Address.
- Configuring the MAC Address and Serial number is crucial if you intend to use DSM cloud services.

---

## Drive Mapping

In this step, we use Arc Loader to map the drives.

1. You may either select "Use SataPortMap (controller active Ports)" to set mappings for only those controller ports that are active.

2. Alternatively, you can choose "Use SataPortMap (controller max Ports)" to map all the controller ports, whether active or not.

![Drive Mapping](https://i.postimg.cc/L6P45h2v/Screenshot-2023-11-07-at-12-27-22-PM.png)

- The "Use SataRemap (remove blank drives)" option can be beneficial when dealing with empty or dummy Ports from your Controller. 
  * Please note that when using this option, all drives must be mapped to the SATA controller because SAS/SCSI Ports will not work.
- You might need to consider setting this up after installing Arc and adding your storage drives.

---

## DSM Extensions

You can choose the DSM extensions you want to enable for your setup. Below is an example of a commonly used configuration:

![DSM Extensions](https://i.postimg.cc/7ZyH2DL4/Screenshot-2023-11-07-at-12-27-58-PM.png)

---

## Ready, Set, Go 

After you've made all of the necessary configurations:

1. Build the Arc Loader.

2. Boot up your setup!

![Build Arc](https://i.postimg.cc/5yhxpWqw/Screenshot-2023-11-07-at-12-28-04-PM.png)

