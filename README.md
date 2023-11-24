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

<p>
<img src="https://i.imgur.com/3PnySam.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<b>CREATING A RESOURCE GROUP IN AZURE</b>

Step 1: Sign in

-Log in to the Azure Portal with your Azure account.

Step 2: Create Resource Group

-In the top menu, click on "Resource groups."

-Click the "+ Create" button.

<p>
<img src="https://i.imgur.com/9HaJh9s.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 3: Basic Details


-Subscription: Choose your Azure subscription.

-Resource group: Enter a unique name for your resource group (e.g., "RG-Lab-02").

-Region: Select the region (location) for your resource group. Remeber your Region because we want to create our additional resource in the same region.

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 4: Review and Create

-Click on the "Review + create" tab.

-Review your settings.

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 5: Create

-Click the "Create" button.

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 6: Wait

-Wait for Azure to create your resource group. You'll see a notification when it's done.

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 7: Confirmation

Go back to "Resource groups" in the left menu. You should see your new resource group listed..
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

