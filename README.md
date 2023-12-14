![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/5eacb21e-6f95-4e22-8a1b-afba0a83f8a7)<p align="center">
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

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/ba89e805-c40b-48d5-b1b5-fc0678633d8f)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/601ec9fb-30c2-43da-9a68-1c35a7cd9e2d)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/c0226eef-6040-4d22-9486-74f6c1723015)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/67d62723-eed8-4070-a864-1875e0139163)





To set the network interface card (NIC) IP address to static, I navigated to the VM DC1 and under networking in the left column, I clicked on the name of the network interface link. I chose the IP configuration option on the left side of the screen. I chose the private IP address and set it from "Dynamic" to "static" assignment and saved it.  

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/963b96d4-9332-46ff-932c-1fad3c6baed8)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/eaf902a8-6252-4f5f-bfa1-c5c347bafcc5)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/e200ac19-61bf-45a2-9153-ad9897505e13)



 

<h3>Ensure Connectivity between the client and Domain Controller</h3>

In this step, I needed to verify connectivity between both machines by issuing a ping to the DC from CLient-1. I Used Remote Desktop to login into the DC machine and in the search bar I typed in Windows Firewall with advanced security to locate the local Windows firewall to enable ICMP messages. I then signed into Client-1, opened up a Powershell terminal and pinged the DC by its private IP address. 

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/3c322ace-0533-4ace-9dc6-86bc63119733)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/014e8a85-ef26-475d-a656-f8a6fe349e9d)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/9e01e567-2f0d-48d8-ac44-2ac125587ab2)


I copied DC's private IP address and I logged in to the DC machine to enable ICMP messages. 

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/a346b888-7568-4457-956d-47b4f626e054)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/1d8f5acd-bde1-4147-8754-16a943e669a8)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/107366f3-ebf0-42a9-a2e1-3697ae9b5f0a)


<h3>Install Active Directory</h3>

In the DC machine I opened up the Server Manager to start the configuration on the server. The first step was to click on the "Add roles and features" and install AD Domain Services. After it is installed, in the top right corner there a yellow notification sign that allowed me to finish setting up the machine as a domain controller. In the deployment window, I chose the "Add a new forest" option and named it "mydomain.com". Once installation of AD DS was complete, it restarrted the machine and instead of using the original credentials I used the first time, I had to use the fully qualified domain name created with the DC.  

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/64404f53-df7a-443a-bda0-35b547527227)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/8fa8409c-4a65-4722-8a21-4f53285ff726)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/409f5328-0a86-4b0d-90f9-1448cf382332)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/bd0bcf0c-4e94-422b-a36f-446babc1f8c2)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/5c38f0d1-1a59-47fb-9579-76a489cb9d15)

It required me to make a password, however, for demonstration purposes I do not need to use it. 

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/154b2f43-732e-4282-a07d-d82b17dbd002)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/8bdc7dcf-de8b-4220-967f-f57c5a2d35bd)

Reconnect to DC using the new credentials.

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/269f130e-f7de-403d-92ab-a22faffeaec1)




<h3>Create an Admin and Normal User Account in AD</h3>

It is bad practice to use the generic account AD DS assigns when it is deployed. It will usually attach the FQDN and the genric username and other acounts to it which is unprofessional and not ideal. I had to create a normal user account by searching for "Active Directory Users and Computers". On the left pane, under mydoamin.com I added two organizational units, "_EMPLOYEES" and "_ADMINS". In the ADMIN folder, I created a new user by right clicking in the open space and added user "Jane Doe" with a user logon name of: jane_admin@mydomain.com. In order to assign this user permissions I right clicked on the name after it was set and chose "porperties". In the "Member of" tab I clicked on "Add" and in the "Enter object names to select" box I typed "domain" and clicked "Check Names" to select the "Domain Admins". After I applied the settings, Jane Doe was part of that Admin group. I then logged off and logged back on as Jane Doe (mydomain.com\jane_admin)  

DC machine:

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/120f5c24-6c62-42b3-be3a-56e58699fe1d)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/5b13c72d-b3b0-476f-91a9-d7237b1f522e)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/dec97ea9-ce47-4640-a855-25262575b99b)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/67da1f4a-fa8b-40d5-8889-48025defd833)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/109bf3d2-09ba-4c1b-b8c8-8d42368714f7)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/39320060-a531-463b-b0ff-2ef5e3403198)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/25fef986-eb6a-4d41-8b05-c7984d463b14)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/10b555e6-06f6-4f0d-8cac-05050faaae27)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/53653591-0105-4cbc-9429-8944a692694c)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/61be061f-99e3-468d-ab10-548b5b2a98b0)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/e3fce4c9-a216-4b0b-a9e8-c1754011855e)



<h3>Join Client-1 to your domain (mydomain.com)</h3>

The first change I made here was set Client-1's DNS settings to the DC's private IP address so that it will not use the virtual network's DNS server. Within the Client-1 machine I right clicked the start menu and chose "system" > "rename this PC (advanced)" > change the domain. I chose the "domain" option and typed in "mydomain.com". I then navigated to the Azure portal, copied the DC's NIC private address, located the Client-1's network interface link in "Networking", clicked on "DNS settings" and chose the "custom" option to input DC's private IP address. I then refreshed the the portal so that it flushes the DNS cache. After it refreshes, I logged into Client-1 with it's new IP address. I wanted to verify that it was set to the DC's DNS server so I used the command line "ipconfig" to show me the DNS server it was on. Once the verification was complete, and I pressed "OK" on the rename my PC window, the system prompted me to enter a new set of credentials. I entered the administrator Jane Doe (mydomain.com\jane_admin), my password and the machine restarted. Since Client-1 is now part of the domain mydomain.com, it will ask for the new set of credentials when logging in through Remote Desktop to reflect that. 

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/baccedc8-c362-4f48-be71-54361b9f9546)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/1e6b7e9f-254f-48bb-9951-6e18e30ca510)

Client-1 machine:

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/d1123c40-11d8-416f-8076-813f1008cd55)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/be15e386-a4f4-48ae-bf5f-5613aab7934c)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/e0a07b04-5cd7-4e19-8d8b-b3f11caa7c70)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/3c93e173-9778-4bb1-93c6-d9a05284ca72)


Copy DC's private IP address.

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/9919e068-11ef-42d9-91cf-f1bf0a34f865)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/306a7a97-a321-421d-87f8-25da5baad7f7)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/705cabc8-6004-44a8-8f25-3e82ccf4f07b)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/9f814a75-a0b1-4211-bed7-8a05b1379565)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/eacc2286-e499-4762-9c58-425d5e893fb3)




<h3>Setup Remote Desktop for non-administrative users on Client-1</h3>

In the Client-1 machine, I right clicked the start menu and chose "system" > "remote desktop" > User accounts : "select users that can remotely access this PC". In the next window I clicked on "Add" and instead of adding individual names, it is more efficient to add a group. In this case, I typed in "Domain Users" and applied it. Back in DC (Users & computers > mydomain.com > Users > Domain Users) I saw who was already part of that group. This configuration allowed any non-adminstrative users to login using Client-1.  

Login to Client-1:

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/5a3f949a-5211-4d35-bd25-981dd7a45815)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/2e864b4d-4807-4da3-9ca3-012803abc7d1)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/abe6e3d9-a75d-44e6-b3ab-7e313113a226)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/736b74cd-6902-4fe7-9884-e880181bf7b2)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/cd83885e-d659-446c-ba5b-09f41c6045d8)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/400b631e-dc3f-4183-a19b-88e147f0e834)




<h3>Create a bunch of additional users and attempt to log into client-1 with one of the use</h3>

I logged in to DC as Jane_admin and opened up a Powershell terminal with administrative permissions and opened a new file. For this section of the project I utilized a script to generate random name accounts into the "_EMPLOYEES" folder by pasting the script contents into the new file in Powershell and running it. I then went back to "Users and Computers" and refreshed it so that it would populate the new users created with the script. By using one of these random users I was able to log into Client-1. Once this is set up I am able to unlock accounts, disable accounts, reset passwords, apply group policies, etc. from within the DC. AD is useful, especially for large companies, for managing resources and accounts. 

DC machine:

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/e6b987d6-a082-446b-aac4-5b862583df26)

Open as administrator.

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/994a29d5-03a2-4eb5-940e-fb8abcb91ffc)

Paste this script into Powershell and run it. 

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/d3e30bfe-7c13-4cf4-8d85-2328aca2fbfb)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/551efb84-e176-4986-be41-fec3e4c6ed32)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/24a69266-11e8-4470-8f64-e1b81ebafe99)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/ca308933-a844-43e1-9537-816f4e761493)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/ad5b8ae6-38b9-4f3e-80cc-c3ae1e1bea36)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/5b1c2d36-ac45-43c3-ae62-1ffe63fe4e9a)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/92cc212f-9bd1-4bed-b49f-25dbb755912a)

![image](https://github.com/jonathansantacruz3/configure-ad/assets/151465848/5cc388cb-9cff-4ab3-9428-3c7763bea5bd)


Thanks for reading! 

