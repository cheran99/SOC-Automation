# SOC-Automation

## Objectives
- Create a Wazuh instance
- Integrate SOAR
- Create a fully functional case management system using TheHive

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

![image](https://github.com/user-attachments/assets/deaf07df-7300-45c4-9e02-9b437e00ca00)

The next step is to copy and paste the contents of the ".pub" file. To obtain the contents of the file, run the following command:

cat ~/.ssh/id_rsa.pub

![image](https://github.com/user-attachments/assets/8ebe8e89-feea-4adf-be89-7736afea3498)

This will show the contents which you can then copy and paste into the box under the "Add public SSH key" section as shown below:

![image](https://github.com/user-attachments/assets/bb0e8056-d938-4b0b-a220-01960bead49e)

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

To access the server, you can either use the browser-based terminal found in the Droplet page or you can use SSH from your computer to connect to the virtual machine. This is done using the following command:

ssh root@your-droplet-public-ip

![image](https://github.com/user-attachments/assets/8bf62ba6-f377-407d-9e22-2607a7e7b247)

Once you are connected to your virtual machine, before installing Wazuh, you will need to update and upgrade the system using the following command:

sudo apt-get update && apt-get upgrade -y

Once the system is updated and upgraded, you can now install Wazuh using the curl command from the Wazuh website:

curl -sO https://packages.wazuh.com/4.9/wazuh-install.sh && sudo bash ./wazuh-install.sh -a

Once Wazuh is installed, you will be provided with a username and password that you will use to access the web interface for Wazuh:

![image](https://github.com/user-attachments/assets/b557c5ce-0d0c-4849-b0f9-cd9a4adbe76c)

The Wazuh dashboard IP address is the public IP address of the virtual machine used for Wazuh. The next step is to access the Wazuh dashboard on your web browser using the link:

https://<wazuh-dashboard-ip>:443

This will direct you to the webpage which you can log in with the credentials provided:

![image](https://github.com/user-attachments/assets/4b4307e0-4eaa-4420-9c66-eb95ed2466d4)

Using the credentials provided upon installation, the login was successful:

![image](https://github.com/user-attachments/assets/c1375bbc-ecae-4806-859f-e0cb307507e7)

Wazuh is now up and running.

## Step 4: Install Ubuntu Virtual Machine for TheHive

The next step is to create the Ubuntu 22.04 virtual machine to run TheHive. The minimum specifications required for TheHive are 50GB storage and 8GB RAM. To make the virtual machine, follow the steps shown in step 3 until you reach the point where you have successfully logged in to the terminal to access the virtual machine. You can also name this virtual machine TheHive.

Assign the same firewall that was created earlier in step 3 to this Droplet. This will protect the virtual machine running TheHive.

Once that is done, you can now connect to this virtual machine instance using SSH on your terminal or the browser-based terminal found on this Droplet page.

![image](https://github.com/user-attachments/assets/3f6c5072-5c5a-47a8-83f0-8f137cd99a6b)

As shown above, the virtual machine has successfully been connected. The next step is to update and upgrade the system using the following command:

sudo apt-get update && apt-get upgrade -y

Before installing TheHive, you must install Java, Cassandra, and Elasticsearch.

The prerequisite needs to be installed first before installing Java. This is done using the following command:

apt install wget gnupg apt-transport-https git ca-certificates ca-certificates-java curl  software-properties-common python3-pip lsb-release

Once that is installed, you can install Java using the following commands:

wget -qO- https://apt.corretto.aws/corretto.key | sudo gpg --dearmor  -o /usr/share/keyrings/corretto.gpg

echo "deb [signed-by=/usr/share/keyrings/corretto.gpg] https://apt.corretto.aws stable main" |  sudo tee -a /etc/apt/sources.list.d/corretto.sources.list

sudo apt update

sudo apt install java-common java-11-amazon-corretto-jdk

echo JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto" | sudo tee -a /etc/environment 

export JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto"

Once Java is installed, you can install Cassandra using the following command:

wget -qO -  https://downloads.apache.org/cassandra/KEYS | sudo gpg --dearmor  -o /usr/share/keyrings/cassandra-archive.gpg

echo "deb [signed-by=/usr/share/keyrings/cassandra-archive.gpg] https://debian.cassandra.apache.org 40x main" |  sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list

sudo apt update

sudo apt install cassandra

Once Cassandra is installed, you can then install Elasticsearch using the following command:

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch |  sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg

sudo apt-get install apt-transport-https

echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" |  sudo tee /etc/apt/sources.list.d/elastic-7.x.list

sudo apt update

sudo apt install elasticsearch

Then finally, you can install TheHive using the following command:

wget -O- https://raw.githubusercontent.com/StrangeBeeCorp/Security/main/PGP%20keys/packages.key | sudo gpg --dearmor -o /usr/share/keyrings/strangebee-archive-keyring.gpg

echo 'deb [arch=all signed-by=/usr/share/keyrings/strangebee-archive-keyring.gpg] https://deb.strangebee.com thehive-5.3 main' |sudo tee -a /etc/apt/sources.list.d/strangebee.list

sudo apt-get update

sudo apt-get install -y thehive

TheHive has now been successfully installed on this virtual machine. 

## Step 5: Configure TheHive

The next step is to configure TheHive. To start off, you would need to configure Cassandra first using the following command:

sudo nano /etc/cassandra/cassandra.yaml

This will open up a YAML file:

![image](https://github.com/user-attachments/assets/b2cfc3e8-52b1-4b87-8561-0e2ed1bc9f16)

Set the configurations to the following:
listen_address: <public-ip-for-TheHive-server>
rpc_address: <public-ip-for-TheHive-server> 
seed_provider:
    - class_name: org.apache.cassandra.locator.SimpleSeedProvider
      parameters:          
          # seeds is actually a comma-delimited list of addresses.          
          # Ex: "<ip1>,<ip2>,<ip3>"          
          - seeds: "<public-ip-for-TheHive-server>:7000"

The next is to stop Cassandra, remove the old files, and then restart Cassandra using the following commands:

sudo systemctl stop cassandra.service

sudo rm -rf /var/lib/cassandra/*

sudo systemctl start cassandra.service

To ensure Cassandra is running, use the following command:

sudo systemctl status cassandra.service

![image](https://github.com/user-attachments/assets/76b59ce9-cbd2-4037-afd9-f661173f99fb)

As shown above, Cassandra is active and running.

The next step is to configure Elasticsearch using the following command:

sudo nano /etc/elasticsearch/elasticsearch.yml

Set the configurations and uncomment the following lines:

cluster.name: thehive

node.name: node-1

network.host: <public-ip-for-TheHive-server>

http.port: 9200

cluster.initial_master_nodes: ["node-1"]

Save the Elasticsearch YAML file. Start and enable Elasticsearch using the following commands:

sudo systemctl start elasticsearch

sudo systemctl enable elasticsearch

This will create a symlink.

![image](https://github.com/user-attachments/assets/59afca5c-ebc7-4f26-b400-5fc69c2f7370)

You can check if Elasticsearch is running using the following command:

sudo systemctl status elasticsearch

![image](https://github.com/user-attachments/assets/550ceb31-1b51-4688-b591-4bbffbef7cc5)

Before configuring TheHive configuration file, TheHive user and group need access to a certain file. To check if the user and group have access to a certain file, run the following command:

sudo ls -la /opt/thp

![image](https://github.com/user-attachments/assets/2c876592-15ed-4fcb-8e81-399bf4bc2b5f)

As shown above, the root has access to the TheHive directory. This needs to be changed using the following command:

sudo chown -R thehive:thehive /opt/thp

The chown command is used to change the ownership of the file or directory. In this scenario, the ownership of the TheHive directory is being changed from root to the TheHive user and TheHive group. To check if the change has occurred, run the following command:

sudo ls -la /opt/thp

![image](https://github.com/user-attachments/assets/2cced468-cbb5-421a-abf6-92412ee7fd9a)

The ownership of the TheHive directory has successfully changed to the TheHive user and the TheHive group. Because of this, TheHive configuration file can now be accessed and configured using the following command:
sudo nano /etc/thehive/application.conf

Set the configurations to the following:
db.janusgraph {
  storage {
    backend = cql
    hostname = ["<public-ip-for-TheHive-server>"]
    # Cassandra authentication (if configured)
    # username = "thehive"
    # password = "password"
    cql {
      cluster-name = Test Cluster
      keyspace = thehive
    }
  }
  index.search {
    backend = elasticsearch
    hostname = ["<public-ip-for-TheHive-server>"]
    index-name = thehive
  }
}
application.baseUrl = "http://<public-ip-for-TheHive-server>:9000"

The next step is to start and enable TheHive using the following commands:

sudo systemctl start thehive

sudo systemctl enable thehive

To check if TheHive is running, run the following command:

sudo systemctl status the hive

![image](https://github.com/user-attachments/assets/1b33bf08-6a53-466f-92f9-1caad9f3a786)

Since it is running, you can access TheHive on the web browser using the link "http://<public-ip-for-TheHive-server>:9000":

![image](https://github.com/user-attachments/assets/122edbc6-1d05-43a4-bcc5-da5ff37b1924)

The access to TheHive has been successful. You can log in using the default credentials provided:

Username: admin@thehive.local

Password: secret

This is the result after logging in:

![image](https://github.com/user-attachments/assets/f3ae6053-325a-44be-92ca-26883da7184c)

On a side note, access to the TheHive dashboard with your login credentials depends on the other components running alongside TheHive. Those components are Cassandra and Elasticsearch. If you experience authentication failure up logging in to TheHive using the default credentials, it is because Elasticsearch has stopped running. To rectify this, you would need to restart Elasticsearch. If the restart fails, you may need to reallocate the memory for Java. This can be done using the following command:

sudo nano /etc/elasticsearch/jvm.options.d/jvm.options

This will open up a file where you can add the configurations that instruct Elasticsearch to allocate 2GB of RAM for Java. This would minimise any potential crashes for Elasticsearch.

![image](https://github.com/user-attachments/assets/a049c7df-0496-4d06-9eaa-5f30766f1cff)

Once you have set up the configurations as shown above, you can save that file and restart Cassandra, Elasticsearch, and TheHive. By doing this, you should be able to successfully log on to the TheHive dashboard. This configuration is only essential if Elasticsearch has failed to start otherwise, there is no need to do the configurations. 

## Step 6: Configure Wazuh

The next step is to configure Wazuh. To do this log in to the Wazuh dashboard on your web browser using the link "https://<wazuh-dashboard-ip>:443" and the credentials provided upon installation.

Go to the terminal on the Wazuh server and check if the Wazuh install files exist using the "ls" command. As shown below, it does exist:

![image](https://github.com/user-attachments/assets/b842f3da-108b-43c2-aa24-b90b6d15b56f)

Extract the files from the tar file using the tar command: 

sudo tar -xvf wazuh-install-files.tar

This will create a directory with the install files in it.

![image](https://github.com/user-attachments/assets/60fa2dd3-31df-4bae-bc84-5b6e1f05b1d3)

Change the directory to the Wazuh install files using the following command:

cd ~/wazuh-install-files

The next step is to open the "wazuh-passwords.txt" file to obtain the login credentials for the Wazuh API user account using the cat command:

cat wazuh-passwords.txt

![image](https://github.com/user-attachments/assets/3bf8f7d0-2436-41e8-8297-6eb3bb65ac02)

The login credentials for certain users are found in the text file shown above.

Once you have obtained the API user credentials, go to the Wazuh dashboard, then to "Endpoints Summary" page under the "Server management" section, and click "Deploy new agent":

![image](https://github.com/user-attachments/assets/27a004e6-7d69-43ad-a58a-b37eb7e75bd1)

This new agent is for the Windows 10 virtual machine established in steps 1 and 2. Select Windows as the package to download and install on the virtual machine. For the server address, use the public IP address for the Wazuh server. You can also assign an agent name of your choosing as shown below:

![image](https://github.com/user-attachments/assets/6641ad24-4467-4a7e-b5c2-3f8938a33f49)

You can then run the following commands on Powershell (as administrator) in your Windows virtual machine to install the agent:

Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.9.0-1.msi -OutFile ${env.tmp}\wazuh-agent; msiexec.exe /i ${env.tmp}\wazuh-agent /q WAZUH_MANAGER='64.227.35.242' WAZUH_AGENT_NAME='SOCDemo' 

Once it is installed, the next step is to start the agent using the following command:

NET START WazuhSvc

![image](https://github.com/user-attachments/assets/58654837-aff7-4400-9ab8-16c814ba1007)

The Wazuh agent has successfully started. As shown in the Wazuh dashboard, the agent is active and connected:

![image](https://github.com/user-attachments/assets/34359b59-30a2-4782-ad15-8f13b6740c02)

## Step 7: Generate Telemetry from Windows 10 Virtual Machine

On the Windows 10 virtual machine, go to File Explorer, then to the ":C" drive, then to "Program Files (x86)", then to "ossec-agent", and open "ossec.conf" file with Notepad. This file will contain everything related to Wazuh. Copy and paste the "ossec.conf" file into the same directory to act as a backup file in case there is a mess up in the main file, you can revert the contents of it to the original state. 

Once you are in the "ossec.conf" file on Notepad, scroll to the log analysis section, copy the first local file tag and paste it underneath the original:

![image](https://github.com/user-attachments/assets/68d4a7a5-c2df-4770-94a5-8d73b77cbdf3)

The pasted local file tag should be changed to Sysmon's channel name. You can find the channel name on Event Viewer. Go to Applications and Services > Microsoft > Windows > Sysmon and then right-click Operational and then Properties. 

![image](https://github.com/user-attachments/assets/a42be533-75ab-4dc5-a824-ecf510f75722)

The full name for Sysmon is the channel name. Copy the name, then go back to the configuration file and paste it to the local files tag in the place held by "Application":

![image](https://github.com/user-attachments/assets/67383593-4f53-43ad-9222-b185fb91acec)

You can remove the local file tags for application, security, and system to allow ingestion of logs only coming from Sysmon. Application, security, and system won't be able to forward events to the Wazuh manager. Save the file. Ensure you have administrative access in doing so. 

Go to Services, and restart Wazuh. The restarting Wazuh agent is essential whenever you make any configuration changes.

## Reference
- https://documentation.wazuh.com/current/quickstart.html
- https://docs.strangebee.com/thehive/installation/step-by-step-installation-guide/#configuration_1 













