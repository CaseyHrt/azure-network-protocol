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

## High-Level Steps:

### Step 1: Create the First Virtual Machine (VM)

Begin by creating the first VM in Azure. The only two selections that should be changed are its "Image" and "Size." Use the following specifications: "Image: __Windows 10 (21H2)__; Size (_recommended_): __2 VCPUs, 16 GiBs of memory__." After these specifications are set, create the VM and wait for it to fully deploy.

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/e0bb8d5e-abd5-4baa-b433-3cb75704c939)



### Step 2: Create the Second Virtual Machine (VM)

Create the second VM in Azure. Specify the "Image" and "Size" as follows: "Image: __Ubuntu Server 20.04 LTS__; Size (_recommended_): __2 VCPUs and 16 GiBs of memory__." In the "Networking" section, choose the same "Virtual Network" that your first VM is using. The shared "Virtual Network" ensures that your VMs can communicate and share data while simplifying security and network management. Thus, It is essential the correct Virual Network is chosen. After theses specifications have been made, create your second VM and let it deploy.

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/7d2193d7-7a24-4225-b079-28250b386785)

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/08014e0f-788e-4abf-8eea-3f00d7769faf)



### Step 3: Connecting to VM & Setting-Up Packet Anazlyzer

We will be using the Remote Desktop Protocol (RDP) to connect directly to our Windows 10 VM. Start an instance of RDP and input your first VM's **Public** IP Address and credentials when prompted.

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/02b5da1c-77bc-4211-8d22-cea2fb308d5d)


After successfully connecting, pat yourself on the back and open Microsoft Edge. In the address bar, search for "Wireshark Download." You should find [this page](https://www.wireshark.org/download.html).

Download the "Wireshark x64 Installer," run the executable (.exe) file, and follow the prompts to install Wireshark. Open the application and be ready for the next step.



### Step 4: Observing Network Traffic

#### __ICMP__
Begin by having both Wirsehsark and Powershell open. In Wireshark, you should already see packet transmission occuring; however, let's filter our traffic by ICMP packets. We can see that there is no _current_ ICMP traffic. So let's create some. In Powershell, input "ping" followed by your second VM's private IP. It should resemble something like this __`ping 10.0.0.5`__. If the command has been executed correctly, you will see it pinging the other VM's IP. If you look at Wireshark you should also see ICMP packets. 

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/3cc4289e-8ccc-40a4-befd-853cdd2117cb)



Further Inspection of theses packets shows our VM's communicating. Our first VM (_IP 10.0.0.4_) initaes the communication. Communcation follows with out second VM (_IP 10.0.0.5_) replying back. __TO NOTE:__ ICMP, unlike Transmission Control Protocol (TCP) or User Datagram Protocol (UDP), operates independently of any transport layer protocol. ICMP is a connectionless protocol, which means that devices can send messages without the necessity of establishing a prior connection with the target device. Thus, it is an important tool in troubleshooting 

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/0b0cc70d-db68-4ef1-8b93-68f146a7ccf2)

#### __SSH__ 

What we will observe next is Secure Shell (SSH) traffic. SSH is a method for securely connecting to and controlling remote computers or servers over the internet. It's a critical tool widely used by system administrators, network engineers, and developers for tasks like configuring, troubleshooting, and maintaining systems, especially when dealing with remote machines. So, in Wireshark fitler by SSH (tcp.port == 22) traffic. Then in Powershell type the command: __`ssh (VM2's User)@(VM2's Private IP)`__. To exemplify, say your VM2 User is "admin1" and its private IP is "10.0.0.5", SSH would be initiated using __`ssh admin1@10.0.0.5`__. You will then be prompted to enter the password for your second VM. Once entered correctly you should be welcomed by a host of text in Powershell, and you will have already noticed the SSH traffic in Wireshark.

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/02f65a83-ac55-458f-b4ce-2d7b17fb7b8c)

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/17c43ac0-efcf-478c-a4b7-aab290d0ef3e)

One connected via SSH make sure to try out some Linux commands. Here are a few basic ones to play around with: 
- __`hostname`__ _a command that prints your computer's name_. 
- __`pwd`__ _print the current working directory on your terminal_.
- __`uname`__ _print information about your machine’s kernel, name, and hardware_. 
- __`exit`__ _close the terminal_.

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/6fbefb14-1f81-45d5-a013-0b2aaf0a6585)

Also, don't forget to observe the changes in traffic when using the SSH protocol. It's interesting to see the 'network action' happen with each input. 

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/84f744b5-0ced-40cc-8e30-554d3be827a4)

#### __DHCP__ 

Now for DHCP (Dynamic Host Configuration Protocol) traffic. DHCP is a network protocol used to automatically assign and manage IP addresses and other network configuration parameters to devices in a TCP/IP network. Simply, DHCP automatically assigns IP addresses and network settings to devices, making it easier for them to connect to a network without manual configuration. In WireShark filter by DHCP. You will see that there is not much DHCP traffic happening naturally. However, if we are to input __`ipconfig /renew`__ into Powershell and hit "enter". Some traffic will appear. 

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/c599f9f8-f2ac-420d-aa68-66b8e88fb1f3)

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/3b9c7f01-8c4b-42b4-b35f-e257014fde5b)

This is because of the command we used,__`ipconfig /renew`__. The command: (1) Asks the network's DHCP server for a new IP address (__DHCP Request__). (2) The server provides a new IP address and network details like subnet mask and DNS settings (__DHCP Server Response__). (3) The network settings refresh, ensuring your computer has a working IP address (__Renewal__). 

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/a49f19ac-3b70-4244-8779-c1e9f9e39871)

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/e3c1db62-bd19-4d52-9e4f-c838dcedc9c1)

#### __DNS__ 

DNS (Domain Name System) is like the internet's Yellow Pages (_though still in use_). It's a system that translates human-friendly domain names (like www.google.com) into IP addresses that computers use to locate and connect to websites or other online services. It is the next network protcol we will observe next. So, filter by DNS on Wireshark. You may see that there is DNS traffic already happening, this goes to show how integral and integrated this protocol is to internet activies. Still though, let us observe traffic of our own making. To do so we will use __`nslookup`__, a command-line tool that helps you query DNS (Domain Name System) information. So, in Powershell input __`nslookup www.google.com`__ then hit enter. Powershell will run the command, providing the requested information of the DNS. 

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/84333303-9654-4822-9463-42813dd46363)

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/2db1af9a-a218-4944-b8f0-2c966772475b)

Now if we look at Wireshark, we should also see new DNS traffic. 

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/f7804e0d-98a2-4ebf-a76a-b266d6f89c77)


<h2>Actions and Observations</h2>


<br />
