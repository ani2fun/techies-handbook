# RedHat (RHEL-9.2) VM Installation on MacOS M1 (aarch64) using UTM

This article provides a step-by-step guide to installing Red Hat Enterprise Linux (RHEL) version 9.2 on a virtual machine (VM) running on a macOS M1 (aarch64) system using the UTM application. The UTM application allows you to create and manage virtual machines on your macOS system.

1. The article assumes that UTM is already installed on your macOS M1 system. If you don’t have UTM installed, you can download it from the official website (https://mac.getutm.app/).
2. Before proceeding with the RHEL installation, you need to download the RHEL 9.2 DVD ISO file for aarch64 architecture. You can download the ISO file from the Red Hat Developer website (https://developers.redhat.com/products/rhel/download). Please note that you may need to sign up on the website to access the download. ![rhel-1](/resources/other/rhel-d1.png)
   
3. Once you have UTM installed and the RHEL 9.2 ISO file downloaded, you can follow the steps below to create the VM and install RHEL 9.2:
   1. Launch the UTM application and click on “Create new VM.”
   2. Select “Virtualize” and choose the “Linux” option.
   3. Tick the checkbox that says “Use Apple Virtualization” and click on “Continue.”
   4. Click on the “Browse” button under Boot ISO Image and select the downloaded RHEL 9.2 ISO file.Then click on “Continue.”
   5. Allocate memory (e.g., 8192 MB or 8 GB) and CPU (e.g., 8 cores) resources for the VM. Click on “Continue.”
   6. Allocate storage (e.g., 64 GB) for the VM. Click on “Continue.”
   7. (Optional) Assign a shared directory path for the VM. Click on “Continue.”
   8. In the “Summary” window, you can change the default VM name from “Linux” to “rhel9.2” or any other desired name. Click “Save” to create the VM.

4. At this point, you have successfully created the VM. However, before proceeding with the installation, there are a few additional settings to adjust.

5. Click on the “3 dots” icon in the UTM application to access the settings for the VM you just created.
![rhel-2](/resources/other/rhel-d2.webp).
6. (Optional)In the Network settings, change the configuration to “Bridge” instead of “Shared” to make the VM appear on your home network environment. Set the interface to “en0.”
   1. Set the interface to: `en0`
   2. ![rhel-3](/resources/other/rhel-d3.png)

7. IMPORTANT STEP: 
Currently due to a bug in the UTM application, the created VM may fail to start. This is because of the ordering of the ISO image and VM image. Refer to the provided image in the article. Ensure that your newly created VM appears before the “rhel9.2-aarch64-dvd.iso” image in the list.
![rhel-4](/resources/other/rhel-d4.png)
   1. If the order is incorrect, delete the “rhel9.2-aarch64-dvd.iso” image and re-add it. To re-add, click on “New” and locate the ISO file in its original location.
   2. For Re adding just click on new and add it wherever it was located before.
   3. The correct order should be as shown in the provided image.
   4. ![rhel-5](/resources/other/rhel-d5.png)


8. Launch your VM, and the installation of RHEL 9.2 will begin.
9. Follow the regular steps for installing Red Hat Linux, as you would with any new installation.
   
By following these steps, you will be able to successfully install Red Hat Enterprise Linux version 9.2 on a virtual machine running on your macOS M1 (aarch64) system using the UTM application.

The system configuration used for testing in this article includes the following:
```
MacBook Pro
Chip: Apple M1 Pro
Memory: 32 GB
macOS version: 13.3.1 (a)
```


### Some commands to test after successful installation:

#### dmidecode
```console
sudo dmidecode|more
```
output:
```
# dmidecode 3.3
Getting SMBIOS data from sysfs.
SMBIOS 3.3.0 present.
Table at 0x26FDC3000.

Handle 0x0000, DMI type 1, 27 bytes
System Information
	Manufacturer: Apple Inc.
	Product Name: Apple Virtualization Generic Platform
	Version: 1
	Serial Number: Virtualization-db087d0b-d366-40bc-809b-969690dcbe1e
	UUID: 0b7d08db-66d3-bc40-809b-969690dcbe1e
	Wake-up Type: Power Switch
	SKU Number: Not Specified
	Family: Not Specified
...
...
...

```

#### uname -a

```
[aniket@m1 ~]$ uname -a
Linux m1 5.14.0-284.11.1.el9_2.aarch64 #1 SMP PREEMPT_DYNAMIC Wed Apr 12 11:23:11 EDT 2023 aarch64 aarch64 aarch64 GNU/Linux
```

#### lsblk
```
[aniket@m1 ~]$ lsblk
NAME             MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
vda              252:0    0   64G  0 disk 
├─vda1           252:1    0  600M  0 part /boot/efi
├─vda2           252:2    0    1G  0 part /boot
└─vda3           252:3    0 62.4G  0 part 
  ├─rhel_m1-root 253:0    0 37.6G  0 lvm  /
  ├─rhel_m1-swap 253:1    0  6.4G  0 lvm  [SWAP]
  └─rhel_m1-home 253:2    0 18.4G  0 lvm  /home
vdb              252:16   0  7.4G  0 disk 
```

References:

BLOG Video post: 
https://developers.redhat.com/articles/2022/10/21/rhel-9-and-single-node-openshift-vms-macos-ventura#running_single_node_openshift_on_apple_silicon