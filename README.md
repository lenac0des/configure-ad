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
In this lab, we will create two VMs in the same VNET. One will be a Domain Controller; the other will be a Client machine. We will change the DC to a static IP because it's offering Active Directory services to the client machine. The client machine will be joined to the domain. We will control the DNS settings on the client machine; the client machine will use the DC as its DNS server.

</p>
<p>
<img src="https://github.com/lenac0des/configure-ad/assets/71302899/3d6d2658-2815-4a10-b786-acd0c7354bd5"
 height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
DC-1 has to have a static Private IP Address. Client one will connect to DC-1 to ensure connectivity; we will try to ping DC-1 from Client-1. At first, the ping will not work correctly. We have to enable ICMPv4 on the firewall on DC-1. Now, we can ping DC-1 successfully from Client-1.
</p>
<br />

<p>
<img src="https://github.com/lenac0des/configure-ad/assets/71302899/917bd5a2-f4e3-4d05-a4a1-8799d6af468a" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/lenac0des/configure-ad/assets/71302899/d75bead5-1e72-4462-af95-dd23e526626f" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
We will log back into DC-1 to install AD Users & Computers. Promote the VM to DC, set up a new forest as "mydomain.com" afterward restart, then log back into DC-1 as user: "mydomain.com\labuser". If you performed the steps properly, you should be able to run AD Users & Computers, as shown below
</p>
<br />

<p>
<img src="https://github.com/lenac0des/configure-ad/assets/71302899/05c17223-00ce-498b-a892-93968dfc0c95" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Excellent! We can start creating Organizational Units (OU). Let's first create an OU named _EMPLOYEES. Create another OU named _ADMINS. In order to do that, right-click on the domain area. Select new->Organizational Unit and fill out the field. Then click inside of your OU and right-click, select new and select user, and fill out the information for your new user. The user should be named Jane Doe. She is going to be an Admin, so her username will be Jane_admin. Lastly, add Jane to the domain admins' security group.
</p>
<br />

<p>
<img src="https://github.com/lenac0des/configure-ad/assets/71302899/9ecf3e65-6a44-4790-9b63-6c92b57c3eee" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/lenac0des/configure-ad/assets/71302899/f8a2b892-d90f-4a78-b059-4e7d6a0570ad" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
From now on, you can use Jane_admin as the administrator account. Now, we will join Client-1 to the domain (mydomain.com) from the Azure portal and change client-1's DNS settings to the DC's Private IP address. After you do that, restart Client-1 from within the Azure portal. Our picture below shows verification that client-1 is on the DC-1 DNS.
</p>
<br />

<p>
<img src="https://github.com/lenac0des/configure-ad/assets/71302899/05028f38-c5cb-4ea8-961b-1a07c02adbfe" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/lenac0des/configure-ad/assets/71302899/d593167a-b5b1-49a2-95af-ff683724549f" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To do so, we have to join Client-1 to the domain, navigate to your system settings, and go to about. Off to the right, select Rename this PC (advanced). From there, select to change the domain. Enter "mydomain.com." After that, enter your credentials from mydomain.com\labuser. Your computer will restart, and then client-1 will be a part of mydomain.com.
</p>
<br />

<p>
<img src="https://github.com/lenac0des/configure-ad/assets/71302899/6282af6e-96df-49f3-8f26-bf886e54de66" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Wonderful Client-1 is now a part of the domain. Now, we will set up a remote desktop for non-administrative users on Client-1. We have to log into Client-1 as an admin and open system properties. Click on "Remote Desktop" and allow "domain users" access to the remote desktop. After completing those steps, you should be able to log into Client-1 as a normal user.
</p>
<br />

<p>
<img src="https://github.com/lenac0des/configure-ad/assets/71302899/2dc5a58c-7c26-431d-b51e-e5d74ca6d6cb" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly, to verify that normal users can RDP into Client-1, we will use a script to generate thousands of users into the domain. We will input the script in Powershell; after the users are created, we will select one and RDP into Client-1.
</p>
<br />

<p>
<img src="https://github.com/lenac0des/configure-ad/assets/71302899/8eb566b5-c824-4ac3-ac5c-73a15465b107" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/lenac0des/configure-ad/assets/71302899/6d1be1ac-da88-4e7a-9621-34cc621c9d71" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/lenac0des/configure-ad/assets/71302899/9d8f2327-88a0-4161-a78b-02d0943a057c" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
As you can see, the Powershell script created a user with the username "bab.hubo" We were able to log in to Client-1 with his credentials as a normal user.
</p>
<br />
