# RedHat (rhel9.2)  installation on MacOS M1 (aarch64)

1. Install UTM : https://mac.getutm.app/
   
2. Download dvd iso aarch64 - https://developers.redhat.com/products/rhel/download.
![rhel-1](/resources/other/rhel-d1.png)
(You probably need to sign up to download.)

3. Steps to create VM:
   1. Click on Create new VM
   2. Select Virtualize.
   3. Select Linux.
   4. Tick check-box "Use Apple Virtualization". Then Click on "Continue"
   5. Click on Button "Browse" under Boot ISO Image. Then Click on "Continue"
   6. Select the downloaded ISO image file. Then Click on "Continue"
   7. Allocate memory 8192 (e.g. 8gb) and CPU (e.g. 8). Then Click on "Continue"
   8. Allocate memory "Storage"=(e.g. 64gb). Then Click on "Continue".
   9. (Optional) Assign shared directory path. Then Click on "Continue".
   10. You will see "Summary" window. You may change the name from "Linux" to "rhel9.2" or whatever.
   11. Click "Save".

4. At this point you have created the VM successfuly. But before continue to do the installation change few settings.

5. Click on the "3 dots" for settings of UTM to get details about the VM you have just created.

6. (Optional) Go to Network change config. Change network to Bridge instead of shared. this will make it appear on your Home network environment.
   1. Set the interface to: `en0`
   2. ![rhel-3](/resources/other/rhel-d3.png)

7. IMPORTANT STEP: 
   1. There is a bug in the "UTM" application. Hence the created VM might not start. It is because of ordering iso-dvd-image and vm-image.
   2. Look at the following image. Your newly creatd VM must appear before the "rhel9.2-aarch64-dvd.iso" image.
   3. ![rhel-4](/resources/other/rhel-d4.png)
   4. If this is not the case then delete the "rhel9.2-aarch64-dvd.iso" image and Re-Add it.
   5. For Re adding just click on new and add it wherever it was located before. 
   6. Correct order is like in following image:
   7. ![rhel-5](/resources/other/rhel-d5.png)


8. Launch your VM and the installation of RHEL9.2 will begin.
   
9. Follow the steps as you create any new Redhat Linux installation.
