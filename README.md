<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we explore the network traffic to and from Azure Virtual Machines using Wireshark, and conduct practical experiments with Network Security Groups.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create an Azure Resource Group with Windows 10 and Ubuntu Virtual Machines: Deploy both on the same virtual network and subnet for simplified cloud management.
- Connect to the Windows VM with remote desktop, then download and install Wireshark for analyzing and monitoring networks.
- Open Wireshark on your Windows VM, filter for ICMP traffic. Find the private IP of your Linux VM, then ping it from your Windows VM and see the pings in Wireshark.
- Access the Network Security Group (NSG) settings for your Ubuntu VM. Turn off incoming ICMP traffic and see how it affects ICMP traffic on your Windows VM. Later, re-enable ICMP traffic in the same NSG for your Ubuntu VM and observe the changes in ICMP traffic on your Windows VM.
- Use Wireshark to filter and study traffic for SSH, DHCP, DNS, and RDP protocols. Watch how each behaves for detailed insights

<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/CbI0xAI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
<img src="https://i.imgur.com/3PnySam.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<b>CREATING A RESOURCE GROUP IN AZURE</b>

<b>Step 1: Sign in</b>
* Log in to the Azure Portal with your Azure account.

<b>Step 2: Create Resource Group</b>
* In the top menu, click on "Resource groups."
* Click the "+ Create" button.

<p>
<img src="https://i.imgur.com/leKbjJU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<b>Step 3: Basic Details</b>
* Subscription: Choose your Azure subscription.
* Resource group: Enter a unique name for your resource group (e.g., "RG-Lab-02").
* Region: Select the region (location) for your resource group. Remeber your Region because we want to create our additional resource in the same region.

<b>Step 4: Review and Create</b>
* Click on the "Review + create" tab.

<p>
<img src="https://i.imgur.com/0T296py.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<b>Step 5: Create</b>
* Review your settings.
* Click the "Create" button.

<p>
<img src="https://i.imgur.com/62iWiFf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<b>Step 6: Wait</b>
* Wait for Azure to create your resource group. You'll see a notification when it's done.

<p>
<img src="https://i.imgur.com/TCipZEh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<b>Step 7: Confirmation</b>
* Go back to the home page and "Resource groups" in the top menu. You should see your new resource group listed..
</p>
<br />

<p>
<img src="https://i.imgur.com/qo6vRqH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<b>CREATING VIRTUAL MACHINES (WINDOWS 10 AND LINUX) ON AZURE.</b>

<b>Step 1: Search for Virtual Machines (VMs)</b>
* Open the Azure portal.
* In the search bar, type "Virtual Machines" and select it.

</p>
<br />

<p>
<img src="https://i.imgur.com/PWX5X9E.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<b>Step 2: Create Windows 10 VM (VM1)</b>

* Click on "Add" and "Azure Virtual Machine" to start creating a new VM.

</p>
<br />

<p>
<img src="https://i.imgur.com/5zPNtp8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

* Using the Same Subscription and Resource Group, Name the VM as "VM1."
* Choose the same region as your resource group.
* For Image choose "Windows 10" (operating system.)
* Choose “Standard 2vCPUs”

</p>
<br />

<p>
<img src="https://i.imgur.com/dJ4zRyq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

* Set up a username and password for Remote Desktop Protocol (RDP) access.
* Check the licensing box.

<p>
<img src="https://i.imgur.com/IosKl2B.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/F2M3B7r.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

* Navigate to the Networking tab and note the Virtual Network (Vnet) for VM1.
* Select "Review + create"
* and, after successful validation, click on "Create."
</p>
<br />

<p>
<b>Step 3: Create Linux (Ubuntu) VM (VM2)</b>

</p>
<br />

<p>
<img src="https://i.imgur.com/LTm342F.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

* While VM1 is finalizing, go back to the Virtual Machines section.
* Click on "Add" to create another VM.
* Using the Same Subscription and Resource Group, Name the VM as "VM2.”
* Choose the same region as your resource group.
* For Image choose "Ubuntu" (Linux) as the operating system.

<p>
<img src="https://i.imgur.com/3aSpkj0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

* For Size, Choose “Standard 2vCPUs”
* Change “Authentication type” from SSH public key to password.
* Set up a username and password for Remote Desktop Protocol (RDP) access.

<p>
<img src="https://i.imgur.com/ZqugBf0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/Lqy9KoP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

* Navigate to the Networking tab and ensure that VM2 is configured with the same Virtual Network as VM1.
* Select "Review + create" and, after successful validation, click on "Create."
* Wait for VM2 to complete its setup.

<p>
<img src="https://i.imgur.com/ZqugBf0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

* Now, you should have two VMs ready to establish a connection with each other on our Virtual Network (Vnet).  

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
