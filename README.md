<p align="center">
    <img src="https://i.imgur.com/8EzPyDZ.png" alt=osTicket logo"/>  
    
    
## Tools, Utilities, Services Used
- Microsoft Azure <br>
- Remote Desktop <br>
- Active Directory
- Event Viewer

## Environments Used
- Windows 10 <br>
- Windows Server 2022
  

## Program Walkthrough
<p align="center">
    Objective: Compared to the other labs, this one should be considerably shorter. We are going to simulate account lockouts, enabling and disabling accounts, as well Observing logs in the Domain Controller, and on our client VM. For this lab "dc-1" will refer to our Windows Server 22' VM that acts as our Domain Controller. "client-1" will refer to our Windows 10 VM, that will act as our client device.
</p>
<br>

<p align="center">
   To start we will configure lockout policy in AD, using Grou Policy Management. In the search task bar, search GPMC.msc, which stands for Group Policy Managment Console. It will open up a Group Policy Management screen. Drop down "Forestmydomain.com" -> Drop down "Domains" -> Drop down "mydomain.com" -> Click "Default Domain Policy". When you click it, you'll receive a notification basically stating that all changes made to this Group Policy Object (GPO) are globally linked. So if this GPO is linked to multiple different domains, the rules will apply to all.
</p>

<p align="center">
    <img src="https://i.imgur.com/dKxbM8Y.png" height="60%" width="60%" alt="placeholder"/>
</p>
<br> 
    
<p align="center">
  Right click Default Domain Policy -> Edit -> Computer Configuration -> Policies -> Windows Settings -> Security Settings -> Account Policy -> Account Lockout Policy. The policy settings are as follows...
</p>

<p align="center">
    <img src="https://i.imgur.com/W43lN1M.png" height="60%" width="60%" alt="placeholder"/>
</p>
<br> 

<p align="center">
    Lockout Duration: Time in minutes that an account remains locked before it's automatically unlocks.<br>
    Lockout Threshold: The number of failed logon attempts that will trigger an account lockout.<br>
    Allow Admin Account Lockout: Determines if Admin is able to be locked out or not.<br>
    Reset Account Lockout Counter After: The time in minutes after which the failed logon attempts counter is reset to 0.<br>
</p>

<p align="center">
    To change the policy, double click the Policy name, and make your changes. For this lab my changes are:<br>
    Lockout Duration: 30 Minutes.<br>
    Lockout Threshold: 5 Invalid logon attempts.<br>
    Allow Admin Account Lockout: Disabled.<br>
    Reset Account Lockout Counter After: 10 Minutes.<br>
    You can double check your configurations in the Group Policy Mangement Console. To do so, click your Default Domain Policy, go to Settings, and scroll down to "Account PoliciesAccount Lockout Policy".
</p>

<p align="center">
    <img src="https://i.imgur.com/7nAgdPX.png" height="60%" width="60%" alt="placeholder"/>
</p>
<br> 

<p align="center">
   It can take some time for the updates to take affect automatically, or you can force an update immediately by opening up your command prompt and useing the command "gpupdate /force". This will forcefully update Group Policy changes.<br>
    To do this lets login to our client-1 using our jane_admin account. Open up command prompt, and type "gpupdate /force".
</p>

<p align="center">
    <img src="https://i.imgur.com/NQZqJKp.png" height="60%" width="60%" alt="placeholder"/>
</p>

<p align="center">
    If successful, you should see that Computer Policy and User Policy has been updated. 
</p>

<p align="center">
    <img src="https://i.imgur.com/xwU02iB.png" height="60%" width="60%" alt="placeholder"/>
</p>
<br>

<p align="center">
   To test if the Group Policy configs were taken, lets go to dc-1. Open up Active Diretory. At the top we will click the magnifying glass, to find objects. We will change the find settings to: Find "Users, Contacts, and Groups" in "mydomain.com". This indicated we will search for every user and group within our domain. Type the letter "g", and click Find Now. This will pull up all users that start with the letter "g". Pick one by random, double click, and get their display name. We will use this User to simulate a failed login scenario.
</p>

<p align="center">
    <img src="https://i.imgur.com/XBeKGKV.png" height="60%" width="60%" alt="placeholder"/>
</p>
<br> 
   
<p align="center">
    We need to logout of our current user to client-1. Remember from our last lab, all user passwords are "Password1". So next we will open up Remote Desktop and attempt to log into client 1 with one of our users, using the wrong password. Ensure you are using the "domain name \ username" format.
</p>
<p align="center">
    After 5 unsuccessful attempts, our 6th attempt resulted in a lockout. This proves that are GP configs are running! To test this out further, after 10 minutes we should be able to login with another 5 attempts.
</p>

<p align="center">
    <img src="https://i.imgur.com/80idWxe.png" height="60%" width="60%" alt="placeholder"/>
</p>
<br> 

<p align="center">
    As the administrator, this is the most common issues faced within the workplace. In the event of a account lockout, the admin has permission to reset the account, and that's what we'll do. Go back to dc-1 and serach for the account that was locked. Right click the user -> Properties -> Account -> Check the box that states, Unlock account -> Apply -> OK.
    Now if you try to log back into client-1 as that user, you will no longer be locked out.
</p>

<p align="center">
    <img src="https://i.imgur.com/l6gQYTJ.png" height="60%" width="60%" alt="placeholder"/>
</p>
<br>

<p align="center">
   If a user gets their account locked out, there's a good chance that they have forgotten their password. As admins we can help them out by resetting their password. There are  two ways of doing this. If a user is in person, we can set the new password directly. If that is not possible we can Unlock their account, and prompt their device to change their password upon next logon.
</p>

<p align="center">
   Reset locally
</p>
<p align="center">
    <img src="https://i.imgur.com/D5Ep2sA.png" height="60%" width="60%" alt="placeholder"/>
</p>

<p align="center">
    Prompt Password change upon next logon.
</p>
<p align="center">
    <img src="https://i.imgur.com/hPo6vLk.png" height="60%" width="60%" alt="placeholder"/>
</p>
<br>

<p align="center">
    Next, we will disable and enable a user account. This would come in handy in a situation where an employee no longer needs access and permissions to resources. This follows the concept of Principle of Least Privilige. If a person doesn't need the access, even if temporary, they should not have it.
</p>

<p align="center">
    Find your user in AD -> Right click the user ->select disable account. Upon next login, they will recieve an error notification
</p>

<p align="center">
    <img src="https://i.imgur.com/2KJb3Gc.png" height="60%" width="60%" alt="placeholder"/>
</p>

<p align="center">
    <img src="https://i.imgur.com/NNukuVo.png" height="60%" width="60%" alt="placeholder"/>
</p>
<br>

<p align="center">
    To reinstate that user, we simply follow the same steps except this time, we select enable account.
</p>

<p align="center">
    <img src="https://i.imgur.com/Na3HdNQ.png" height="60%" width="60%" alt="placeholder"/>
</p>
<br>

<p align="center">
    Lastly, we will observe logs within the dc-1. What we should see is login attempts, failed logins, etc. In the search taskbar, search "event viewer". This is where you can observe, analyze, different event logs happening within your system. They are split into categories such as application, security, setup, system, etc.
</p>

<p align="center">
   If you go into Security, you can see login attempts, and failures. Each alert corresponds to an Event ID, specific to Windows environments. 
</p>

<p align="center">
    <img src="https://i.imgur.com/il3TqDL.png" height="60%" width="60%" alt="placeholder"/>
</p>
<br>

<p align="center">
     For example EID 4625 represents Failed logon attempt, this could give insight on whether or not a device is compromised by showing multiple failed logins within a certain time frame, certain time of the day etc.
</p>
<br>

<p align="center">
     EID 4673 represents Actions taken with Elevated Priviliges. This can include logging in as an admin user, running applications as administrator, etc. You can see details of the event by looking at the bottom portion of the event viewer application. Details include, the service ran, processes, account names, etc.
</p>

<p align="center">
    <img src="https://i.imgur.com/kQQff29.png" height="60%" width="60%" alt="placeholder"/>
</p>
<br>

<p align="center">
    If you're investigating an incident Event Viewer can be a pricless tool. For example, you notice a string of failed login attempts. You can filter by Event ID, by right clicking the Log file you want to search, select Filter Current Log, and specify the EID you are investigating.
</p>

<p align="center">
    <img src="https://i.imgur.com/GVGS3Lc.png" height="60%" width="60%" alt="placeholder"/>
</p>
<br>

## Lessons Learned
<p align="center">
    As an IT Technician I have some hands on experience with Active Directory, but without Admin privileges, most of what I do is surface level. This what a good refresher in some aspects such as resetting passwords, and searching for objects within AD. I now have a good understanding of admin responsibilities in dealing with account lockouts, and the processes that deal with them. I gained new knowledge in observing Event Viewer Logs, andfrom a security standpoint how effective a tool this can be. Utilizing Azure continuously I am building a working knowledge of the architecture that is behind the name. This lab is just a foundation, I plan to explore further, and recreate scenarios I could face in the workplace. The Marathon Continues!
</p>
<br>
<p align="center">
    <strong>The Marathon Continues!</strong>
</p>
