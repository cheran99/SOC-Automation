# SOC-Automation

## Objectives
- Create a Wazuh instance
- Integrate SOAR
- Create a fully functional case management using TheHive

## Set Up
![Screenshot 2024-09-24 205038](https://github.com/user-attachments/assets/194ad8d0-fbf2-4d1f-94e4-667a058c30c0)
![Screenshot 2024-09-24 205141](https://github.com/user-attachments/assets/0a12dedb-578b-4a34-86d4-2138a520ba6e)

The virtual machines used in this project are the following:
- Windows 10 with Sysmon installed (Client machine)
- Ubuntu 22.04 (Wazuh and TheHive)

## Methodology 

### Step 1: Install Windows 10 Virtual Machine

The first step is downloading a virtual machine platform such as VirtualBox or VMWare. In this project, VirtualBox is used as a platform.

Once VirtualBox is downloaded, the next step is to download the ISO image for Windows 10. To do this, download the ISO file from the official Microsoft website.

![Screenshot 2024-09-25 123258](https://github.com/user-attachments/assets/bd168c33-7214-409c-b5cc-6ff89c15436c)

As shown above, click "Download Now" under "Create Windows 10 installation media". Once it is downloaded, click on the downloaded file where you are required to agree to the license agreement and you will be presented with two options: "Upgrade this PC now" and "Create installation media (USB flash drive, DVD, or ISO file) for another PC". Select the second option which is to create installation media for another PC. 

You will be asked which media to choose whether it is a USB flash drive or ISO file. Since Windows is being installed on the VM, choose ISO file. You can save the ISO file in your preferred directory on your PC. 

The next step is to go to VirtualBox, click "New". Select the name for your virtual machine and the directory where you want to save your virtual machine files. For the ISO Image, select "Other", and go to the directory where you saved the Windows ISO file and choose that ISO file as the image:

![Screenshot 2024-09-25 125506](https://github.com/user-attachments/assets/916d7f0f-ac0c-4697-be9f-b0460b7c7b8f)

As shown above, click skip unattended installation so that you can set up the virtual machine manually. 

For the system's settings, allocate 4GB of RAM and 2 CPUs to the Windows virtual machine. The reason for this allocation is that the performance of the virtual machine is dependent on the PC that runs it. In other words, the virtual machine rely of the PC's specifications. 

Once the mandatory settings and configuration are done, you can finish the setup and run the virtual machine. 

To run the virtual machine, click start. As it is running, you will need to complete the Windows 10 setup. It will ask you for a product key but if you don't have one, you can select "I don't have a product key". You will then be asked which Windows 10 operating system you want to install, select Windows 10 Pro. Accept the terms and conditions, then choose "Custom: Install Windows only (advanced)" as the installation type. Select the driver you want to install the Windows on and click next. This will install Windows 10 as a virtual machine.

### Step 2: Install Sysmon on Windows 10 VM

The next step is to download Sysmon on the Windows 10 virtual machine. To do this, go to the Sysmon page on the Microsoft website and click "Download Sysmon". Once it is downloaded, extract the zip file. However, don't click on anything yet.

To obtain the configuraiton file for Sysmon, go to this page https://github.com/olafhartong/sysmon-modular/tree/master
Click the sysmonconfig.xml file at the bottom of the repository. Once you are on the page, click raw at the top right corner. On the raw page, right-click and save as. Ensure that the config file is on the same directory as the extracted Symon files. 

The next is to open Powershell and run as administrator. Once you are on Powershell, change the directory to the same directory as the extracted Sysmon files using the following command:
cd "C:\Users\chera\Downloads\Sysmon"

![Screenshot 2024-09-25 142043](https://github.com/user-attachments/assets/795717b2-1343-443c-b88f-76e4ba01b47b)

Type in "dir" on the Powershell and you can see which files are in directory:

![Screenshot 2024-09-25 142139](https://github.com/user-attachments/assets/28ac742a-5ee4-4035-b23e-52f606846959)

To check how to install Sysmon on the virtual machine, type ".\Sysmon64.exe" on Powershell and that will show you how to install it:

![image](https://github.com/user-attachments/assets/e80e771c-0e53-494c-bfad-bd5e920434fd)

The next step is to install Sysmon (64 bit) using the following command:
.\Sysmon64.exe -i "C:\Users\chera\Downloads\Sysmon\sysmonconfig.xml"

![image](https://github.com/user-attachments/assets/0c6a7919-22d0-4b75-bf59-e9409daa9cde)

To check if Sysmon is installed on the mvirtual machine, go to start, search for "Services", open the application, and scroll to see if Sysmon is installed. 

![image](https://github.com/user-attachments/assets/99dff074-4b12-44c7-a24e-310a29b2788d)

As shown above, Sysmon is successfully installed. 

## Step 3: Install Ubuntu Virtual Machine for Wazuh

To create the Ubuntu 22.04 virtual machine instance to run Wazuh, a cloud virtual machine will be used for this project. The cloud provider that will be used is Digital Ocean. Using a compatible instance to run Wazuh is costly but if you are within your budget, it is best to use a referral link to create a Digital Ocean account and earn $200 free credits which are valid for 60 days. 

Once you have set up your Digital Ocean account, to create a virtual machine, click "Droplets2 under the create section as shown below:

![image](https://github.com/user-attachments/assets/03e55eb0-e75a-41e3-a120-79ffaf03705a)

The next step is to choose the region in which you are based:

![image](https://github.com/user-attachments/assets/c2e35c4e-9b3c-4e8c-bf10-6f79eb6eda69)

The next step is to select Ubuntu as the operating system and 22.04 as the version:

![image](https://github.com/user-attachments/assets/5dd46e98-2ba3-400e-8300-e2a1c71366f1)

The next step is to choose "Basic" as the droplet type and then "Premium Intel" as the CPU option. To ensure that Wazuh runs smoothly on the Ubuntu VM, it would require at least 8GB RAM and 50GB storage, so select the plan that meets this requirement:

![image](https://github.com/user-attachments/assets/075eed26-3403-4d7b-86e2-b75e8cb9596f)

For the authentication method, you can choose either an SSH key or a password. For this project, an SSH key is chosen because they are more secure and less vulnerable to brute-force attacks. Once you have chosen SSH key as the authentication method, the next step is to set up the SSH key. To do this, you would need to create a public SSH key first. This is done by opening the terminal and running the following command:
ssh-keygen

Enter the name you want the file to be saved as. You will be prompted to enter a passphrase. You can press enter if you don't want to put a passphrase. this will generate two files which are by default called "id_rsa" and "id_rsa.pub".

![image](https://github.com/user-attachments/assets/e7672455-f2e7-4492-bf5d-1cb40bd5b90a)

The next step is to copy and paste the contents of the ".pub" file. To otain the contents of the file, run the following command:
cat ~/.ssh/id_rsa.pub

![image](https://github.com/user-attachments/assets/8ebe8e89-feea-4adf-be89-7736afea3498)

This will show the contents which you can then copy and paste in the box under the "Add public SSH key" section as shown below:

![image](https://github.com/user-attachments/assets/d67124a1-a801-4f4e-9ea8-6e489b138b51)

This will create a new SSH key which you can use to connect to the Ubuntu virtual machine.

In the "Finalize Details2 section, you can change the hostname of this virtual machine as you wish. Since this virtual machine is used for Wazuh, the hostname will be Wazuh. 

![image](https://github.com/user-attachments/assets/425dc1c5-8111-4416-a7f6-2e8709533fed)

Once this is done, you can create the droplet. The droplet will take a while to be ready.  

To further secure this virtual machine, the next step is to create a firewall. To do this go to "Networking", then to the "Firewall" section, and then click "Create Firewall".

Once you are on that page, choose a name for that firewall. Under the inbound rules section, choose "All TCP" as the rule type, and for sources, you can put the public IP address for the device that your are running the VM machine on. To find your public IP address, go to https://www.whatismyip.com/ and copy the public IP address.

![image](https://github.com/user-attachments/assets/a7e19e3f-50a2-4a70-a4a1-3659ba21ff11)

Once that is done, you can create the firewall.

The next step is to go to "Droplets" and to the virtual machine that you just created. Go to the "Networking" section, then scroll all the way down to the "Firewalls". Click edit and then click the firewall you just created. Go to the "Droplets" section, click "Add Droplets", and then add the Wazuh virtual machine. 

![image](https://github.com/user-attachments/assets/44ac850c-0d23-4579-914f-1ff3f0ab6a1d)

The firewall can now protect the virtual machine. 



## Reference














