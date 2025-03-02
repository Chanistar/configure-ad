<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-Prem Active Directory Implemented in the Cloud (Azure)</h1>
This guide outlines the installation and configuration of on-premises Active Directory within Azure Virtual Machines.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Creating and setting up the Virtual Machines.
- Testing and Observing connectivity.
- Installing Active Directory Domain Services. 
- Adding clientele to the Domain.
- Allowing "domain users" access to Remote Desktop.
- Configuring Active Directory and Group Policy.

<h2>Deployment and Configuration Steps</h2>

<p>

Create two Virtual Machines in the same resource group, Network, and Subnet:                                                                                                                                          
    One as a server, and one as a standard virtual machine used as a client.

![image](https://github.com/user-attachments/assets/4f397fc5-0538-41b0-bef7-9a702a0a2719)



After creating the DC-VM, the Domain Controller’s NIC private IP address is set to static. A Domain Controller's IP address must remain static to ensure that other devices can reliably locate it for authentication and resource access. A dynamic IP address could cause communication failures, interrupting domain services and network functionality.
</p>
<p>

</p>
<br />

<p>

![image](https://github.com/user-attachments/assets/af110f0c-5f84-4bbc-a111-a617454c7073)


After creating the Client-VM, configure its DNS settings to point to the DC-VM's Private IP address.
</p>
<p>
</p>
<br />

<p>


![image](https://github.com/user-attachments/assets/74c4cf14-d2c2-40ff-a991-b9cbe1b0381b)

  Temporarily Disabling the Windows firewall to test connectivity in the DC-VM.

![image](https://github.com/user-attachments/assets/c15f5aec-52a1-45c5-adee-b14e2a4dfc50)

</p>
<p>
Observing the communication between the two virtual machines after pinging the DC-VM from the Client-VM.
<br />

![image](https://github.com/user-attachments/assets/f6778708-3042-4750-90f6-71cec247eda5)

 After running the command ipconfig /all, we can see that the DNS Server's private IP address is now that of the DC-VM and not the usual Azure private IP address. 


![image](https://github.com/user-attachments/assets/d79108c8-613a-4df5-96e1-07d8aeb91f68)

Installing Active Directory.
Active Directory (AD) is a directory service that stores and organizes network information like users, groups, and computers.

![image](https://github.com/user-attachments/assets/debbb524-547d-4678-bfb5-4e73a95e64eb)




Creating a forest in Active Directory. (A Forest is a top-level container that holds one or more domains, sharing a common schema and global catalog for centralized management and security.)

![image](https://github.com/user-attachments/assets/b8ed06bd-7a9f-4ca2-a8af-14b785ac2860)



Joining the Client-VM to the Domain.




![image](https://github.com/user-attachments/assets/305ce9a7-43d8-484e-98da-056bcf89e031)





Setting up Remote Desktop for non-administrative users on the Client-VM.                                                                                                                                            
             Now, it is possible to log into the Client-VM as a normal, non-administrative user.





![image](https://github.com/user-attachments/assets/ace37d43-b6af-493f-9875-c9c9045ec8c7)




Creating multiple additional users using PowerShell as an administrator.
([https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1](url))



![image](https://github.com/user-attachments/assets/6f17bcc6-93a0-4259-9e0d-823838753689)




Configuring Group Policy of Active Directory: Locking out the account after five failed login attempts.





![image](https://github.com/user-attachments/assets/39b76fb1-597b-49b6-8b78-b0d668875840)




Resetting passwords from within Active Directory for locked-out accounts.

