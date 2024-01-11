<p align="center">

![Microsoft_Azure-Logo wine](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/0cc402a9-a0f7-4889-9303-65eade1ac965)


</p>

<h1> Creating Virtual Machines </h1>
This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>

<p>

![Screenshot (155)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/978d348f-0437-4c81-b076-603e38f85fef)



![Screenshot (157)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/c0269eed-1a60-4856-bcc5-67e518c20647)


</p>
<p>
First step is to have an Microsoft Azure subscription active to create resource groups in. Then we will create a resoucre group that the virtual machine will be connected to it will be named VM1.
</p>
<br />

<p>

![Screenshot (158)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/4b6b8427-8266-4571-99d1-cd598b4a4454)



![Screenshot (160)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/504955f2-cd3c-4034-8d42-6b85c8e6be76)



</p>
<p>
  
 Next we will create another virtual machine and we will name it VM2
  
</p>
<br />

<p>

![Screenshot (161)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/7d7611e1-088e-4ac8-b0c3-895a3a5a1b30)


  
</p>
<p>
Make sure that that VM1 and VM2 are sharing the same VNET.
</p>
<br />

![Screenshot (163)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/3913e1f1-33af-421c-b274-e99ab49d0f35)


Next copy the public IP address of VM1 and open up Remote Desktop.


![Screenshot (165)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/ebc2fc7f-627b-433c-ada2-e68654efb09c)


With the credentails that you used  when setting up the VM1, imput the IP address and password and long in to RDT


![Screenshot (166)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/5419245b-54e4-4011-8e8a-3d2acb7f1053)


Next once the home screen loads, open up the browser and download wireshark


![Screenshot (167)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/0a90d2ac-e8ac-44c0-a75d-80b0a8cafe67)


Go to the start menu and look up wireshark and run the instalation.


![Screenshot (168)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/01c52a12-3534-4042-bd38-4049330e1d4e)


![Screenshot (169)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/f66756d0-2251-4209-9cba-97907aa07252)


Click on next to continue running the program



![Screenshot (170)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/22dabd2d-6fc8-4dd1-a2cb-b055301ab2b3)


Once wireshark opens, click on the shark fin in the top left corner




![Screenshot (171)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/9183a0c4-008c-489b-a0b7-64b77b2096bc)


Once you are able to see inbound traffice through wireshark, click in to the address bar and type ICMP and hit enter.


![Screenshot (172)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/8f55ddfb-2e4d-4626-abbe-fcb15959137d)


Once you have only allowed CIMP traffic only, go to the search bar and type in powershell and hit enter, Powershell should open up.


![Screenshot (174)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/78791a26-2290-49c5-921b-3c5cc16750bf) 


Next inside powershell type "ping 10.0.0.5" which is VM2's private IP address to see if we get any traffic response.



![Screenshot (175)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/75de383b-1d03-4ba9-bad4-29f06c713b2b)


![Screenshot (176)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/68f5dd85-436e-4c2c-aad4-f731035d074e)


Next go back to Microsoft Azure and open up VM2's Network Settings, Then click on create port rule, then select Inbound rule.


![Screenshot (177)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/8df1fc94-85d5-41a6-b1c4-9a100538fbe2)


![Screenshot (178)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/af9fe199-3aa9-4a3f-b90f-d8ad020fa1cf)


Leave everything the same but change the protocol to ICMP, Action to Deny, Prioty 200, and name to DENY_ICMP_PING_FROM_ANYWHERE then click add



![Screenshot (180)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/df41f2ca-a258-44e2-bc9e-5fa8803f1cf0)


Going back to RDT type in "ping -t 10.0.0.5" this will start an unstopping ping but the request should be timed out due to us blocking the incoming icmp traffic.


![Screenshot (184)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/e4977b91-637f-4faf-977e-e8d623c444a9)

Next we go back to home computer screen and go back to VM2's network settings and we allow imbound ICMP traffic


![Screenshot (185)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/271d446c-2394-4158-979c-0223a680e0b2)


And to verify this we will go back to RDT and notice the traffic in wireshark

![Screenshot (186)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/c5cb1ae4-224a-4f33-a3da-61b375b76eac)



![Screenshot (187)](https://github.com/JoshuaMoorecc/configure-ad-/assets/154629831/2b484e8f-1de0-4802-a48e-e1cfa414752a)

























