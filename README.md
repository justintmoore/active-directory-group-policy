<p align="center">
    <img src="https://i.imgur.com/8EzPyDZ.png" alt=osTicket logo"/>  
    
    
## Tools, Utilities, Services Used
- Microsoft Azure <br>
- Remote Desktop <br>
- Active Directory

## Environments Used
- Windows 10 <br>
- Windows Server 2022
  

## Program Walkthrough
<p align="center">
    Objective: This lab is working on the premise that we have already created our client and domain controller as instructed in the previous lab. In this lab we will go in and install Active Directory. We will also be creating an Administrator account and a normal User account within AD. After we will join our client VM, to our domain, as well as setup RDP for non-administrative users on on client VM. Lastly we will create a bunch of additional users using a powershell script and attempt to log into our client VM with one of the users. We may even reset a random users password and attempt to login with them.
</p>
<br>

<p align="center">
   First we will remote desktop into our domain controller, click the windows icon at the bottom and open up Server Manager. 
</p>

<p align="center">
    <img src="https://i.imgur.com/F2NCTYy.png" height="60%" width="60%" alt="placeholder"/>
</p>
<br> 
