# Understanding Microsoft Server Failover
<p align="center">
<img src= "https://mertcangokgoz.com/wp-content/uploads/2020/11/windows-server-2019-fhd-logo.png" alt="Windows Server Logo"/>
</p>

<h1>𝐒𝐞𝐭𝐭𝐢𝐧𝐠 𝐔𝐩 𝐌𝐢𝐜𝐫𝐨𝐬𝐨𝐟𝐭 𝐒𝐞𝐫𝐯𝐞𝐫 𝐃𝐇𝐂𝐏 𝐅𝐚𝐢𝐥𝐨𝐯𝐞𝐫<h1></h1>
I completed a lab on configuring DHCP failover between two Windows Server 2019 domain controllers. I did run into some difficulties which gave me the opportunity to use some real-world troubleshooting.</h1>
<br />


<h2>What I Accomplished</h2>

- Configured Server Manager to manage multiple servers remotely 
- Installed and configured DHCP role with custom scope
- Set up IP exclusions and lease durations
- Successfully activated the scope and verified client IP assignment

<h2>Failover Configuration </h2>
 
 - Implemented Hot Standby mode between two domain controllers
 - Configured partner server relationships with shared secrets
 - Set up maximum client lead time and standby server address allocation
</b> 

<h2>Troubelshooting</h2>
𝐓𝐫𝐨𝐮𝐛𝐥𝐞𝐬𝐡𝐨𝐨𝐭𝐢𝐧𝐠
I did come across some challenges during this lab but this was a valuable learning experience for me:

𝐂𝐡𝐚𝐥𝐥𝐞𝐧𝐠𝐞#1 𝐂𝐨𝐦𝐩𝐞𝐭𝐢𝐧𝐠 𝐃𝐇𝐂𝐏 𝐒𝐞𝐫𝐯𝐞𝐫𝐬 My Windows 10 client kept getting IP addresses I had excluded. After investigating, I discovered the firewall/router had its on DHCP server running. I had to access the M0n0wall web GUI to disable the competing service. This is a good reminder to check all devices and not just one that you are working on.

𝐂𝐡𝐚𝐥𝐥𝐞𝐧𝐠𝐞#2 𝐃𝐨𝐦𝐚𝐢𝐧 𝐂𝐨𝐧𝐟𝐢𝐠𝐮𝐫𝐚𝐭𝐢𝐨𝐧 𝐌𝐢𝐬𝐭𝐚𝐤𝐞 The biggest learning moment came trying to set up failover between servers. I had accidently created a new forest instead of adding the second server to the existing domain. This prevented the failover configuration entirely and required rebuilding the virtual machine.


<h2>Installation Steps</h2>

<h2>Step 1 </h2>
  The first stage was to create a virtual machine in Azure. I created a Resource Group then created a Windows 10 virtual machine with 2 virtual CPUs so that it had sufficient resources. This also allowed me to create a virtual network.

![Creating DC-1 copy](https://github.com/user-attachments/assets/45986734-6b76-4601-ad62-9583139061e4)

</p>


<p>

</p>
<br />

<p>



<p>
<h2> Step 2</h2>
The next stage was to enable Internet Information Services on the virtual machine. Click on start and type IIS. IIS is a crucial component for running osTicket on a Windows server. It provides the necessary web server environment for the application to function properly and interact with users through the internet. I turned CGI, Common HTTP Features and IIS Management Console. To enable IIS with CGI Control Panel > Programs and Features > Turn Windows Features on or off > World Wide Web Services > Application Development Features > CGI (check) Expand Internet Information Services. To enable IIS Management Console Internet Information Services > Web Management Tools > IIS Management Console
</p>
<br />


<p>

![Configuring ISS](https://github.com/user-attachments/assets/26df7b13-857c-42bb-bb9e-7f8be25a6235)

![Internet information service](https://github.com/user-attachments/assets/ae26d97e-91dc-4fe8-bec9-9e7f1d953cda)  ![check cgi](https://github.com/user-attachments/assets/67274c7e-21a5-4563-bc4a-b92b6ade5533)           
![Internet information service](https://github.com/user-attachments/assets/52adb8ce-038b-48b1-9c49-bca2da8931c1)![making sure all checked](https://github.com/user-attachments/assets/01f51c44-5944-47e4-8e75-7b40671bfc85)

<h2> Step 3 Download and Install PHP Manager for IIS and Rewrite Module </h2>
  
![Download PHP Manager](https://github.com/user-attachments/assets/7d405e80-f043-484a-9378-d7671d279ee4)



 PHP Manager provides a user-friendly interface for configuring PHP settingsPHP manager can be downloaded at https://www.iis.net/. I then created a new folder C:/PHP.

![Make PHP folder in C drive](https://github.com/user-attachments/assets/1f0b298c-2d97-4b0c-816b-ebe05e6eb0c8)




![image](https://github.com/user-attachments/assets/5380e24b-b1ec-43c1-8232-22bbfc4ed2b7)


![Download Rewrite Module](https://github.com/user-attachments/assets/9937e075-e212-4526-9666-762f9cc677c2)

Rewrite module in IIS (Internet Information Services) is a powerful tool that allows you to modify the URL structure of web requests before they are processed by the web server. Can be downloaded at https://www.iis.net/.



<h2> Step 4 Download MYSQL and Visual C++ </h2>

OSTicket needs a database to store users so it needs MYSQL to be installed. It can be downloaded from https://www.mysql.com/downloads/

![downloading MY SQL](https://github.com/user-attachments/assets/1373af4c-ea10-4515-a8ed-c242b0775480)


![MY SQL passowrd](https://github.com/user-attachments/assets/9b445a9a-8f8d-49cd-90c4-be69f1e4bf64)

I created a password and user name that was needed to configure database in OSTicket.


![Downloading microsoft VC ++](https://github.com/user-attachments/assets/aed6acac-0f5b-4bab-806f-bbfae0521b6a)

<h2> Step 5 Register PHP from within IIS a </h2>

Register PHP within IIS and find the PHP folder that was created earlier. Open IIS as admin.

![Enabling PHP](https://github.com/user-attachments/assets/b9e750c3-af8e-4d8a-b085-d87ecd535561)

![Finding PHP folder that we created](https://github.com/user-attachments/assets/21475646-00d2-4c6a-9f60-5fe9a8b02fff)


<h2> Step 6 Install OSTicket </h2>

Download osTicket from  https://osticket.com/
Extract and copy “upload” folder to c:\inetpub\wwwroot
Within c:\inetpub\wwwroot, Rename “upload” to “osTicket”
Reload IIS (Open IIS, Stop and Start the server)
Within IIS Go to sites -> Default -> osTicket On the right, click “Browse *:80
![Restart os ticket](https://github.com/user-attachments/assets/585605c4-ab78-46ff-b636-95d6caa1c021)






![succesfully navigated to os ticket page](https://github.com/user-attachments/assets/f3243723-efeb-49f9-b8c9-c05e96ddc862)

Go back to IIS, sites -> Default -> osTicket
Double-click PHP Manager
Click “Enable or disable an extension”
Enable: php_imap.dll
Enable: php_intl.dll
Enable: php_opcache.dll
Refresh the osTicket site in your browse, observe the changes

Rename: ost-config.php
 From: C:\inetpub\wwwroot\osTicket\include\ost-sampleconfig.php
To: C:\inetpub\wwwroot\osTicket\include\ost-config.php
Assign Permissions: ost-config.php
Disable inheritance -> Remove All
New Permissions -> Everyone -> Al

Continue Setting up osTicket in the browser (click Continue)
Name Helpdesk
 Default email (receives email from customers)
From the Installation Files, download and install HeidiSQL.
  Open Heidi SQL
  Create a new session, root/Password1
  Connect to the session
  Create a database called “osTicket”


![database set up](https://github.com/user-attachments/assets/e7a2b787-12e3-4b31-b1de-f24f108a2fd8)

Continue Setting up osticket in the browser
MySQL Database: osTicket
MySQL Username: root
MySQL Password: Password1
Click “Install Now!”


<h2> Successfully Installed! </h2>


![os-ticket successfully installed logged in](https://github.com/user-attachments/assets/b5be7cf2-e25e-4d71-ac0a-79a80adf97aa)

 <h2>Final Clean up </h2>
 Delete: C:\inetpub\wwwroot\osTicket\setup
Set Permissions to “Read” only: C:\inetpub\wwwroot\osTicket\include\ost-config.php
</p>
<p>

</p>
<br />
