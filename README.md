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
