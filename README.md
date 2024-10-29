<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
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
</p>
<p>
We will create two virtual machines (VMs) within the same virtual network (VNET): one will serve as a Domain Controller (DC), while the other will function as a Client machine. We will assign a static IP address to the Domain Controller since it provides Active Directory services to the Client machine. The Client machine will be joined to the domain, and we will configure its DNS settings to use the Domain Controller as its DNS server.
</p>
<br />

<p>
<img src="https://i.imgur.com/d22FHIm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
DC-1 must have a static private IP address. Client-1 will connect to DC-1, and to verify connectivity, we will attempt to ping DC-1 from Client-1. Initially, the ping may fail. To resolve this, we need to enable ICMPv4 on the firewall of DC-1. After making this change, we can successfully ping DC-1 from Client-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/HvZBWzc.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/1lrrGPw.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
Next, we will log back into DC-1 to install Active Directory Users & Computers. We will promote the VM to a Domain Controller and set up a new forest named "mydomain.com." After completing this, restart the VM and log back into DC-1 using the credentials "mydomain.com\labuser." If you followed the steps correctly, you should be able to launch Active Directory Users & Computers as shown below.
</p>
<img src="https://i.imgur.com/cGjvRke.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
Great! We can now begin creating Organizational Units (OUs). First, create an OU named _EMPLOYEES and another named _ADMINS. To do this, right-click on the domain area, select New → Organizational Unit, and fill in the required fields. Once the OUs are created, click inside the _ADMINS OU, right-click, select New → User, and enter the details for your new user. Name the user Jane Doe and set her username as Jane_admin since she will be an Admin. Finally, add Jane to the Domain Admins security group.
</p>
<img src="https://i.imgur.com/hL7g5Y5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kcgvzdE.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
From now on, you can use Jane_admin as the administrator account. Next, we will join Client-1 to the domain (mydomain.com). To do this, access the Azure portal and update Client-1's DNS settings to point to the DC's private IP address. After making this change, restart Client-1 from within the Azure portal. The image below confirms that Client-1 is using DC-1 as its DNS server.
</p>
<img src="https://i.imgur.com/jbrGTXW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kvcm2cY.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<p>
To join Client-1 to the domain, navigate to your system settings and go to About. On the right side, select Rename this PC (advanced). From there, choose the option to change the domain, and enter "mydomain.com." After that, provide your credentials using mydomain.com\labuser. Your computer will restart, and upon completion, Client-1 will be successfully part of mydomain.com.
</p>
<br />
<p>
  <p>
<img src="https://i.imgur.com/Ze0Em5e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Client-1 is now a part of the domain. Next, we will set up Remote Desktop access for non-administrative users on Client-1. Begin by logging into Client-1 as an administrator and opening System Properties. Click on "Remote Desktop" and grant access to Domain Users for Remote Desktop. Once you've completed these steps, you should be able to log into Client-1 as a normal user.
</p>
<br />

<p>
  <p>
<img src="https://i.imgur.com/SApOKiE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To verify that normal users can RDP into Client-1, we will use a script to generate thousands of users in the domain. We will execute the script in PowerShell, and once the users are created, we will select one and attempt to RDP into Client-1.
</p>
<br />
<img src="https://i.imgur.com/EzWG8ug.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<p>
  <p>
<img src="https://i.imgur.com/Gkpe68K.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/n3gMwQV.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
As you can see the Powershell script created a user with the username "bab.hubo" We were able to login to Client-1 with his credentials as a normal user. 
</p>
