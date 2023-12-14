<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into client-1 with one of the users


<h2>Deployment and Configuration Steps</h2>


Active Directory (AD) is a Microsoft service that provides centralized authentication and authorization to network resources. AD is used in business environments to simplify user management, control access to data and enforce company security policies. A breakdown of how it is used by administrators is the management of thousands of accounts such as accounts, passwords, permissions, application of security policies, all inside a single place. Do you ever wonder how in organizations such as hospitals, schools or libraries, employees were able to use any computer on the premises to login to their own account? This is where AD comes is useful. AD makes use of a computer (server) also known as a domain controller (DC) for all its host computers. The DC is where credentials and resources are stored and security policies are able to be enforced within that domain or collection of clients. 



In the following steps I will go over how to utilize virtualization within Azure to host an on-premises version deployment of AD. 

<h3>Setup Resources in Azure</h3>

The first things I created were two virtual machines (VMs) in my Azure subscription. The first VM (Windows Server 2022) was to act as the domain controller and the second VM (Windows 10 Pro) was used as the client computer. After these VMs are created, Azure creates a subnet and a virtual network automatically among other resources. When configuring a domain controller, it is essential that the IP private address assigned to it is configured to be set to static. In other words, it needed to stay the same address. The client will automatically try to locate the DC using the virtual network DNS server it was assigned to it when it was created. When AD is deployed into the DC, it will also install its own DNS service. I needed to configure which DNS service the client will use to successfully be able to connect to the DC.

To set the network interface card (NIC) IP address to static, I navigated to the VM DC1 and under networking in the left column, I clicked on the name of the network interface link. I chose the IP configuration option on the left side of the screen. I chose the private IP address and set it from "Dynamic" to "static" assignment and saved it.  
 

<h3>Ensure Connectivity between the client and Domain Controller</h3>


<h3>Install Active Directory</h3>


<h3>Create an Admin and Normal User Account in AD</h3>


<h3>Join Client-1 to your domain (mydomain.com)</h3>


<h3>Setup Remote Desktop for non-administrative users on Client-1</h3>


<h3>Create a bunch of additional users and attempt to log into client-1 with one of the use</h3>

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
