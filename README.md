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

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

![image](https://github.com/user-attachments/assets/b95a6ff8-b029-4977-99b3-399a65df4601)

<p>
After setting up the virtual machines (dc-1 with Windows Server 2022 and client-1 with Windows 10), open Remote Desktop and log in to dc-1. Once logged in, the above image will come up called the Server Manager. In this step, Active Directory Domain Services will be installed. On "Server Manager," click on "Add Roles and Features." 
</p>
<br />

![image](https://github.com/user-attachments/assets/e6efca26-b442-4326-bd6c-c03b4b5ab039)

<p>
(In dc-1) Under "Add Roles and Features Wizard," click "Next." Once on "Select server roles," 1) click on "Active Directory Domain Services" and then 2) click "Next." Then, continue to "Add Features" and then "Install."
</p>
<br />

![image](https://github.com/user-attachments/assets/8c9d6655-c524-4f69-9886-951b45da5290)

<p>
(In dc-1) Back on "Server Manager," a caution sign will appear next to the flag. 1) Click on the flag 2) Then click on "Promote this server to a domain controller." 
</p>
<br />

![image](https://github.com/user-attachments/assets/5bd52278-081b-4294-be0a-d5ef055b6430)

<p>
(In dc-1) After clicking on "Promote this server to a domain controller," it will show "Deployment Configuration" with selections underneath. 1) Click on "Add a new forest." 2) Then type in a name for "Root domain name."
</p>
<br />

![image](https://github.com/user-attachments/assets/74fd5d06-f157-4082-9c64-1c5edc5a16e1)

<p>
(In dc-1) In this step, create a new user. Click on the Start menu, go and click on "Windows Administrative Tools" --> "Active Directory Users and Computers." After getting in the "Active Directory Users and Computers," create two Organizational Units (ADMINS and EMPLOYEES). Click on "_ADMINS" and then right-click and go to "New," scroll down to "User." 
</p>
<br />

![image](https://github.com/user-attachments/assets/ac50f10d-2325-4e27-b926-7c2a6b1c4dc6)

<p>
(In dc-1) In this step, will create the user by inputting the name, logon name, and on the next page password.
</p>
<br />

![image](https://github.com/user-attachments/assets/524a2b98-daf5-4ffb-a65f-e27c846ea061)

<p>
In this step, add the user as a Domain Admin (do in dc-1). 1) Double-click on the user's name. A window "(name) Properties" will pop up. Under this, go and click on "Member Of" --> "Add." 3) Type in "domain admins" then click "Check Names" --> "OK."
</p>
<br />

![image](https://github.com/user-attachments/assets/289a5450-7551-4018-b037-f1ba1822f694)

<p>
In this step, join client-1 to the domain. To do this, move to the client-1 remote desktop. Go and right-click on the Start menu and 1) click on "System" to go to System Properties. 2) Scroll down or navigate to the right, locate and click "Rename this PC (advanced)." In the "System Properties" window under "Computer Name," 3) go and click on "Change." 4) Locate and click on "Domain" to type the domain of dc-1. Then click "Ok."
</p>
<br />

![image](https://github.com/user-attachments/assets/afea8a85-d55c-4f25-aa1b-25e4e662a349)

<p>
Back in dc-1, go to "Active Directory Users and Computers" --> "mydomain.com" --> "Computers" to see client-1 joined to the domain.
</p>
<br />

![image](https://github.com/user-attachments/assets/e81afaa6-2643-4676-b34e-586898e970b1)

<p>
(In client-1) In this step, allow Domain Users to access Remote Desktop. In the System Properties, go to "Related Settings" and click on "Remote Desktop" found underneath. Under "Remote Desktop," go to User accounts and click "Select Users that can remotely access this PC." In Select Users or Groups, type "domain users" in the box below, click Check Names, and then "Ok." Now domain users can access the remote desktop.
</p>
<br />

![image](https://github.com/user-attachments/assets/f302ad60-d4db-4e82-893d-a827b9102b03)

<p>
Back in dc-1, will create users (in employees) in Powershell ISE as an admin in this step and choose a created user to log in and test. At the Start, can type or locate "Powershell ISE." 1) Right-click or click "Run as administrator" to get the window above. On that window, a file was used with code to create those users. That code was placed in the document that said "create-users." After the code was pasted on the document, 2) click the play button (once was green) to run the script. Below is a list of users being created. On the "Active Directory Users and Computers" window, click "_EMPLOYEES" to see the users populate.
</p>
<br />

![image](https://github.com/user-attachments/assets/0f814c44-1ae7-4cfe-8743-1b43ea7b1544)

<p>
In this step, will configure the Group Policy to test a user from the list of Employees failing to log in. Still in dc-1, to navigate to Group Policy Management. Click on the Start menu --> "Run" --> gpmc.msc to pull up the Group Policy Management window. In this window, go to mydomain.com and right click "Default Domain Policy" and go to "Edit." This brings up Group Policy Management Editor. From here, click "Policies" --> "Windows Settings" --> "Security Settings" --> "Account Policies" --> "Account Lockout Policy." On the right will see a list of policies to adjust clicking on one of them, changing the setting, and then "Apply" and "Ok" will change the rest of the settings. In this example, "Account lockout threshold" was changed from 0 to 5 invalid logon attempts. After adjusting the settings and clicking "Ok." Users who fail to log in will receive an error message.
</p>
<br />

![image](https://github.com/user-attachments/assets/3f2beedc-8c52-4eb3-b791-112c6785a2c1)

<p>
In this step will see the result from changing the Group Policy.
</p>
<br />

![image](https://github.com/user-attachments/assets/4ea5c63e-0b0d-4ef4-bed4-54d592ecfb81)

<p>
In this final step, a user named "caw.noki" fails to log in and receives an error message. On the admin's side (dc-1), can unlock their account by finding their name from the _EMPLOYEES list. Can click to show "(name) Properties." Then go to the account and check the box to Unlock the account under this user. To reset password instead, 1) can right-click the user's name to see options. 2) Clicking on the option "Reset Password" will lead to a Reset Password window. There, 3) can input a new password and 4) unlock the user's account. Once click "Ok," the user can sign in with the new password.
</p>
<br />
