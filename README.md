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

In this step, I needed to verify connectivity between both machines by issuing a ping to the DC from CLient-1. I Used Remote Desktop to login into the DC machine and in the search bar I typed in Windows Firewall with advanced security to locate the local Windows firewall to enable ICMP messages. I then signed into Client-1, opened up a Powershell terminal and pinged the DC by its private IP address. 



<h3>Install Active Directory</h3>

In the DC machine I opened up the Server Manager to start the configuration on the server. The first step was to click on the "Add roles and features" and install AD Domain Services. After it is installed, in the top right corner there a yellow notification sign that allowed me to finish setting up the machine as a domain controller. In the deployment window, I chose the "Add a new forest" option and named it "mydomain.com". Once installation of AD DS was complete, it restarrted the machine and instead of using the original credentials I used the first time, I had to use the fully qualified domain name created with the DC.  

<h3>Create an Admin and Normal User Account in AD</h3>

It is bad practice to use the generic account AD DS assigns when it is deployed. It will usually attach the FQDN and the genric username and other acounts to it which is unprofessional and not ideal. I had to create a normal user account by searching for "Active Directory Users and Computers". On the left pane, under mydoamin.com I added two organizational units, "_EMPLOYEES" and "_ADMINS". In the ADMIN folder, I created a new user by right clicking in the open space and added user "Jane Doe" with a user logon name of: jane_admin@mydomain.com. In order to assign this user permissions I right clicked on the name after it was set and chose "porperties". In the "Member of" tab I clicked on "Add" and in the "Enter object names to select" box I typed "domain" and clicked "Check Names" to select the "Domain Admins". After I applied the settings, Jane Doe was part of that Admin group. I then logged off and logged back on as Jane Doe (mydomain.com\jane_admin)  


<h3>Join Client-1 to your domain (mydomain.com)</h3>

The first change I made here was set Client-1's DNS settings to the DC's private IP address so that it will not use the virtual network's DNS server. Within the Client-1 machine I right clicked the start menu and chose "system" > "rename this PC (advanced)" > change the domain. I chose the "domain" option and typed in "mydomain.com". I then navigated to the Azure portal, copied the DC's NIC private address, located the Client-1's network interface link in "Networking", clicked on "DNS settings" and chose the "custom" option to input DC's private IP address. I then refreshed the the portal so that it flushes the DNS cache. After it refreshes, I logged into Client-1 with it's new IP address. I wanted to verify that it was set to the DC's DNS server so I used the command line "ipconfig" to show me the DNS server it was on. Once the verification was complete, and I pressed "OK" on the rename my PC window, the system prompted me to enter a new set of credentials. I entered the administrator Jane Doe (mydomain.com\jane_admin), my password and the machine restarted. Since Client-1 is now part of the domain mydomain.com, it will ask for the new set of credentials when logging in through Remote Desktop to reflect that. 


<h3>Setup Remote Desktop for non-administrative users on Client-1</h3>

In the Client-1 machine, I right clicked the start menu and chose "system" > "remote desktop" > User accounts : "select users that can remotely access this PC". In the next window I clicked on "Add" and instead of adding individual names, it is more efficient to add a group. In this case, I typed in "Domain Users" and applied it. Back in DC (Users & computers > mydomain.com > Users > Domain Users) I saw who was already part of that group. This configuration allowed any non-adminstrative users to login using Client-1.  


<h3>Create a bunch of additional users and attempt to log into client-1 with one of the use</h3>

I logged in to DC as Jane_admin and opened up a Powershell terminal with administrative permissions and opened a new file. For this section of the project I utilized a script to generate random name accounts into the "_EMPLOYEES" folder by pasting the script contents into the new file in Powershell and running it. I then went back to "Users and Computers" and refreshed it so that it would populate the new users created with the script. By using one of these random users I was able to log into Client-1.  

