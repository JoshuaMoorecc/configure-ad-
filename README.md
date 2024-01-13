<p align="center">

![active-directory-logo](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/45a67b0d-c885-41bf-a466-ec8e134ed436)


</p>

<h1> On-premises Active Directory Deployed in the Cloud (Azure) </h1>
This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>

<p>



![Screenshot (199)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/640402a1-f2b0-4a2e-b303-ae25faed5daf)




</p>
<p>
  
In Microsoft Azure, create two virtual machines that run on Windows 10 (Client-1) and Windows Server (Domain Controller or DC). In the virtual machines, I made sure to keep the region, virtual machine size, and virtual network (vnet) were the same to ensure that both machines are running on the same resources to communicate and drive traffic.


</p>
<br />

<p>

![Screenshot (205)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/09462f5b-3850-4d69-9d61-9d0d2980652a)


</p>
<p>

After creating the virtual machines, change the Domain Controller's (DC) Network Interface (NIC) to static inside of Microsoft Azure. I have toggled to Networking under the DC-virtal machine --> Click on the virtual NIC --> Click on IP configurations --> Edit the assignment to "Static" --> Click on Save. Changing the NIC to static allows the IP address to stay the same and not change if a computer pulls the IP address.



</p>
<br />

<p>


![Course - CourseCareers - Opera 1_12_2024 12_54_23 PM](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/b1551fe1-3d2e-4351-b99a-c76dfef242d8)


  
</p>
<p>
After creating both machines, this will be the topology to showcase how both machines were running from the same Vnet and subnet mask. Also, we can see that each virtual machine has its own IP addresses and network security groups.
</p>
<br />




<h1> Reviewing Connectivity between DC and Client 1 </h1>

![Screenshot (210)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/d5230fb1-2d30-4ee4-9b8f-1e2f5d9f0d76)

![20 150 223 237 - Remote Desktop Connection 1_11_2024 3_34_47 PM](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/75c09921-de75-4507-95f8-6758d0922ea6)





After logging into the DC and Client 1, complete a perpetual ping to DC's prviate IP address on Client 1 to observe the connectivity. The connectivity has failed due to DC's firewall blocking the ICMP protocols. Thus, go back to the DC remote desktop and enabled the ICMP4s protocols for "Echo Request" for the pings to respond. After enabling the ICMP4 protocol, toggle back to Client 1 to review that the ping to DC's private IP address was successful.


<h1> Install Active Directory </h1>

![20 169 104 33 - Remote Desktop Connection 1_11_2024 3_37_35 PM](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/fb6dff8d-b83e-410c-8af7-b836ad7d92e3)


After reviewing the connectivity between both machines, install Active Directory on the domain controller by opening Server Manager --> Clicking on "Tools" --> Clicking on "Active Directory Users and Computers" --> Clicking on "Add Roles and Features" and starting the setup wizard. Under "Server Roles", select "Active Directory Domain Services" and complete the remainder of the setup wizard. Once the installation is complete, the notifications flag will have a hazard notification next to it.



<h1> Continue Installing Active Directory </h1>



![20 169 104 33 - Remote Desktop Connection 1_11_2024 3_43_29 PM](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/b471f045-be68-40dc-b28b-87676ee37b84)



After clicking on the notification flag, click on "Promote this server to a domain controller" to continue setup of the Active Directory. Next in the Deployment Confirguration Wizard, select "Add a new forest" and create a domain name. Continue to install and setup in the configuration wizard and ensure to allow the "NETBIOS" to load with previously created domain name. After the installation is complete, DC will restart itself and sign back in with DC VM login credentials.



<h1> Create Administrator and User Accounts in Active Directory </h1>

![20 169 104 33 - Remote Desktop Connection 1_11_2024 4_06_31 PM](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/8cf3d19c-67d8-4618-b41f-19dcc7a33573)



After logging in, open Server Manager and toggle to "Active Directory Users and Computers" --> right-click under "domain.com" --> Click on "New" and "Organizational Unit" to create "ADMINS" and "EMPLOYEES folders in Active Directory. Once the "Admin" folder is created, right-click on "New" and "User" to add "Jane Doe" to user group.



<h1> Continue creating Administrator and User Accounts in Active Directory </h1>


![20 169 104 33 - Remote Desktop Connection 1_11_2024 4_19_47 PM](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/0c513a1a-2e33-47bc-856c-c73f11cff82a)



Once "Jane Doe has been created, add "Jane Doe" to the "Domain Admins" group to allow access to login as an administrator. Next, log out and login back in as "Jane Doe" under "domain.com\createdjanedoeuser and use the account to continue to join client 1 with the DC.


<h1> Join Client 1 to Domain Controller </h1>

![Screenshot (216)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/ab625d9b-34ec-4510-8708-701d4ebe4ea8)


After logging back in DC as "Jane Doe", toggle back to DC in Microsoft Azure portal and change the DNS server settings. First, copy the DC's private IP address. Click on the Client Virtual Machine and click on "Networking" --> Click on the virtual NIC --> Click on DNS server --> Click on "Custom" and paste DC's private IP address --> Click on "Save. Next, Client 1 will restart itself and log back in to Client 1.


<h1> Continue joining Client 1 to Domain Controller </h1>

![20 150 223 237 - Remote Desktop Connection 1_11_2024 4_50_39 PM](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/f44ed9eb-9953-4174-a986-03eb9628fe43)


Next, remote desktop into Client 1 and open System properties. Right-click on "Windows" --> click on System --> Select "Rename this PC", --> click on "Change" for "Rename this computer or change its domain" --> Select "domain", add "domain.com", and select "OK" to add domain. Next, verify that Client 1 displays under "Computers" folder in Server Manager on the Domain Contoller.


<h1> Setup Remote Desktop to Non-Administrative Users </h1>

![20 150 223 237 - Remote Desktop Connection 1_11_2024 4_58_19 PM](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/7ca1d941-727d-420a-9f56-72e9cd2a2c36)



Next, we will add non-administrative users to remotely log in and gain access on other computers. Right-click on "Windows" --> click on System --> Select "Remote Desktop", --> click on "Add", type in "domain users", and click on "Check Names" --> Select "OK". Next, toggle back to DC as "Jane Doe" login


<h1> Create Additional Users </h1>

![20 169 104 33 - Remote Desktop Connection 1_11_2024 5_11_30 PM](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/f67ca4e2-f2f6-4230-8008-9a2e83753045)


After logging into Dc, open and run Powershell ISE as an administrator. Copy and paste created script to create additional users in the employees file under Active Directory. After pasting the script, select "RUN" (green play notification) to start the creation of users. Also, the powershell script will create a number of random user names and that will use the login credential of any password that is set.


<h1> Logging in as a User </h1>

![20 150 223 237 - Remote Desktop Connection 1_11_2024 5_17_07 PM](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/e254de3b-6a2d-48c6-9224-b260f10bcbeb)


After the additional names are created, we can login as a user and verify that the client computer provides access to non-administrative users.
Once this has been verified, Congratulations !! You have successfully set up and administered Active Directory and set up ou, groups and users. 




























