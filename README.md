<p align="center">
  <img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination">
</p>

# Inspecting Traffic Between Azure Virtual Machines and Network Security Groups (NSGs)

In this comprehensive tutorial, we'll guide you through the process of creating and using Azure Virtual Machines (VMs), monitoring network traffic using Wireshark, and configuring Network Security Groups (NSGs). By the end of this tutorial, you'll have a clear understanding of how to work with Azure VMs and enhance network security. Let's get started!

## Video Demonstration

- [Watch on YouTube: A Basic Tutorial; Azure VMs, Network Traffic & NSGs!](https://youtu.be/UHdRTmpI2mI)

## Environments and Technologies Used

- **Microsoft Azure (Virtual Machines/Compute)**
- **Remote Desktop**
- **Various Command-Line Tools**
- **Various Network Protocols (SSH, RDP, DNS, HTTP/S, ICMP)**
- **Wireshark (Protocol Analyzer)**

## Operating Systems Used

- **Windows 10 (21H2)**
- **Ubuntu Server 20.04**

## High-Level Steps

### Step 1: Create the First Virtual Machine (VM)

Begin by creating the first VM in Azure. The only two selections that should be changed are its "Image" and "Size." Use the following specifications: "Image: Windows 10 (21H2); Size (_recommended_): 2 VCPUs, 16 GiBs of memory." After these specifications are set, create the VM and wait for it to fully deploy.

### Step 2: Create the Second Virtual Machine (VM)

Create the second VM in Azure. Specify the "Image" and "Size" as follows: "Image: Ubuntu Server 20.04 LTS; Size (_recommended_): 2 VCPUs and 16 GiBs of memory." In the "Networking" section, choose the same "Virtual Network" that your first VM is using. The shared "Virtual Network" ensures that your VMs can communicate and share data while simplifying security and network management. After theses specifications have been made, create your second VM and let it deploy.

### Step 3: Connecting to VM & Setting-Up Packet Anazlyzer

We will be using the Remote Desktop Protocol (RDP) to connect directly to our Windows 10 VM. Start an instance of RDP and input your first VM's **Public** IP Address and credentials when prompted.

After successfully connecting, pat yourself on the back and open Microsoft Edge. In the address bar, search for "Wireshark Download." You should find [this page](https://www.wireshark.org/download.html).

Download the "Wireshark x64 Installer," run the executable (.exe) file, and follow the prompts to install Wireshark. Open the application and be ready for the next step.

### Step 4: Observing Network Traffic


<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
