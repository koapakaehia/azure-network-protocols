<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we'll be examining the network activity of Azure Virtual Machines through Wireshark. Additionally, we'll be conducting hands-on experiments with Network Security Groups, all performed on a Mac OS.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04
- Mac Os (user)

<h2>High-Level Steps</h2>

- Create an Azure Resource Group with Windows 10 and Ubuntu Virtual Machines: Deploy both on the same virtual network and subnet for simplified cloud management.
- Connect to the Windows VM with remote desktop (download Microsoft Remote Desktop on Mac) and then download and install Wireshark for analyzing and monitoring networks.
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
* Go back to the home page and "Resource groups" in the top menu. You should see your new resource group listed.

<p>
<img src="https://i.imgur.com/qo6vRqH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<b>CREATING VIRTUAL MACHINES (WINDOWS 10 AND LINUX) ON AZURE.</b>

<b>Step 1: Search for Virtual Machines (VMs)</b>
* Open the Azure portal.
* In the search bar, type "Virtual Machines" and select it.

<p>
<img src="https://i.imgur.com/PWX5X9E.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<b>Step 2: Create Windows 10 VM (VM1)</b>

* Click on "Add" and "Azure Virtual Machine" to start creating a new VM.

<p>
<img src="https://i.imgur.com/5zPNtp8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

* Using the Same Subscription and Resource Group, Name the VM as "VM1."
* Choose the same region as your resource group.
* For Image choose "Windows 10" (operating system.)
* Choose “Standard 2vCPUs”

<p>
<img src="https://i.imgur.com/QGC6l95.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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

<p>
<img src="https://i.imgur.com/LTm342F.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<b>Step 3: Create Linux (Ubuntu) VM (VM2)</b>
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
<img src="https://i.imgur.com/IRVwCs8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

* Now, you should have two VMs ready to establish a connection with each other on our Virtual Network (Vnet).  

<p>
<img src="https://i.imgur.com/mreIS2m.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<b>CONNECTING VMs AND CAPTURING NETWORK PACKETS WITH WIRESHARK</b>

<b>Step 1. Connect to Windows 10 VM (VM1):</b>
* In the Azure portal, click on "Virtual machines."
* Select "VM1" from the list.
* Note down VM1's public IP address.
* For Mac OS, download and install “Microsoft Remote Desktop”
* Next open "Remote Desktop"
* Enter VM1's public IP.
* Click "Connect."

<p>
<img src="https://i.imgur.com/HPoemkd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

* Scroll down to “Use a different account” and hit enter.
* Enter your VM1 username and password when prompted.
* Click on "continue" for the certification.

<p>
<img src="https://i.imgur.com/4hvT67N.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 2. Certification and Access:
* After connecting, allow VM1 to access other PCs on the network if prompted.
* Open a web browser on VM1.

<p>
<img src="https://i.imgur.com/dReuwQV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 3. Download and Install Wireshark:
* Go to google.com.
* In the Google search bar, type "Wireshark" and press Enter.

<p>
<img src="https://i.imgur.com/V9NhoUU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

* Locate the official Wireshark website in the search results.
* Download the Wireshark program for Windows from the official website.
* Install Wireshark on VM1 following the installation prompts.

<p>
<img src="https://i.imgur.com/jyeDgFs.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 4. Open and Start Capturing with Wireshark:
* Launch Wireshark after installation.

<p>
<img src="https://i.imgur.com/Js0xWOX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

* In Wireshark, click on "Ethernet" to start capturing packets and to analyze the captured packets as needed.

<p>
<img src="https://i.imgur.com/CSKjv7f.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<b>NETWORK TESTING WITH WIRESHARK AND NSG: PING AND ICMP TRAFFIC ANALYSIS</b>

<b>Step 1. Ping Linux "VM2" from Windows VM1:</b>
* Open Wireshark on Windows VM1.
* Filter for ICMP traffic.

<p>
<img src="https://i.imgur.com/AHhkDDg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

* Find Linux VM2's private IP.

<p>
<img src="https://i.imgur.com/8OGTso1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

* On Windows VM1 Command Prompt, type ping (Linux VM2's private IP).
* Observe changes in Windows VM1 ICMP traffic.

<b>Step 2. Manage ICMP Traffic with NSG for Ubuntu VM2:</b>
* First do a perpetual ping from VM1 To VM2, using the command prompt on VM1.
* On VM1 Command Prompt, type ping -t (VM2's private IP) for perpetual ping.
* Access NSG settings for Ubuntu VM2.
* Turn off incoming ICMP traffic.
* Observe impact on Windows VM1’s ICMP traffic.
* Re-enable ICMP traffic in NSG for Ubuntu VM2.
* Observe changes in Windows VM1’s ICMP traffic.

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<b>Step 3: Filter DNS (Domain Name System) Traffic:</b>
* In Wireshark, filter for DNS traffic.
* Refresh Wireshark and continue without saving. Now you are filtering by DNS traffic.
* Observe traffic when using nslookup:
* In VM1's Command Prompt, type: nslookup (website)
* Observe DNS Traffic.

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<b>Step 4: Filter RDP (Remote Desktop Protocol) Traffic:</b>
* Refresh Wireshark and continue without saving.
* In Wireshark, filter for RDP traffic or tcp.port == 3389.
* Observe traffic when using RDP between VM1 and VM2.

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<b>Step 5: Cleanup in Azure:</b>
* When finished, delete your Resource Group and Virtual Machines in Microsoft Azure to avoid incurring costs.
* Go to the Azure portal.
* Click on Resource groups and select your created Resource Group.
* Click on Delete resource group.
* Copy and paste the resource group and click delete.
* Repeat steps for any remaining Resource groups in your subscription that may have been automatically created.
* Go back to resource groups and refresh.
* When it says "No resource groups to display," you are done.

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
