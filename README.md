<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: Create Resources in Azure - Domain Controller VM and Client VM 
- Step 2: Ensure Connectivity between the Client and Domain Controller
- Step 3: Install Active Directory (AD)
- Step 4: Create an Admin and Normal User Account in AD
- Step 5: Join the Client VM to the Domain Controller VM
- Step 6: Setup Remote Desktop for non-administrative users on the Client VM
- Step 7: Create additional users and log in to the Client VM with one of the users

<h2>Deployment and Configuration Steps</h2><br>

<h3>Step 1: Create Resources in Azure - Domain Controller VM and Client VM</h3><br>
<p>
<img src="https://i.imgur.com/FaIVHWj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In this lab, we will create two VMs in the same VNET. One will be a Domain Controller (DC-1), and the other will be a Client VM (Client-1). We will set the Domain Controller's NIC Private IP Address to be static because it is providing Active Directory services to the Client VM. We will place both VMs in the same Vnet.
</p>
<br />

<h3>Step 2: Ensure Connectivity between the Client and Domain Controller</h3><br>
<p>
<img src="https://i.imgur.com/45A21Ub.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
<img src="https://i.imgur.com/CeExnCC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
To ensure connectivity between the two VMs, first we will log in to DC-1 and enable ICMPv4 on the local windows firewall. Then we will ping DC-1's private IP address from Client-1 and assess that the ping is successful.
</p>
<br />

<h3>Step 3: Install Active Directory (AD)</h3><br>
<p>
<img src="https://i.imgur.com/TvX0sSb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On DC-1 we will install Active Directory Domain Services and promote it to a domain controller by setting up a new forest and naming it "mydomain.com." Then restart the VM and log in to DC-1 as "mydomain.com\trust" (note that "trust" can be substituted with any username assigned to DC-1).
</p>
<br />

<h3>Step 4: Create an Admin and Normal User Account in AD</h3><br>
<p>
<img src="https://i.imgur.com/QHdvVq3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To create Admin and Normal User accounts within Active Directory, we will open Active Directory Users and Computers (ADUC) from the Windows Start menu . Within ADUC, we will create Organizational Units (OU), which are the user accounts. The OU for Admins will be named "_ADMINS;" the OU for Normal Users will be named, "_EMPLOYEES." To create the OU for Normal Users, right click on mydomain.com, click new, then click "Organizational Unit," and name it "_EMPLOYEES." To create the OU for Admins, right click on mydomain.com, click new, then click "Organizational Unit," and name it "_ADMINS." Then we will create a new employee, named "Jane Doe," and assign her the username, "jane_admin." Jane is an Admin, so we will add her to the "Domain Admins" Security Group. Henceforth, we will use jane_admin as the admin account.
</p>
<br />

<h3>Step 5: Join the Client VM to the Domain Controller VM</h3><br>
<p>
<img src="https://i.imgur.com/M75vue2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
From the Azure Portal, we will change Client-1's DNS settings to DC-1's Private IP address. Once you do that, restart Client-1 from within the Azure portal. The picture below shows verification that client-1 is using DC-1's DNS. Next, log in to Client -1 VM as the original local admin, using the username mydomain.com\please (note that "please" can be substituted with any user name you choose for Client-1). Then right click on the Windows Start icon, navigate to Systems, click "Rename this PC (advanced)," click "Change," click "Domain" and name it "mydomain.com." Enter the credentials for jane_admin.  The computer will restart and Client-1 will now be a part of mydomain.com. 
</p>
<br />

<h3>Step 6: Setup Remote Desktop for non-administrative users on the Client VM</h3><br>
<p>
<img src="https://i.imgur.com/KtDr8a0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log in to Client-1 as an admin (we will use mydomain.com\jane_admin). Open system poperties. Click "Remote Desktop," allow "domain users" access to remote desktop. You will now be able to log in to Client-1 as a normal, non-administrative user (this would normally be done with Group Policy, which allows you to change many systems at once).
</p>
<br />

<h3>Step 7: Create additional users and log in to the Client VM with one of the users</h3><br>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Last, to verify that normal users can log in to Client-1, we will use a script to generate thousands of users. Next, log in to DC-1 as jane_admin. Open PowerShell_ise as an administrator and input the script into it. We will select one of the users from the script and log in to Client-1 with his credentials, as a normal user.
</p>
<br />

