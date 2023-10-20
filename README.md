<p align="center">
  <img src="https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/193b8047-5d68-41c8-bb29-5a8238481e28" alt="Traffic Examination">

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

Begin by creating the first VM in Azure. The only two selections that should be changed are its "Image" and "Size." Use the following specifications: "Image: __Windows 10 (21H2)__; Size (_recommended_): __2 VCPUs, 16 GiBs of memory__." After these specifications are set, create the VM and wait for it to fully deploy.

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/e0bb8d5e-abd5-4baa-b433-3cb75704c939)



### Step 2: Create the Second Virtual Machine (VM)

Create the second VM in Azure. Specify the "Image" and "Size" as follows: "Image: __Ubuntu Server 20.04 LTS__; Size (_recommended_): __2 VCPUs and 16 GiBs of memory__." In the "Networking" section, choose the same "Virtual Network" that your first VM is using. The shared "Virtual Network" ensures that your VMs can communicate and share data while simplifying security and network management. Thus, It is essential the correct Virual Network is chosen. After theses specifications have been made, create your second VM and let it deploy.

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/7d2193d7-7a24-4225-b079-28250b386785)

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/dfc18540-0710-4707-baf7-9034c5dcf82d)






### Step 3: Connecting to VM & Setting-Up Packet Anazlyzer

We will be using the Remote Desktop Protocol (RDP) to connect directly to our Windows 10 VM. Start an instance of RDP and input your first VM's **Public** IP Address and credentials when prompted.

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/02b5da1c-77bc-4211-8d22-cea2fb308d5d)


After successfully connecting, pat yourself on the back and open Microsoft Edge. In the address bar, search for "Wireshark Download." You should find [this page](https://www.wireshark.org/download.html).

Download the "Wireshark x64 Installer," run the executable (.exe) file, and follow the prompts to install Wireshark. Open the application and be ready for the next step.



### Step 4: Observing Network Traffic

#### __ICMP__
Begin by having both Wirsehsark and Powershell open. In Wireshark, you should already see packet transmission occuring; however, let's filter our traffic by ICMP packets. We can see that there is no _current_ ICMP traffic. So let's create some. In Powershell, input "ping" followed by your second VM's private IP. It should resemble something like this `ping 10.0.0.5`. If the command has been executed correctly, you will see it pinging the other VM's IP. If you look at Wireshark you should also see ICMP packets. 

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/3cc4289e-8ccc-40a4-befd-853cdd2117cb)


![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/19686d97-9a7b-4e0d-9cab-54c2b6c5edf0)

Further Inspection of theses packets shows our VM's communicating. Our first VM (_IP 10.0.0.4_) initaes the communication. Communcation follows with out second VM (_IP 10.0.0.5_) replying back. __TO NOTE:__ ICMP, unlike Transmission Control Protocol (TCP) or User Datagram Protocol (UDP), operates independently of any transport layer protocol. ICMP is a connectionless protocol, which means that devices can send messages without the necessity of establishing a prior connection with the target device. Thus, it is an important tool in troubleshooting 

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/0b0cc70d-db68-4ef1-8b93-68f146a7ccf2)

#### __SSH__ 

What we will observe next is Secure Shell (SSH) 










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
