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


<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/d22FHIm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In this lab, two VMs will be created in the same VNET. One will function as a domain controller, and the other as a client computer. The DC will get a static IP because it is providing Active Directory services to the client PC. There will be a domin join between the two virtual machines. The client machine will be using the domain controller as it's DNS server.
</p>
<br />

<p>
<img src=https://i.imgur.com/Lxyh7zJ.png height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a domain controller and a client virtual machine. Set the domain's controller NIC private IP address to be static.
</p>
<br />

<p>
<img src=https://i.imgur.com/Ja25H4x.png height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We must ensure connectivity between domain controller and Client virtual machine. Login into Domain controller VM and enable ICMPv4 in local firewall settings. 
</p>
<br />

<p>
<img src=https://i.imgur.com/jrBEiBn.png height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Reconnect into Clinet VM,open Powershell. Ping -t your domain controller private ip address to see if there is a successful connection.
</p>
<br /v

<p>
<img src= https://i.imgur.com/pUy7x8L.png height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Connect to DC VM and Install Active Directory. Server Manager should be in your background, if not click start menu, tap server manager under windows server. Add roles and features -> install active dicerectory domain services -> install.
</p>
<br /v

<p>
<img src= https://i.imgur.com/OgHZjTd.png height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Promote server as a domain controller -> add a new forest-> name domain mydomain.com.
</p>
<br /v

<p>
<img src= https://i.imgur.com/2sM3XZG.png height="80%" width="80%" alt="Disk Sanitization Steps"/> 
</p>
<p>
Restart Domain controller VM. We are going to create an Admin and normal user account in AD. To open up active directory, select active directory users and computers under tools in service manager. Next we will create a couple of organizational units, right click mydomain.com or whatever name you chose. Select new then organizational units, name them _EMPLOYEES and _ADMINS. 
</p>
<br /v

<p>
<img src=https://i.imgur.com/k1WSfoN.png height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  right click inside of your _employees OU and select new user and fill out the proper information for your new user. The user name for this practice will be jane Doe, she is going to be an Admin.
</p>
<br /v

<p>
<img src= https://i.imgur.com/lyUgXpH.png height="80%" width="80%" alt="Disk Sanitization Steps"/> 
</p>
<p>
Add jane_admin to the "domain admins" Secruity group. right click the active directory account jane admin, then select properties -> member of -> add -> type domain then check names-> select domain admin-> apply-> ok. 
</p>
<br /v

<p>
<img src= https://i.imgur.com/U6kCVIE.png height="80%" width="80%" alt="Disk Sanitization Steps"/> 
</p>
<p>
From now on, we will be able to log in as an admin as jane_admin.
</p>
<br /v

<p>
<img src= https://i.imgur.com/Soa2gnx.png height="80%" width="80%" alt="Disk Sanitization Steps"/>  
</p>
<p>
From the Azure portal, we will alter Client-1's DNS settings to the DC's Private IP address. From the Azure portal, restart Client VM.
</p>
<br /v

<p>
<img src= https://i.imgur.com/mmHExB5.png height="80%" width="80%" alt="Disk Sanitization Steps"/>   
</p>
<p>
Login into Client Vm as the original admin which is labuser. go into command prompt or powershell to check for hostname, whoami, and also for your new client dns server setting.
</p>
<br /v

<p>
<img src= https://i.imgur.com/BawSOlb.png height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src= https://i.imgur.com/EWGEPeU.png height="80%" width="80%" alt="Disk Sanitization Steps"/>  
</p> 
<p>
We are going to join the client vm to the domain (mydomain.com). Right click start menu -> system -> rename pc -> change-> type mydomain.com in member of-> ok -> set computer name/ domain name and password to the admin info you set earlier. In my case my user name is mydomain.com\jane_admin.
</p>
<br /v

<p>
<img src= https://i.imgur.com/t412yeG.png height="80%" width="80%" alt="Disk Sanitization Steps"/>  
</p>
<p>
Next we will setup non admin users to log into Client VM. You should be logged into client Vm already, right click the start menu and go to system-> remote desktop-> under user accounts select users  that can remotely access this PC-> tap add-> type domain users-> tap check names-> tap okay. 
</p>
<br /v

<p>
<img src= https://i.imgur.com/3zmxHCq.png height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
next we will be creating a bunch of additional users and attepmt to login as one. First we are going to open up powershell.ISE but make sure to run it as an administrator by right clicking.   
</p>
<br /v

<p>
<img src= https://i.imgur.com/30jWE6T.png height="80%" width="80%" alt="Disk Sanitization Steps"/> 
</p>
<p>
To generate thousands of users for the domain, we will utilize a script to input into powershell.ISE by creating a new file and pasting the contents. After pasting the contents, run the script and observe the creation of thousands of users. Next we will choose one user and log into Client-1.
</p>
<br /v

<p>
<img src= https://i.imgur.com/CQX5OsK.png height="80%" width="80%" alt="Disk Sanitization Steps"/>   
</p>
<p>
To see all the users, open active directory and check under _employees. I will be choosing a random user by the name of tal.sun to log into client Vm.
</p>
<br /v

<p>
<img src= https://i.imgur.com/1Gmg8R5.png height="80%" width="80%" alt="Disk Sanitization Steps"/>      
  
  
