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
### Contents
- [Virtual Machine Creation](https://github.com/CaseyHrt/azure-network-protocol#step-1-create-the-virtual-machines-vm)
- [Connecting to VM & Setting-Up Packet Anazlyzer](https://github.com/CaseyHrt/azure-network-protocol#step-2-connecting-to-vm--setting-up-packet-anazlyzer)
- [Observing Network Traffic](https://github.com/CaseyHrt/azure-network-protocol#step-3-observing-network-traffic)
- [Network Security Groups & Implementaion](https://github.com/CaseyHrt/azure-network-protocol#step-4-network-security-groups--implementaion)

### Step 1: Create the Virtual Machines (VM)

Begin by creating the first VM in Azure. The only two selections that should be changed are its "Image" and "Size." Use the following specifications: "Image: __Windows 10 (21H2)__; Size (_recommended_): __2 VCPUs, 16 GiBs of memory__." After these specifications are set, create the VM and wait for it to fully deploy.

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/e0bb8d5e-abd5-4baa-b433-3cb75704c939)

Create the second VM in Azure. Specify the "Image" and "Size" as follows: "Image: __Ubuntu Server 20.04 LTS__; Size (_recommended_): __2 VCPUs and 16 GiBs of memory__." In the "Networking" section, choose the same "Virtual Network" that your first VM is using. The shared "Virtual Network" ensures that your VMs can communicate and share data while simplifying security and network management. Thus, It is essential the correct Virual Network is chosen. After theses specifications have been made, create your second VM and let it deploy.

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/7d2193d7-7a24-4225-b079-28250b386785)

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/08014e0f-788e-4abf-8eea-3f00d7769faf)



### Step 2: Connecting to VM & Setting-Up Packet Anazlyzer

We will be using the Remote Desktop Protocol (RDP) to connect directly to our Windows 10 VM. Start an instance of RDP and input your first VM's **Public** IP Address and credentials when prompted.

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/02b5da1c-77bc-4211-8d22-cea2fb308d5d)


After successfully connecting, pat yourself on the back and open Microsoft Edge. In the address bar, search for "Wireshark Download." You should find [this page](https://www.wireshark.org/download.html).

Download the "Wireshark x64 Installer," run the executable (.exe) file, and follow the prompts to install Wireshark. Open the application and be ready for the next step.



### Step 3: Observing Network Traffic

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

Its worth noting that while the __`nslookup`__ command-tool has its uses, you might find the __`Get-DnsClient`__ and __`Resolve-DnsName`__ command-tools more useful in Powershell. 

#### __RDP__

RDP (Remote Desktop Protocol) is a technology that enables users to access and control a remote computer or server over a network, allowing them to interact with the remote system as if they were physically present. It's a propitary protocol, developed by Microsoft, and is currently how we are accessing our VM's in this tutorial! To view RDP traffic, filter Wireshark by "RDP" or "tcp.port == 53". Once you have filtered by RDP, you should see a continious flow of traffic. This is (_again_) due to us using RDP to connect to our virtual machine. 

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/c4bfd6b2-bc85-41f6-ae90-69f3f91ab632)

### Step 4: Network Security Groups & Implementaion

Network Security Groups (NSGs) are a fundamental component of network security in Microsoft Azure. They act as a built-in, distributed firewall that allows you to control traffic to and from Azure resources such as virtual machines (VMs), virtual networks, and subnets. Azure hosts many features for these NSGs, like; monitoring and logging, stateful inspection, rule-based operation, and so on. For this tutorial we will: (1) create traffic on our network using ICMP, then (2) implement an imbound rule that prohibits ICMP requests on our second VM, and (3) observe the rule's effect on the network in real time.

To begin, fliter by ICMP traffic in Wireshark and have an instance of Powershell running. Once you've filtered the traffic correctly, use the `ping` command-line again, referencing your second VM (_[Click Here for Assistance with "Ping"](https://github.com/CaseyHrt/azure-network-protocol/blob/main/README.md#icmp)_). This is to confirm that we can (_still_) communicate between our VMs. If the `ping` is successful, you should also see ICMP traffic in Wireshark, and we can then move on to further steps. 

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/afdae094-9086-4174-bca6-bdc056f5c639)

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/3145e08a-aedf-4fbb-a36a-1d853dc0ce67)

Undertanding that we can still communicate on our network, we're going to setup a perpetual `ping` from VM1 to VM2. This is what the command-line should look like in Powershell: `ping 10.0.0.5 -t` (__REMEMBER__, to input your second VM's private IP). Once inputted, you should see your VM1 sending constant 'ping requests' to VM2. This will be refelcted in both Powershell and Wireshark. 

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/e75bc9f4-2449-4d93-915c-b9586cd14950)

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/daec121d-9ae7-478b-ba65-928eb510a74a)

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/d8799306-4b3a-417d-95ab-6934b5b14ffd)

Once the perpetual ping has been setup, go into your [Azure Portal](https://portal.azure.com/?quickstart=true#home). Here simply type "Network Security Groups" in the search bar, and click it in the box that appears under the "Services" tab. Once there, select your second VM. Within your VM2's "Network Security Groups", click "Inboung Security Rules under the "Settings" tab on the left. 

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/2937b7c3-435d-4455-be1c-ac0811fdb27d)

![image](https://github.com/CaseyHrt/azure-network-protocol/assets/146404028/c7e8e70d-6ab7-4e14-a056-6ce8df02e51f)



<h2>Actions and Observations</h2>


<br />
